---
ms.assetid: 407d5e81-c04c-4275-9ae9-35f65b4a371a
title: "Planeación de la ubicación de servidor de catálogo Global"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c32393640e929adf9681f511ed3707b85a3784db
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="planning-global-catalog-server-placement"></a>Planeación de la ubicación de servidor de catálogo Global

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Colocación del catálogo global exige una buena planificación excepto si tienes un bosque de dominio único. En un bosque de dominio único, configurar todos los controladores de dominio como servidores de catálogo global. Dado que cada controlador de dominio almacena la partición de directorio solo en el bosque, configuración de cada controlador de dominio como un servidor de catálogo global no requiere cualquier uso del espacio en disco adicional, el uso de CPU o el tráfico de replicación. En un bosque de dominio único, todos los controladores de dominio actúan como servidores virtuales catálogo global; es decir, pueden todos responder a ninguna autenticación o la solicitud de servicio. Esta condición especial para bosques de dominio único es por diseño. Las solicitudes de autenticación no es necesario ponerse en contacto con un servidor de catálogo global, como sucede cuando hay varios dominios, y un usuario puede ser un miembro de un grupo universal con la que existe en un dominio diferente. Sin embargo, solo los controladores de dominio que se designan como servidores de catálogo global pueden responder a las consultas del catálogo global en el puerto del catálogo global 3268. Para simplificar la administración en este escenario y para garantizar las respuestas coherentes, la designación de todos los controladores de dominio como servidores de catálogo global elimina la preocupación sobre qué dominio controladores pueden responder a las consultas de catálogo global. Específicamente, siempre que un usuario usa Start\Search\For personas o buscar impresoras o grupos universales, expande, estas solicitudes se van solo en el catálogo global.  
  
En bosques de varios dominios, servidores de catálogo global facilitan las solicitudes de inicio de sesión de usuario y las búsquedas de todo el bosque. La siguiente ilustración muestra cómo determinar las ubicaciones que requieren los servidores de catálogo global.  
  
![Planeación de colocación gc](media/Planning-Global-Catalog-Server-Placement/8fc4777c-47b6-4ee7-b8ad-a04e7c5ee67f.gif)  
  
En la mayoría de los casos, se recomienda que incluyas el catálogo global al instalar nuevos controladores de dominio. Se aplican las siguientes excepciones:  
  
-   Ancho de banda limitado: en sitios remotos, si el vínculo de área extensa (WAN) de red entre el sitio remoto y el sitio de concentrador es limitado, puedes usar el almacenamiento en caché de la pertenencia a grupos universales en el sitio remoto para satisfacer las necesidades de inicio de sesión de los usuarios del sitio.  
  
-   Incompatibilidad del rol de maestro de operaciones de infraestructura: el catálogo global no las coloques en un controlador de dominio que hospeda el rol de maestro de operaciones de infraestructura en el dominio a menos que todos los controladores de dominio en el dominio son servidores de catálogo global o bosque tiene un solo dominio.  
  
## <a name="adding-global-catalog-servers-based-on-application-requirements"></a>Agregar servidores de catálogo global en función de los requisitos de la aplicación  
Ciertas aplicaciones, como Microsoft Exchange, Message Queue Server (también conocido como MSMQ) y aplicaciones que usan DCOM no ofrecer respuesta adecuado en los vínculos WAN latentes y, por tanto, necesita una infraestructura de alta disponibilidad catálogo global para proporcionar baja latencia. Determinar si se ejecutan todas las aplicaciones que no funcionan correctamente en un vínculo WAN lento en ubicaciones o si las ubicaciones requieren Microsoft Exchange Server. Si las ubicaciones incluyen aplicaciones que no proporcionen suficiente respuesta en un vínculo WAN, debes colocar un servidor de catálogo global en la ubicación para reducir la latencia de consulta.  
  
> [!NOTE]  
> Read-only controladores de dominio (RODC) pueden promocionarse correctamente para el estado del servidor de catálogo global. Sin embargo, ciertas aplicaciones habilitadas para directorio no admiten un RODC como un servidor de catálogo global. Por ejemplo, ninguna versión de Microsoft Exchange Server usa RODC. Sin embargo, Microsoft Exchange Server funciona en entornos que incluyan los RODC, siempre y cuando hay controladores de dominio grabable disponibles. Exchange Server 2007 eficazmente omite RODC. Exchange Server 2003 también pasa por alto los RODC en condiciones de forma predeterminada en los componentes de Exchange detectan automáticamente los controladores de dominio disponible. Cambios no realizados en Exchange Server 2003, para que sea consciente de servidores de directorio de sólo lectura. Por lo tanto, al intentar forzar servicios de Exchange Server 2003 y herramientas de administración para usar RODC puede provocar un comportamiento impredecible.  
  
## <a name="adding-global-catalog-servers-for-a-large-number-of-users"></a>Agregar servidores de catálogo global para un gran número de usuarios  
Coloca los servidores de catálogo global en todas las ubicaciones que contienen más de 100a los usuarios para reducir la congestión de los vínculos WAN de red y para evitar la pérdida de productividad en caso de error del vínculo WAN.  
  
## <a name="using-highly-available-bandwidth"></a>Uso de ancho de banda altamente disponible  
No es necesario colocar un catálogo global en una ubicación que no incluye aplicaciones que requieren un servidor de catálogo global, incluye menos de 100a los usuarios y también está conectada a otra ubicación que incluye un catálogo global de servidor mediante un vínculo WAN que está disponible para los servicios de dominio de Active Directory (AD DS) el 100 por ciento. En este caso, los usuarios pueden tener acceso al servidor de catálogo global en el vínculo WAN.  
  
Los usuarios móviles ponerte en contacto con los servidores de catálogo global cuando inician sesión por primera vez en cualquier ubicación. Si el tiempo de inicio de sesión a través del vínculo WAN es aceptable, coloca un catálogo global en una ubicación que se visita un gran número de usuarios móviles.  
  
## <a name="enabling-universal-group-membership-caching"></a>Habilitar el almacenamiento en caché de la pertenencia a grupos universales  
Para las ubicaciones que incluyan menos de 100a los usuarios y que no incluyen un gran número de móvil a los usuarios o aplicaciones que requieren un servidor de catálogo global, puedes implementar controladores de dominio que ejecutan Windows Server 2008 y habilitar la caché de pertenencia a grupos universales. Asegúrate de que los servidores de catálogo global no son más de un salto de replicación del controlador de dominio en el que se habilita el almacenamiento en caché de la pertenencia a grupos universales para que se puede actualizar la información de los grupos universales en la memoria caché. Para obtener información sobre cómo universal grupo el almacenamiento en caché, consulta Global catálogo funcionamiento del ([https://go.microsoft.com/fwlink/?LinkId=107063](https://go.microsoft.com/fwlink/?LinkId=107063)).  
  
Para que una hoja de cálculo para ayudarte en documentar dónde va a colocar los controladores de dominio y servidores de catálogo global está activada la caché de grupos universales, consulta el trabajo Aid para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), descargar < DICT__Job_ Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip</DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>, and open Domain Controlador colocación (DSSTOPO_4.doc). Consulta la información sobre ubicaciones en el que deberás colocar los servidores de catálogo global al implementar el dominio raíz del bosque y dominios regionales.  
  


