---
title: "Recuperación de bosque de AD - reimplementar restante controladores de dominio"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 66b0bc65b3b8e5dfbf5f1a85350dab60ac3a11c8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---redeploy-remaining-dcs"></a>Recuperación de bosque de AD - reimplementar restante controladores de dominio

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

 Los pasos hasta este punto se aplican a todos los bosques: encontrar una copia de seguridad válida para cada dominio, recuperar los dominios de manera aislada, vuelva a conectarlos, restablecer el catálogo global y limpia. En este paso se vuelve a implementar el bosque. La manera de hacerlo dependerá en gran medida el diseño de bosque, los acuerdos de nivel de servicio, estructura del sitio, ancho de banda disponible y muchos otros factores. Tendrás que diseñar tu propio plan reimplementación en función de los principios y sugerencias en esta sección, de manera que se adapta mejor a los requisitos empresariales.  
  
 El siguiente paso es instalar AD DS en todos los controladores de dominio que estaban presentes antes de la recuperación de bosque tuvo lugar. Si los controladores de dominio siguen existan, el servicio de AD DS, deberás quitar forzosamente o pueden volver a instalar los controladores de dominio. No se puede reutilizar las copias de seguridad existentes para estos controladores de dominio, porque se ha quitado los metadatos correspondientes durante la recuperación de bosque. En un entorno sin complicaciones este proceso reimplementación puede ser tan sencillo como volver a conectar los controladores de dominio recuperados a la red de producción, promover nuevos controladores de dominio, según sea necesario.  
  
 En una empresa grande con una infraestructura de todo el mundo, es necesario un plan más sofisticado. La primera fase se suele restaurar el anuncio como un servicio; Esto significa para instalar estratégica coloca controladores de dominio que todas las aplicaciones y divisiones fundamentales para la empresa pueden empezar a trabajar de nuevo. Puede ser aceptable para las sucursales a temporalmente se reduzca el rendimiento como resultado de este. Como una segunda fase, todos los demás y controladores de dominio menos críticos se vuelve a instalar.  
  
 Hay dos métodos para instalar controladores de dominio adicionales, que se pueden automatizar:  
  
