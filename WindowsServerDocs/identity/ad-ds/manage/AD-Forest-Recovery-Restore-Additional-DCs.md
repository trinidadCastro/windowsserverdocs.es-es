---
title: Recuperación de bosques de AD - Redeploy restantes controladores de dominio
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: af9ec02a480b35e573edebcdd5928451174b6c1d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812006"
---
# <a name="ad-forest-recovery---redeploy-remaining-dcs"></a>Recuperación de bosques de AD - Redeploy restantes controladores de dominio

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Los pasos hasta este punto se aplican a todos los bosques: encontrar una copia de seguridad válido para cada dominio, recuperar los dominios de forma aislada, vuelva a conectarlos, restablezca el catálogo global y limpiar. En este paso, volverá a implementar el bosque. La manera de hacerlo dependerá en gran medida el diseño de bosque, los contratos de nivel de servicio, estructura del sitio, ancho de banda disponible y muchos otros factores. Debe diseñar su propio plan de implementación según los principios y sugerencias en esta sección, de forma que se adapta mejor a sus requisitos empresariales.  
  
El siguiente paso es instalar AD DS en todos los controladores de dominio que se encontraban antes de la recuperación de bosque tuvo lugar. Si aún existen los controladores de dominio, debe quitar forzosamente el servicio AD DS, o pueden volver a instalar los controladores de dominio. No se pueden reutilizar las copias de seguridad existentes para estos controladores de dominio, porque los metadatos correspondientes se quitó durante la recuperación del bosque. En un entorno sin complicaciones, este proceso de implementación puede ser tan sencillo como volver a conectar los controladores de dominio recuperadas a la red de producción y promover nuevos controladores de dominio según sea necesario.  
  
En una gran empresa que se enfrentan con una infraestructura en todo el mundo, se necesita un plan más sofisticado. La primera fase normalmente consiste en restaurar el anuncio como un servicio; Esto significa que para instalar de forma estratégica coloca los controladores de dominio, que todas las aplicaciones y las distintas divisiones empresariales críticos puedan empezar a funcionar de nuevo. Puede ser aceptable para las sucursales a temporalmente se reduzca el rendimiento como consecuencia de ello. Como una segunda fase, todos los restante y los controladores de dominio menos críticos se volvió a implementar.  
  
 Hay dos métodos para instalar controladores de dominio adicionales, ambos de los cuales se pueden automatizar:  
  
