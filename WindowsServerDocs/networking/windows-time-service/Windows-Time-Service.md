---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Referencia técnica del servicio de hora de Windows
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 0b424e118980792d4b4db8ba365ad60cc5edc75c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815946"
---
# <a name="windows-time-service"></a>Servicio de hora de Windows

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o posterior


**En esta guía**  
  
* Dónde encontrar información de configuración de servicio de hora de Windows  
* ¿Qué es el servicio de hora de Windows?  
* Importancia de los protocolos de tiempo  
* Funcionamiento del servicio Hora de Windows   
* Configuración y herramientas del servicio Hora de Windows  
  
> [!NOTE]  
> En Windows Server 2003 y Microsoft Windows 2000 Server, el servicio de directorio se denomina servicio de directorio de Active Directory. En Windows Server 2008 R2 y Windows Server 2008, el servicio de directorio se denomina Active Directory Domain Services (AD DS). El resto de este tema hace referencia a AD DS, pero la información también es aplicable a los servicios de dominio de Active Directory en Windows Server 2016.  
  
El servicio de hora de Windows, también conocido como W32Time, sincroniza la fecha y hora para todos los equipos que se ejecutan en un dominio de AD DS. Sincronización de hora es fundamental para el correcto funcionamiento de muchos servicios de Windows y aplicaciones de línea de negocio. El servicio de hora de Windows usa el protocolo de tiempo de red (NTP) para sincronizar los relojes del equipo en la red para que un valor de reloj exacto o marca de tiempo, puede asignarse a las solicitudes de acceso de validación y recursos de red. El servicio integra NTP y proveedores de hora, lo que un servicio de hora confiable y escalable para los administradores de empresa.
  
> [!IMPORTANT]  
> Antes de Windows Server 2016, el servicio W32Time no se diseñó para satisfacer las necesidades de la aplicación dependiente del tiempo.  Sin embargo, las actualizaciones de Windows Server 2016 ahora le permiten implementar una solución de 1 ms precisión en el dominio.  Consulte [Windows 2016 preciso momento](accurate-time.md) y [límite de soporte técnico para configurar el servicio de hora de Windows para entornos de alta precisión](support-boundary.md) para obtener más información.  
  
## <a name="BKMK_Config"></a>Dónde encontrar información de configuración de servicio de hora de Windows  
Esta guía **no** analizará la configuración del servicio de hora de Windows. Hay varios temas diferentes en Microsoft Knowledge Base y en Microsoft TechNet que explican procedimientos para configurar el servicio de hora de Windows. Si necesita información de configuración, los siguientes temas le ayudarán a localizar la información adecuada.  
  
-   Para configurar el servicio de hora de Windows para el emulador PDC (controlador) del dominio principal de raíz de bosque, consulte:  
  
    -   [Configurar el servicio de hora de Windows en el emulador de PDC en el dominio raíz del bosque](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29) 
  
    -   [Configurar un origen de hora para el bosque](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794823%28v%3dws.10%29) 
  
    -   Artículo de Microsoft Knowledge Base 816042 [cómo configurar un servidor horario con autoridad en Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402), que describe las opciones de configuración para equipos que ejecutan Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 y Windows Server 2003 R2.  
  
