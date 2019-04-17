---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Referencia técnica del servicio hora de Windows
description: ''
author: shortpatti
ms.author: pashort
manager: brianlic
ms.date: 02/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 916ac654aeb1ca56e0283f9a18e011ef67734776
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="windows-time-service-technical-reference"></a>Referencia técnica del servicio hora de Windows
El servicio de W32Time proporciona sincronización de reloj de red para los equipos sin necesidad de configuración amplia. El servicio W32Time es fundamental para el funcionamiento correcto de autenticación Kerberos V5 y, por lo tanto, para la autenticación de AD DS. Cualquier aplicación con reconocimiento de Kerberos, incluidos la mayoría de los servicios de seguridad, se basa en la sincronización de hora entre los equipos que participan en la solicitud de autenticación. También deben sincronizado a controladores de dominio de AD DS relojes para ayudar a garantizar la replicación de datos precisos.

> [!NOTE]  
> En Windows Server 2003 y Microsoft Windows 2000 Server, el servicio de directorio se denomina servicio de directorio de Active Directory. En Windows Server 2008 R2 y Windows Server 2008, el servicio de directorio se llama servicios de dominio de Active Directory (AD DS). El resto de este tema se refiere a AD DS, pero la información también es aplicable a los servicios de dominio de Active Directory en Windows Server 2016.

El servicio W32Time se implementa en una biblioteca de vínculos dinámicos denominada W32Time.dll, que se instala de forma predeterminada en **%Systemroot%\System32**. W32Time.dll originalmente fue desarrollado para que Windows 2000 Server admitir una especificación por el protocolo de autenticación de Kerberos V5 que necesita relojes en una red a sincronizarse. A partir de Windows Server 2003, W32Time.dll proporcionan una mayor precisión en la sincronización de reloj de red en el sistema operativo Windows Server 2000. Además, en Windows Server 2003, W32Time.dll admite una variedad de dispositivos de hardware y los protocolos de red tiempo con proveedores de hora.

Aunque se diseñó originalmente para proporcionar una sincronización reloj para la autenticación de Kerberos, muchas aplicaciones actuales usan marcas de tiempo para asegurar la coherencia transaccional, registrar la hora de eventos importantes y otros fundamentales de la empresa, sujetos a limitación temporal información.  Estas aplicaciones se benefician de la sincronización de hora entre equipos proporcionados por el servicio hora de Windows.

## <a name="importance-of-time-protocols"></a>Importancia de los protocolos de tiempo
Protocolos de tiempo que se comunican entre dos equipos para intercambiar información de tiempo y, a continuación, usar esa información para sincronizar sus relojes. Con el protocolo de tiempo del servicio hora de Windows, un cliente solicita información de tiempo desde un servidor y sincroniza su reloj según la información que se recibió.
  
El servicio hora de Windows usa NTP para sincronizar la hora a través de una red. NTP es un protocolo de tiempo de Internet que incluya los algoritmos disciplina necesarios para sincronizar el reloj. NTP es un protocolo de tiempo más preciso que el protocolo de tiempo de red Simple (SNTP) que se usa en algunas versiones de Windows; Sin embargo, W32Time continúa admitiendo SNTP para habilitar la compatibilidad con equipos que ejecutan servicios basados en SNTP tiempo como Windows 2000.
<!-- maybe this should be its own topic under the Tech Ref section -->
## <a name="BKMK_Config"></a>Dónde encontrar información relacionada con la configuración de servicio hora de Windows  
Esta guía hace **no** hablar sobre la configuración del servicio hora de Windows. Hay varios temas diferentes en Microsoft TechNet y en Microsoft Knowledge Base donde se explican los procedimientos para configurar el servicio hora de Windows. Si necesitas información de configuración, los siguientes temas deberían ayudarte a encontrar la información adecuada.  
<!-- should this be an if/then table -->
-   Para configurar el servicio hora de Windows para el emulador de controlador (PDC) del dominio principal de raíz de bosque, consulta:  
  
    -   [Configurar el servicio hora de Windows en el emulador PDC en el dominio raíz del bosque](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29) 
  
    -   [Configurar un origen de tiempo para el bosque](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794823%28v%3dws.10%29) 
  
    -   Artículo de Microsoft Knowledge Base 816042, [cómo configurar un servidor horario autorizado en Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402), que describe las opciones de configuración para equipos que ejecutan Windows Server 2008 R2, Windows Server 2008, Windows Server 2003, y Windows Server 2003 R2.  
  
