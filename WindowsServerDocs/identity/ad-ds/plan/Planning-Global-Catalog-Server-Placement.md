---
ms.assetid: 407d5e81-c04c-4275-9ae9-35f65b4a371a
title: Planear la ubicación del servidor de catálogo global
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: aefed1a2ed0d346f75ad4d135f8c0ae314fd7fc7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820686"
---
# <a name="planning-global-catalog-server-placement"></a>Planear la ubicación del servidor de catálogo global

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ubicación del catálogo global requiere una planeación, excepto si tiene un bosque de dominio único. En un bosque de dominio único, configure todos los controladores de dominio como servidores de catálogo global. Dado que cada controlador de dominio almacena la partición de directorio de dominio solo en el bosque, la configuración de cada controlador de dominio como servidor de catálogo global no requiere ningún uso de espacio en disco adicional, el uso de CPU o el tráfico de replicación. En un bosque de dominio único, todos los controladores de dominio actúan como servidores de catálogo global virtual; es decir, pueden todas responden a cualquier autenticación o solicitud de servicio. Esta condición especial para bosques de dominio único es así por diseño. Las solicitudes de autenticación no es necesario ponerse en contacto con un servidor de catálogo global, como lo hacen cuando hay varios dominios y un usuario puede ser un miembro de un grupo universal que existe en un dominio diferente. Sin embargo, solo los controladores de dominio que se designan como servidores de catálogo global pueden responder a las consultas del catálogo global en el puerto del catálogo global 3268. Para simplificar la administración en este escenario y garantizar respuestas coherentes, la designación de todos los controladores de dominio como servidores de catálogo global elimina la preocupación sobre qué dominio controladores pueden responder a las consultas del catálogo global. En concreto, cada vez que un usuario usa Start\Search\For personas o buscar impresoras o expande los grupos universales, estas solicitudes irán solo en el catálogo global.  
  
En los bosques de varios dominios, servidores de catálogo global facilitan las solicitudes de inicio de sesión de usuario y las búsquedas en todo el bosque. La siguiente ilustración muestra cómo determinar qué ubicaciones requieren servidores de catálogo global.  
  
![Planear la ubicación del catálogo global](media/Planning-Global-Catalog-Server-Placement/8fc4777c-47b6-4ee7-b8ad-a04e7c5ee67f.gif)  
  
En la mayoría de los casos, se recomienda incluir el catálogo global al instalar nuevos controladores de dominio. Se aplican las siguientes excepciones:  
  
- Ancho de banda limitado: En sitios remotos, si el vínculo de área extensa (WAN) de red entre el sitio remoto y el sitio concentrador es limitado, se puede usar el almacenamiento en caché de la pertenencia a grupos universales en el sitio remoto para dar cabida a las necesidades de inicio de sesión de usuarios en el sitio.  
- Incompatibilidad de rol de maestro de operaciones de infraestructura: No coloque el catálogo global en un controlador de dominio que hospeda las operaciones de infraestructura dominar rol en el dominio a menos que todos los controladores de dominio del dominio sean servidores de catálogo global o el bosque tiene un único dominio.  
  
## <a name="adding-global-catalog-servers-based-on-application-requirements"></a>Adición de servidores de catálogo global en función de los requisitos de aplicación

Determinadas aplicaciones como Microsoft Exchange, Message Queuing (también conocido como MSMQ) y las aplicaciones que usan DCOM no entregar la respuesta adecuada en los vínculos WAN latente y, por tanto, necesita una infraestructura de alta disponibilidad de catálogo global para proporcionar una consulta baja latencia. Determinar si se está ejecutando las aplicaciones que tienen un rendimiento bajo en un vínculo WAN lento en ubicaciones o si las ubicaciones requieren Microsoft Exchange Server. Si las ubicaciones incluyen las aplicaciones que no proporcionen una respuesta adecuada en un vínculo WAN, debe colocar un servidor de catálogo global en la ubicación para reducir la latencia de consulta.  
  
> [!NOTE]  
> Controladores de dominio de solo lectura (RODC) se pueden promocionar correctamente el estado del servidor de catálogo global. Sin embargo, ciertas aplicaciones habilitadas para directorio no admiten un RODC como un servidor de catálogo global. Por ejemplo, ninguna versión de Microsoft Exchange Server usa los RODC. Sin embargo, Microsoft Exchange Server funciona en entornos que incluyen los RODC, siempre que hay controladores de dominio de escritura. Exchange Server 2007 eficazmente omite los RODC. Exchange Server 2003 también omite los RODC en condiciones predeterminadas donde los componentes de Exchange detectan automáticamente los controladores de dominio disponibles. No hay cambios realizados en Exchange Server 2003 para que sea consciente de los servidores de directorio de sólo lectura. Por lo tanto, intentando forzar los servicios de Exchange Server 2003 y las herramientas de administración para usar el RODC puede provocar un comportamiento imprevisible.  
  
## <a name="adding-global-catalog-servers-for-a-large-number-of-users"></a>Adición de servidores de catálogo global para un gran número de usuarios

Coloque los servidores de catálogo global en todas las ubicaciones que contienen más de 100 usuarios para reducir la congestión de los vínculos WAN de red y para evitar la pérdida de productividad en el caso de error en el vínculo WAN.  
  
## <a name="using-highly-available-bandwidth"></a>Uso de ancho de banda de alta disponibilidad

No es necesario colocar un catálogo global en una ubicación que no incluye las aplicaciones que requieren un servidor de catálogo global, incluye menos de 100 usuarios y también está conectada a otra ubicación que incluye un servidor de catálogo global mediante un vínculo WAN al 100 por cien una disponible para los servicios de dominio de Active Directory (AD DS). En este caso, los usuarios pueden tener acceso el servidor de catálogo global a través del vínculo WAN.  
  
Los usuarios móviles deben ponerse en contacto con los servidores de catálogo global cada vez que inician sesión por primera vez en cualquier ubicación. Si la hora de inicio de sesión a través del vínculo WAN es aceptable, coloque un catálogo global en una ubicación que se visita un gran número de usuarios móviles.  
  
## <a name="enabling-universal-group-membership-caching"></a>Habilitar caché de pertenencia al grupo universal

Para las ubicaciones que incluyen menos de 100 usuarios y que no incluyen un gran número de móvil a los usuarios o aplicaciones que requieren un servidor de catálogo global, puede implementar controladores de dominio que ejecutan Windows Server 2008 y habilitar la pertenencia a grupos universales almacenamiento en caché. Asegúrese de que los servidores de catálogo global no más de un salto de replicación del controlador de dominio en el que está habilitado el almacenamiento en caché de la pertenencia al grupo universal para que se puede actualizar la información del grupo universal en la memoria caché. Para obtener información acerca de cómo universal funciona almacenamiento en caché de grupo, consulte el artículo [Global Catalog funcionamiento](https://go.microsoft.com/fwlink/?LinkId=107063).  
  
Para que una hoja de cálculo para ayudarle en la documentación donde piensa colocar los servidores de catálogo global y controladores de dominio está activada la caché de grupo universal, consulte [trabajo ayudas para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), descargue Job_Aids_ Designing_and_Deploying_Directory_and_Security_Services.zip y abrir ubicación del controlador de dominio (DSSTOPO_4.doc). Consulte la información acerca de las ubicaciones en el que tiene que colocar los servidores de catálogo global al implementar el dominio raíz del bosque y dominios regionales.  