-   Para configurar el servicio de hora de Windows en cualquier cliente miembro de dominio o servidor o incluso controladores de dominio que no están configurados como el emulador de PDC raíz de bosque, consulte [configurar un equipo cliente para la sincronización de hora automática de dominios](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
    > [!WARNING]  
    > Algunas aplicaciones pueden requerir que los equipos que los servicios de tiempo de alta precisión. Si es así, puede configurar un origen de hora manual, pero tenga en cuenta que el servicio de hora de Windows no se diseñó para funcionar como un origen de hora muy precisos. Asegúrese de que es consciente de las limitaciones de compatibilidad para entornos de tiempo de alta precisión como se describe en Microsoft Knowledge Base article 939322, [límite de soporte técnico para configurar el servicio de hora de Windows para entornos de alta precisión](support-boundary.md).  
  
-   Para configurar el servicio de hora de Windows en los equipos de cliente o servidor basado en Windows que están configurados como miembros del grupo de trabajo en lugar de los miembros del dominio ven [configura un origen de hora manual para un equipo cliente seleccionado](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816656%28v%3dws.10%29).  
  
-   Para configurar el servicio de hora de Windows en un equipo host que ejecuta un entorno virtual, consulte el artículo de Microsoft Knowledge Base 816042 [cómo configurar un servidor horario con autoridad en Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402). Si está trabajando con un producto de virtualización que no sean de Microsoft, asegúrese de consultar la documentación del proveedor para ese producto.  
  
-   Para configurar el servicio de hora de Windows en un controlador de dominio que se ejecuta en una máquina virtual, se recomienda deshabilitar parcialmente la sincronización de hora entre el sistema host invitado y de sistema operativo que actúa como un controlador de dominio. Esto permite que el controlador de dominio de invitado para sincronizar la hora de la jerarquía de dominios, pero lo protege de tener asimetría si se restaura desde un estado guardado de una hora. Para obtener más información, consulte el artículo de Microsoft Knowledge Base 976924, [recibe servicio de hora de Windows de los identificadores de evento 38, 24 y 29 en un controlador de dominio virtualizados que se está ejecutando en un servidor de host basado en Windows Server 2008 con Hyper-V](https://go.microsoft.com/fwlink/?LinkID=192236) y [consideraciones de implementación para controladores de dominio virtualizados](https://go.microsoft.com/fwlink/?LinkID=192235).  
  
-   Para configurar el servicio de hora de Windows en un controlador de dominio que actúa como el emulador de PDC de raíz de bosque que también se ejecuta en un equipo virtual, siga las mismas instrucciones para un equipo físico, como se describe en [configurar el servicio de hora de Windows en el emulador de PDC en el dominio raíz del bosque](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
-   Para configurar el servicio de hora de Windows en un servidor miembro que se ejecuta como un equipo virtual, use la jerarquía de tiempo de dominio como se describe en ([configurar un equipo cliente para la sincronización de hora automática de dominios](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
## <a name="BKMK_WTS"></a>¿Qué es el servicio de hora de Windows?  
El servicio de hora de Windows (W32Time) proporciona sincronización de relojes de red para equipos sin necesidad de numerosas tareas de configuración.  
  
El servicio de hora de Windows es esencial para el correcto funcionamiento de la autenticación Kerberos versión 5 y, por lo tanto, para autenticación AD DS. Cualquier aplicación que reconozca Kerberos, incluidos la mayoría de los servicios de seguridad se basa en la sincronización temporal entre los equipos que forman parte de la solicitud de autenticación. Controladores de dominio de AD DS deben también tener los relojes sincronizados para ayudar a garantizar la replicación de datos precisos.  
  
El servicio de hora de Windows se implementa en una biblioteca de vínculos dinámicos denominada W32Time.dll. W32Time.dll se instala de forma predeterminada en el **%Systemroot%\System32** carpeta durante la instalación y configuración del sistema operativo.  
  
W32Time.dll originalmente se desarrolló para que Windows 2000 Server admitir una especificación de mediante el protocolo de autenticación Kerberos V5 que necesita en una red que se va a sincronizar los relojes. A partir de Windows Server 2003, W32Time.dll proporcionados mayor precisión en la sincronización de reloj de red a través del sistema operativo Windows 2000 Server y, además, admite una variedad de dispositivos de hardware y los protocolos de red de tiempo por medio de tiempo proveedores. Aunque se diseñó originalmente para proporcionar sincronización de relojes para la autenticación Kerberos, muchas aplicaciones actuales usan marcas de tiempo para garantizar la coherencia transaccional, para registrar la hora de eventos importantes y otros críticos, sujeto a limitación temporal información. Estas aplicaciones se benefician sincronización temporal entre equipos suministrada por el servicio de hora de Windows.  
  
## <a name="BKMK_TimeProtocols"></a>Importancia de los protocolos de tiempo  
Protocolos de tiempo que se comunican entre dos equipos para intercambiar información de tiempo y, a continuación, usar esa información para sincronizar sus relojes. Con el protocolo de tiempo de servicio de hora de Windows, un cliente solicita información de hora de un servidor y sincroniza su reloj según la información que se recibe.  
  
El servicio de hora de Windows usa NTP para sincronizar la hora a través de una red. NTP es un protocolo de tiempo de Internet que incluye los necesarios para sincronizar los relojes de los algoritmos de disciplina. NTP es un protocolo de tiempo más preciso que el protocolo de tiempo de red Simple (SNTP) que se utiliza en algunas versiones de Windows; Sin embargo, W32Time sigue admitiendo SNTP para habilitar la compatibilidad con versiones anteriores con los equipos que ejecutan servicios de tiempo basada en SNTP como Windows 2000.  
  
## <a name="see-also"></a>Vea también  
[Cómo funciona el servicio de hora de Windows](How-the-Windows-Time-Service-Works.md)  
[Configuración y herramientas del servicio de hora de Windows](Windows-Time-Service-Tools-and-Settings.md)  
[Artículo de Microsoft Knowledge Base 902229](https://go.microsoft.com/fwlink/?LinkId=186066)