---
title: 'Recuperación de bosque de AD: volver a implementar los controladores de DC restantes'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: fbab907c5624a76540ab6a28c568afbd9192c028
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390248"
---
# <a name="ad-forest-recovery---redeploy-remaining-dcs"></a>Recuperación de bosque de AD: volver a implementar los controladores de DC restantes

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Los pasos hasta este punto se aplican a todos los bosques: Busque una copia de seguridad válida para cada dominio, recupere los dominios de forma aislada, vuelva a conectarlos, restablezca el catálogo global y limpie. En el paso siguiente, volverá a implementar el bosque. La manera de hacerlo dependerá en gran medida del diseño del bosque, los contratos de nivel de servicio, la estructura del sitio, el ancho de banda disponible y otros muchos factores. Tendrá que diseñar su propio plan de reimplementación en función de los principios y las sugerencias de esta sección, de la forma que mejor se adapte a sus necesidades empresariales.  
  
El siguiente paso es instalar AD DS en todos los controladores de sesión que estaban presentes antes de que se realizara la recuperación del bosque. Si los DC todavía existen, el servicio AD DS tendrá que quitarse forzosamente o se pueden volver a instalar los controladores de DC. No se pueden volver a usar las copias de seguridad existentes de estos DC, porque los metadatos correspondientes se han quitado durante la recuperación del bosque. En un entorno poco complicado, este proceso de reimplementación puede ser tan sencillo como volver a conectar los controladores de red recuperados a la red de producción y promover nuevos controladores de red según sea necesario.  
  
En una empresa de gran tamaño que se enfrenta a una infraestructura mundial, se necesita un plan más sofisticado. La primera fase suele ser restaurar AD como servicio. Esto significa que debe instalar los controladores de DC colocados estratégicamente de forma que todas las divisiones y aplicaciones críticas de negocio puedan empezar a funcionar de nuevo. Puede ser aceptable que las sucursales hayan reducido temporalmente el rendimiento como resultado. Como segunda fase, todos los controladores de DC restantes y menos críticos se reimplementan.  
  
 Existen dos métodos para instalar controladores de DC adicionales, los cuales se pueden automatizar:  
  