- Clonación  
   - Para entornos virtualizados que ejecutan Windows Server 2012, la clonación es la forma más rápida y sencilla para recuperar un gran número de controladores de dominio. Puede automatizar la recuperación de todos los controladores de dominio virtualizados en un dominio después de restaurar un único controlador de dominio de copia de seguridad.  
   - Para obtener más información acerca de la clonación y los requisitos previos, consulte [Introducción a la virtualización de servicios de dominio de Active Directory (AD DS) (nivel 100)](https://technet.microsoft.com/library/hh831734.aspx).  
- Volver a instalar AD DS mediante Windows PowerShell en servidores que ejecutan Windows Server 2012 (o Dcpromo.exe en servidores que ejecutan versiones anteriores de Windows Server) o mediante el uso de la interfaz de usuario  
   - Para acelerar volver a instalar AD DS, puede usar la instalación desde la opción de medios (IFM) para reducir el tráfico de replicación durante la instalación. Para obtener más información sobre el uso de la **ntdsutil ifm** comando para crear medios de instalación, consulte [instalar AD DS desde medios](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  

Tenga en cuenta los siguientes puntos adicionales para cada réplica de controlador de dominio que se recupera en el bosque mediante la clonación de controlador de dominio virtualizados o mediante la instalación de AD DS (en lugar de restaurar desde copia de seguridad):  
  
- Todo el software en un controlador de dominio que se usa como el origen de clonación debe ser capaz de clonación. Aplicaciones y servicios que no se puede clonar deben quitarse antes de que se inicia la clonación. Si no es posible, debe seleccionarse un controlador de dominio virtualizado alternativo como el origen.  
- Si clonar controladores de dominio virtualizados adicionales desde el primer controlador de dominio virtualizado que se va a restaurar, el DC de origen debe apagarse mientras se copia el archivo VHDX. Deberá estar en ejecución y disponible en línea cuando el clon controladores de dominio virtuales se inicia por primera vez. Si el tiempo de inactividad requerido por el apagado no es aceptable para el primer controlador de dominio recuperado, implementar más controladores de dominio virtualizados mediante la instalación de AD DS para que actúe como el origen de clonación.  
- No hay ninguna restricción en el nombre de host de que el controlador de dominio virtualizado clonado o el servidor en el que desea instalar AD DS. Puede usar un nuevo nombre de host o el nombre de host que estaba en uso. Para obtener más información sobre la sintaxis de nombre de host DNS, consulte [crear los nombres de equipo DNS](https://technet.microsoft.com/library/cc785282.aspx) ([https://go.microsoft.com/fwlink/?LinkId=74564](https://go.microsoft.com/fwlink/?LinkId=74564)).  
- Configure cada servidor con el primer servidor DNS en el bosque (el primer controlador de dominio que se ha restaurado en el dominio raíz) como servidor DNS preferido en las propiedades de TCP/IP de su adaptador de red. Para obtener más información, consulte [configurar TCP/IP para utilizar DNS](https://technet.microsoft.com/library/cc779282.aspx).  
- Volver a implementar todos los RODC en el dominio, mediante la clonación de controlador de dominio virtualizados si varios de los RODC se implementan en una ubicación central, o por el método tradicional de regeneración de quitar y volver a instalar AD DS si se implementan individualmente en ubicaciones encuentra aisladas como las sucursales.  
   - Volver a generar los RODC, se garantiza que no contienen todos los objetos persistentes y puede ayudar a evitar conflictos de replicación más adelante. Al quitar AD DS de un RODC, *elegir la opción de conservar los metadatos del controlador de dominio*. Con esta opción conserva la cuenta krbtgt para el RODC y conserva los permisos para la cuenta de administrador RODC delegado y la directiva de replicación de contraseñas (PRP) y evita tener que usar credenciales de administrador de dominio para quitar y volver a instalar AD DS en un RODC. También conserva las funciones de catálogo global y el servidor DNS si están instaladas originalmente en el RODC.  
   - Cuando se vuelve a generar los controladores de dominio (RODC o controladores de dominio grabables), puede haber aumento del tráfico de replicación durante su reinstalación. Para ayudar a reducir este impacto, se puede escalonar la programación de las instalaciones de RODC y puede usar la opción instalar desde medios (IFM). Si usa la opción IFM, ejecute el **ntdsutil ifm** comando en un controlador de dominio grabable que confían libre de datos dañados. Esto ayuda a evitar posibles daños en los que no aparezcan en el RODC una vez completada la reinstalación de AD DS. Para obtener más información acerca de IFM, vea [instalar AD DS desde medios](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  
   - Para obtener más información sobre la reconstrucción de los RODC, vea [RODC eliminación y reinstalación](https://technet.microsoft.com/library/cc835490\(WS.10\).aspx).  
- Si un controlador de dominio estaba ejecutando el servicio servidor DNS antes del error de funcionamiento del bosque, instalar y configurar el servicio servidor DNS durante la instalación de AD DS. En caso contrario, configure sus clientes DNS anteriores con otros servidores DNS.  
- Si necesita otros catálogos globales para compartir la autenticación o carga de consultas para los usuarios o aplicaciones, puede agregar el catálogo global en el origen de había virtualizada de controlador de dominio antes de clonar o hacer que un controlador de dominio un servidor de catálogo global durante la instalación de AD DS.  
  
## <a name="next-steps"></a>Pasos siguientes

- [Recuperación de bosques de AD: requisitos previos](AD-Forest-Recovery-Prerequisties.md)  
- [Recuperación de bosques de AD - idear un plan de recuperación personalizada del bosque](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperación de bosques de AD: identificar el problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperación de bosques de AD - determinar cómo recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperación de bosques de AD - realizar una recuperación inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Recuperación de bosques de AD: procedimientos](AD-Forest-Recovery-Procedures.md)  
- [Recuperación de bosques de AD - preguntas más frecuentes](AD-Forest-Recovery-FAQ.md)  
- [Recuperación de bosques de AD - recuperar un único dominio dentro de un bosque Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Recuperación de bosques de AD: recuperación del bosque con controladores de dominio de Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)