-   Clonación  
  
     Para entornos virtualizados que ejecutan Windows Server 2012, clonación es la manera más rápida y más sencilla para recuperar un gran número de controladores de dominio. Puede automatizar la recuperación de todos los controladores de dominio virtualizadas en un dominio después de restaurar un único controlador de dominio virtualizada de copia de seguridad.  
  
     Para obtener más información sobre los requisitos previos y clonación, consulta [Introducción a la virtualización de los servicios de dominio de Active Directory (AD DS) (nivel 100)](https://technet.microsoft.com/library/hh831734.aspx).  
  
-   Volver a instalar AD DS mediante Windows PowerShell en servidores que ejecutan Windows Server 2012 (o Dcpromo.exe en los servidores que ejecutan versiones anteriores de Windows Server) o mediante el uso de la interfaz de usuario  
  
     Para acelerar volver a instalar AD DS, puedes usar instalación de la opción de medios (IFM) para reducir el tráfico de replicación durante la instalación. Para obtener más información sobre cómo usar la **ntdsutil ifm** comando para crear medios de instalación, consulta [instalar AD DS desde medios](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  
  
 Ten en cuenta los siguientes puntos adicionales para cada réplica DC que se recupera del bosque duplicando virtualizada DC o mediante la instalación de AD DS (en lugar de restauración de copia de seguridad):  
  
-   Todo el software en un controlador de dominio que se usa como el origen para clonar debe poder clonar. Aplicaciones y servicios que no se puede clonar deben quitarse antes de que se inicie la clonación. Si no es posible, un controlador de dominio virtualizada alternativo debería elegirse como el origen.  
  
-   Si clonar DC virtualizada adicional desde el primer controlador de dominio virtualizada restaurarse, el origen de controlador de dominio que apagar mientras se copia el archivo VHDX. A continuación, deberás estar en ejecución y disponible en línea cuando la copia de controladores de dominio virtuales inicia por primera vez. Si el tiempo de inactividad requerido por el apagado no es aceptable para el primer controlador de dominio recuperado, implementar un controlador de dominio virtualizada adicional mediante la instalación de AD DS para que actúe como el origen de clonación.  
  
-   No hay ninguna restricción en el nombre de host de DC virtualizado clonado o el servidor en el que desea instalar AD DS. Puedes usar un nuevo nombre de host o el nombre de host que estuviera en uso. Para obtener más información sobre la sintaxis de nombre de host DNS, consulta [crear los nombres de equipo DNS](https://technet.microsoft.com/library/cc785282.aspx) ([https://go.microsoft.com/fwlink/?LinkId=74564](https://go.microsoft.com/fwlink/?LinkId=74564)).  
  
-   Configurar cada servidor con el primer servidor DNS en el bosque (el primer controlador de dominio que se restauró en el dominio raíz) como el servidor DNS preferido en las propiedades de TCP/IP de su adaptador de red. Para obtener más información, consulta [configurar TCP/IP para que use DNS](https://technet.microsoft.com/library/cc779282.aspx).  
  
-   Vuelve a implementar todos los RODCs en el dominio, virtualizada DC clonación si varias RODC están distribuidos en una ubicación central, bien por el método tradicional de regeneración de quitando y volviendo a instalar AD DS si implementarlos individualmente en ubicaciones encuentra aislados como sucursales.  
  
     Recompilar RODC garantiza que no contienen todos los objetos persistentes y puede ayudar a evitar conflictos de replicación más adelante. Cuando quites AD DS de un RODC, *elige la opción para conservar los metadatos de DC*. Con esta opción mantiene la cuenta krbtgt de RODC conserva los permisos para la cuenta de administrador delegado RODC y la directiva de replicación de contraseñas (PRP) y evita tener usar credenciales de administrador de dominio para quitar y reinstalar AD DS en un RODC. También conserva el servidor DNS y funciones de catálogo global si se instalan en el RODC originalmente.  
  
     Al volver a compilar controladores de dominio (RODC o controladores de dominio grabables), es posible que haya aumentado el tráfico de replicación durante la reinstalación. Para ayudar a reducir afecta esto, puede escalonar la programación de las instalaciones de RODC y puedes usar la opción de instalar desde el medio (IFM). Si usas la opción IFM, ejecutar la **ntdsutil ifm** comando en un controlador de dominio grabable que confíes libre de datos dañados. Esto ayuda a evitar posibles daños aparezca en el RODC una vez completada la reinstalación de AD DS. Para obtener más información sobre IFM, consulta [instalar AD DS desde medios](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx).  
  
     Para obtener más información acerca de cómo reconstruir RODC, consulta [RODC eliminación y reinstalación](https://technet.microsoft.com/library/cc835490\(WS.10\).aspx).  
  
-   Si un controlador de dominio estaba ejecutando el servicio de servidor DNS antes del error de funcionamiento del bosque, instalar y configurar el servicio de servidor DNS durante la instalación de AD DS. De lo contrario, configurar a sus clientes DNS anteriores con otros servidores DNS.  
  
-   Si necesitas otros catálogos globales para compartir la autenticación o la carga de consultas a los usuarios o aplicaciones, puede agregar el catálogo global para el origen virtualizados DC antes de clonación o puedes crear un controlador de dominio un servidor de catálogo global durante la instalación de AD DS.  
  
## <a name="next-steps"></a>Pasos siguientes
-   [Recuperación de bosque de AD - requisitos previos](AD-Forest-Recovery-Prerequisties.md)  
-   [Recuperación de bosque de AD - diseñar un plan de recuperación personalizada del bosque](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperación de bosque de AD - identificar el problema](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Recuperación de bosque de AD - determinar cómo recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Recuperación de bosque de AD - realizar una recuperación inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md)  
-   [Recuperación de bosque de AD - preguntas más frecuentes](AD-Forest-Recovery-FAQ.md)  
-   [Recuperación de bosque de AD - recuperar un solo dominio dentro de un bosque Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [Recuperación de bosque de AD - recuperación del bosque con controladores de dominio de Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  