- Clonación  
   - En entornos virtualizados que ejecutan Windows Server 2012, la clonación es la forma más rápida y sencilla de recuperar un gran número de controladores de sistema. Puede automatizar la recuperación de todos los controladores de dominio virtualizados en un dominio después de restaurar un único DC virtualizado desde la copia de seguridad.  
   - Para obtener más información sobre la clonación y los requisitos previos, vea [Introducción a la virtualización de Active Directory Domain Services (AD DS) (nivel 100)](https://technet.microsoft.com/library/hh831734.aspx).  
- Vuelva a instalar AD DS mediante Windows PowerShell en servidores que ejecutan Windows Server 2012 (o Dcpromo. exe en servidores que ejecutan versiones anteriores de Windows Server) o mediante la interfaz de usuario.  
   - Para agilizar la reinstalación de AD DS, puede usar la opción instalar desde medios (IFM) para reducir el tráfico de replicación durante la instalación. Para obtener más información acerca del uso del comando **Ntdsutil IFM** para crear medios de instalación, consulte [instalación de AD DS desde un medio](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  

Tenga en cuenta los siguientes puntos adicionales para cada DC de réplica recuperado en el bosque mediante la clonación de controladores de dominio virtualizados o mediante la instalación de AD DS (en lugar de restaurar desde la copia de seguridad):  
  
- Todo el software de un controlador de dominio que se usa como origen para la clonación debe ser capaz de clonarse. Las aplicaciones y los servicios que no se pueden clonar deben quitarse antes de iniciar la clonación. Si no es posible, se debe elegir un controlador de dominio virtualizado alternativo como origen.  
- Si clona controladores de dominio virtualizados adicionales desde el primer DC virtualizado que se va a restaurar, el controlador de dominio de origen deberá cerrarse mientras se copia su archivo VHDX. Después, deberá estar en ejecución y disponible en línea cuando se inicien por primera vez los controladores de seguridad virtuales de clonación. Si el tiempo de inactividad requerido por el apagado no es aceptable para el primer controlador de dominio recuperado, implemente un controlador de dominio virtualizado adicional instalando AD DS para actuar como origen de la clonación.  
- No hay ninguna restricción en el nombre de host del controlador de dominio virtualizado clonado o del servidor en el que desea instalar AD DS. Puede usar un nuevo nombre de host o el nombre de host que estaba en uso previamente. Para obtener más información acerca de la sintaxis de nombre de host DNS, vea [crear nombres de equipos DNS](https://technet.microsoft.com/library/cc785282.aspx) ([https://go.microsoft.com/fwlink/?LinkId=74564](https://go.microsoft.com/fwlink/?LinkId=74564)).  
- Configure cada servidor con el primer servidor DNS del bosque (el primer DC que se restauró en el dominio raíz) como servidor DNS preferido en las propiedades TCP/IP de su adaptador de red. Para obtener más información, vea [configurar TCP/IP para utilizar DNS](https://technet.microsoft.com/library/cc779282.aspx).  
- Vuelva a implementar todos los RODC en el dominio, ya sea mediante la clonación de DC virtualizado si se implementan varios RODC en una ubicación central, o mediante el método tradicional de recompilarlos quitando y reinstalando AD DS si se implementan individualmente en ubicaciones ubicadas aisladas. como sucursales.  
   - La regeneración de los RODC garantiza que no contengan objetos persistentes y puede ayudar a evitar que se produzcan conflictos de replicación posteriormente. Cuando quite AD DS de un RODC, *Elija la opción para conservar los metadatos de DC*. El uso de esta opción conserva la cuenta krbtgt del RODC y conserva los permisos para la cuenta de administrador de RODC delegada y el Directiva de replicación de contraseñas (PRP), y evita tener que usar credenciales de administrador de dominio para quitar y volver a instalar AD DS en RODC. También conserva los roles de servidor DNS y catálogo global si están instalados en el RODC originalmente.  
   - Al volver a generar los controladores de DC (RODC o DC grabables), es posible que haya aumentado el tráfico de replicación durante la reinstalación. Para ayudar a reducir el impacto, puede escalonar la programación de las instalaciones del RODC y puede usar la opción instalar desde medios (IFM). Si usa la opción IFM, ejecute el comando **Ntdsutil IFM** en un controlador de dominio grabable en el que confíe para que esté libre de datos dañados. Esto ayuda a evitar que aparezcan posibles daños en el RODC una vez completada la reinstalación de AD DS. Para obtener más información acerca de IFM, consulte [instalación de AD DS desde un medio](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  
   - Para obtener más información acerca de cómo volver a generar los RODC, consulte [eliminación y reinstalación de RODC](https://technet.microsoft.com/library/cc835490\(WS.10\).aspx).  
- Si un controlador de dominio estaba ejecutando el servicio servidor DNS antes de que el bosque funcione correctamente, instale y configure el servicio servidor DNS durante la instalación de AD DS. De lo contrario, configure los clientes DNS anteriores con otros servidores DNS.  
- Si necesita catálogos globales adicionales para compartir la autenticación o la carga de consultas de usuarios o aplicaciones, puede Agregar el catálogo global al controlador de dominio virtualizado de origen antes de la clonación o puede convertir un controlador de dominio en un servidor de catálogo global durante la instalación de AD DS.  
  
## <a name="next-steps"></a>Pasos siguientes

- [Recuperación del bosque de AD: requisitos previos](AD-Forest-Recovery-Prerequisties.md)  
- [Recuperación de bosque de AD: diseño de un plan de recuperación de bosque personalizado](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperación del bosque de AD: identificación del problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperación del bosque de AD: determinar cómo recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperación de bosque de AD: realizar la recuperación inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)  
- [Recuperación de bosque de AD: preguntas más frecuentes](AD-Forest-Recovery-FAQ.md)  
- [Recuperación de bosque de AD: recuperación de un solo dominio en un bosque de varios dominios](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Recuperación de bosque de AD: recuperación de bosque con controladores de dominio de Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)
