---
ms.assetid: 407d5e81-c04c-4275-9ae9-35f65b4a371a
title: Planear la ubicación del servidor de catálogo global
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: b424d758d98315ff63f05926880a23970f54e49c
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81623973"
---
# <a name="planning-global-catalog-server-placement"></a>Planear la ubicación del servidor de catálogo global

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La selección de ubicación del catálogo global requiere planeación, excepto si tiene un bosque de un solo dominio. En un bosque de un solo dominio, configure todos los controladores de dominio como servidores de catálogo global. Dado que cada controlador de dominio almacena la única partición de directorio de dominio en el bosque, la configuración de cada controlador de dominio como un servidor de catálogo global no requiere el uso adicional de espacio en disco, el uso de CPU o el tráfico de replicación. En un bosque de un solo dominio, todos los controladores de dominio actúan como servidores de catálogo global virtual; es decir, todos pueden responder a cualquier solicitud de autenticación o servicio. Esta condición especial para bosques de dominio único es por diseño. Las solicitudes de autenticación no requieren que se ponga en contacto con un servidor de catálogo global como lo hacen cuando hay varios dominios y un usuario puede ser miembro de un grupo universal que existe en un dominio diferente. Sin embargo, solo los controladores de dominio que se designan como servidores de catálogo global pueden responder a las consultas del catálogo global en el puerto del catálogo global 3268. Para simplificar la administración en este escenario y garantizar respuestas coherentes, la designación de todos los controladores de dominio como servidores de catálogo global elimina la preocupación sobre qué controladores de dominio pueden responder a las consultas del catálogo global. En concreto, cada vez que un usuario usa Start\Search\For People o encuentra impresoras o expande grupos universales, estas solicitudes solo van al catálogo global.

En bosques de varios dominios, los servidores de catálogo global facilitan las solicitudes de inicio de sesión de usuario y las búsquedas en todo el bosque. En la ilustración siguiente se muestra cómo determinar qué ubicaciones requieren servidores de catálogo global.

![Planeación de la ubicación de GC](media/Planning-Global-Catalog-Server-Placement/8fc4777c-47b6-4ee7-b8ad-a04e7c5ee67f.gif)

En la mayoría de los casos, se recomienda incluir el catálogo global al instalar nuevos controladores de dominio. Se aplican las excepciones siguientes:

- Ancho de banda limitado: en sitios remotos, si el vínculo de la red de área extensa (WAN) entre el sitio remoto y el sitio del concentrador es limitado, puede usar el almacenamiento en caché de la pertenencia a grupos universales en el sitio remoto para dar cabida a las necesidades de inicio de sesión de los usuarios en el sitio.
- Incompatibilidad de la función de maestro de operaciones de infraestructura: no coloque el catálogo global en un controlador de dominio que hospede el rol de maestro de operaciones de infraestructura en el dominio a menos que todos los controladores de dominio del dominio sean servidores de catálogo global o que el bosque solo tenga un dominio.

## <a name="adding-global-catalog-servers-based-on-application-requirements"></a>Adición de servidores de catálogo global en función de los requisitos de la aplicación

Ciertas aplicaciones, como Microsoft Exchange, Message Queue Server (también conocido como MSMQ), y las aplicaciones que usan DCOM no proporcionan una respuesta adecuada a través de vínculos WAN latentes y, por tanto, necesitan una infraestructura de catálogo global de alta disponibilidad para proporcionar una latencia de consulta baja. Determine si las aplicaciones que funcionan mal a través de un vínculo WAN lento se ejecutan en ubicaciones o si las ubicaciones requieren Microsoft Exchange Server. Si las ubicaciones incluyen aplicaciones que no proporcionan una respuesta adecuada a través de un vínculo WAN, debe colocar un servidor de catálogo global en la ubicación para reducir la latencia de las consultas.

> [!NOTE]
> Los controladores de dominio de solo lectura (RODC) se pueden promover correctamente al estado del servidor de catálogo global. Sin embargo, algunas aplicaciones habilitadas para el directorio no admiten un RODC como servidor de catálogo global. Por ejemplo, ninguna versión de Microsoft Exchange Server utiliza RODC. Sin embargo, Microsoft Exchange Server funciona en entornos que incluyen RODC, siempre y cuando haya controladores de dominio de escritura disponibles. Exchange Server 2007 omite de forma eficaz los RODC. Exchange Server 2003 también omite los RODC en condiciones predeterminadas en las que los componentes de Exchange detectan automáticamente los controladores de dominio disponibles. No se realizó ningún cambio en Exchange Server 2003 para que sea consciente de los servidores de directorio de solo lectura. Por lo tanto, si se intenta forzar los servicios y las herramientas de administración de Exchange Server 2003 para usar RODC, se puede producir un comportamiento impredecible.

## <a name="adding-global-catalog-servers-for-a-large-number-of-users"></a>Adición de servidores de catálogo global para un gran número de usuarios

Coloque los servidores de catálogo global en todas las ubicaciones que contengan más de 100 usuarios para reducir la congestión de los vínculos WAN de red y evitar la pérdida de productividad en caso de error de vínculo WAN.

## <a name="using-highly-available-bandwidth"></a>Uso de ancho de banda de alta disponibilidad

No es necesario colocar un catálogo global en una ubicación que no incluya aplicaciones que requieran un servidor de catálogo global, que incluya menos de 100 usuarios y que también esté conectada a otra ubicación que incluya un servidor de catálogo global mediante un vínculo WAN que esté disponible en el 100 por Active Directory Domain Services (AD DS). En este caso, los usuarios pueden tener acceso al servidor de catálogo global a través del vínculo WAN.

Los usuarios móviles deben ponerse en contacto con los servidores de catálogo global cada vez que inicien sesión por primera vez en cualquier ubicación. Si el tiempo de inicio de sesión a través del vínculo WAN no es aceptable, coloque un catálogo global en una ubicación que sea visitada por un gran número de usuarios móviles.

## <a name="enabling-universal-group-membership-caching"></a>Habilitación del almacenamiento en caché de pertenencia al grupo universal

En las ubicaciones que incluyen menos de 100 usuarios y que no incluyen un gran número de usuarios móviles o aplicaciones que requieren un servidor de catálogo global, puede implementar controladores de dominio que ejecuten Windows Server 2008 y habilitar el almacenamiento en caché de pertenencia a grupos universales. Asegúrese de que los servidores de catálogo global no tengan más de un salto de replicación del controlador de dominio en el que esté habilitado el almacenamiento en caché de pertenencia a grupos universales para que se pueda actualizar la información del grupo universal en la memoria caché. Para obtener información acerca de cómo funciona el almacenamiento en caché de grupos universales, consulte el artículo sobre [Cómo funciona el catálogo global](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc737410(v=ws.10)).

Para ver una hoja de cálculo que le ayude a documentar dónde va a colocar los servidores de catálogo global y los controladores de dominio con el almacenamiento en caché de grupo universal habilitado, vea la [ayuda de trabajo para el kit de implementación de Windows Server 2003](https://microsoft.com/download/details.aspx?id=9608), descargar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip y abrir la ubicación del controlador de dominio (DSSTOPO_4. doc). Vea la información acerca de las ubicaciones en las que es necesario colocar los servidores de catálogo global al implementar el dominio raíz del bosque y los dominios regionales.