-   Para configurar el servicio hora de Windows en cualquier cliente de miembro de dominio o servidor o incluso controladores de dominio que no están configurados como el emulador PDC de raíz de bosque, consulte [configurar un equipo cliente para la sincronización de hora automática dominio](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
    > [!WARNING]  
    > Algunas aplicaciones pueden requerir sus equipos para que los servicios en tiempo de alta precisión. Si es así, puedes configurar un origen de tiempo manual, pero ten en cuenta que el servicio hora de Windows no está diseñado para funcionar como un origen en tiempo de alta precisión. Asegúrate de que el que estés informado de las limitaciones de soporte técnico para entornos de tiempo de alta precisión, como se describe en el artículo de Microsoft Knowledge Base 939322, [límite de soporte para configurar el servicio hora de Windows para entornos de alta precisión](https://go.microsoft.com/fwlink/?LinkID=179459).  
  
-   Para configurar el servicio hora de Windows en los equipos de cliente o servidor basado en Windows que están configurados como miembros del grupo de trabajo en lugar de los miembros del dominio verán [configurar un origen de tiempo manual de un equipo cliente seleccionado](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816656%28v%3dws.10%29).  
  
-   Para configurar el servicio hora de Windows en un equipo host que ejecuta un entorno virtual, consulta el artículo de Microsoft Knowledge Base 816042, [cómo configurar un servidor horario autorizado en Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402). Si trabajas con un producto de virtualización no son de Microsoft, asegúrate de consultar la documentación del proveedor para ese producto.  
  
-   Para configurar el servicio hora de Windows en un controlador de dominio que se ejecuta en una máquina virtual, se recomienda que parcialmente deshabilitar la sincronización de tiempo entre el sistema y el invitado sistema operativo host actúa como un controlador de dominio. Esto permite que el controlador de dominio de invitado sincronizar la hora de la jerarquía de dominios, pero protege de tener un tiempo sesgar si se restaura desde un estado guardado. Para obtener más información, consulta el artículo de Microsoft Knowledge Base 976924, [recibe servicio hora de Windows de los identificadores de evento 24, 29 y 38 en un controlador de dominio virtualizados que está ejecutando en un servidor basado en Windows Server 2008 con Hyper-V](https://go.microsoft.com/fwlink/?LinkID=192236) y [Consideraciones de implementación de controladores de dominio virtualizada](https://go.microsoft.com/fwlink/?LinkID=192235).  
  
-   Para configurar el servicio hora de Windows en un controlador de dominio que actúa como el emulador PDC de raíz de bosque que también se esté ejecutando en un equipo virtual, sigue las mismas instrucciones para un equipo físico, como se describe en [configurar el servicio hora de Windows en el PDC emulador de dominio raíz del bosque](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
-   Para configurar el servicio hora de Windows en un servidor miembro que se ejecuta como un equipo virtual, usa la jerarquía de tiempo del dominio como se describe en ([configurar un equipo cliente para la sincronización de hora automática dominio](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).


> [!IMPORTANT]  
> Antes de Windows Server 2016, el servicio W32Time no se diseñó para satisfacer las necesidades de aplicación sujetos a limitación temporal.  Sin embargo, las actualizaciones de Windows Server 2016 ahora te permiten implementar una solución para 1 ms precisión en su dominio.  Consulta [Windows 2016 preciso tiempo](accurate-time.md) y [límite de soporte para configurar el servicio hora de Windows para entornos de alta precisión](https://go.microsoft.com/fwlink/?LinkID=179459) para obtener más información.

## <a name="related-topics"></a>Temas relacionados  
[Cómo funciona el servicio hora de Windows](How-the-Windows-Time-Service-Works.md)  
[Herramientas de servicio hora de Windows y configuración](Windows-Time-Service-Tools-and-Settings.md)  
[Artículo de Microsoft Knowledge Base 902229](https://go.microsoft.com/fwlink/?LinkId=186066)