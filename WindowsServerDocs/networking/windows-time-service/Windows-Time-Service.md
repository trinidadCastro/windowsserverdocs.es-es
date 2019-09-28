---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Referencia técnica del servicio de hora de Windows
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 31c7c53a5dd28813076fcaa745093050808b5755
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405226"
---
# <a name="windows-time-service"></a>Servicio de hora de Windows

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o posterior


**En esta guía**  
  
* Dónde encontrar la información de configuración del servicio de hora de Windows  
* ¿Qué es el servicio de hora de Windows?  
* Importancia de los protocolos de tiempo  
* Funcionamiento del servicio Hora de Windows   
* Configuración y herramientas del servicio Hora de Windows  
  
> [!NOTE]  
> En Windows Server 2003 y Microsoft Windows 2000 Server, el servicio de directorio se denomina Active Directory servicio de directorio. En Windows Server 2008 R2 y Windows Server 2008, el servicio de directorio se denomina Active Directory Domain Services (AD DS). En el resto de este tema se hace referencia a AD DS, pero la información también se aplica a Active Directory Domain Services en Windows Server 2016.  
  
El servicio de hora de Windows, también conocido como W32Time, sincroniza la fecha y la hora de todos los equipos que se ejecutan en un dominio de AD DS. La sincronización de la hora es fundamental para el funcionamiento correcto de muchos servicios de Windows y aplicaciones de línea de negocio. El servicio de hora de Windows usa el protocolo de tiempo de red (NTP) para sincronizar los relojes de los equipos de la red de modo que se pueda asignar un valor de reloj exacto, o una marca de tiempo, a las solicitudes de validación de red y de acceso a recursos. El servicio integra los proveedores de NTP y hora, lo que lo convierte en un servicio de hora confiable y escalable para los administradores de empresas.
  
> [!IMPORTANT]  
> Antes de Windows Server 2016, el servicio W32Time no estaba diseñado para satisfacer las necesidades de la aplicación que depende del tiempo.  Sin embargo, las actualizaciones de Windows Server 2016 permiten ahora implementar una solución de una precisión de 1 ms en el dominio.  Consulte el límite de [tiempo](accurate-time.md) y [soporte técnico de Windows 2016 para configurar el servicio de hora de Windows para entornos de alta precisión](support-boundary.md) para obtener más información.  
  
## <a name="BKMK_Config"></a>Dónde encontrar la información de configuración del servicio de hora de Windows  
En esta guía **no** se describe la configuración del servicio de hora de Windows. Hay varios temas diferentes en Microsoft TechNet y en Microsoft Knowledge base que explican los procedimientos para configurar el servicio de hora de Windows. Si necesita información de configuración, los temas siguientes le ayudarán a localizar la información adecuada.  
  
-   Para configurar el servicio de hora de Windows para el emulador del controlador de dominio principal (PDC) raíz del bosque, consulte:  
  
    -   [Configurar el servicio de hora de Windows en el emulador de PDC del dominio raíz del bosque](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29) 
  
    -   [Configuración de un origen de hora para el bosque](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794823%28v%3dws.10%29) 
  
    -   Artículo 816042 de Microsoft Knowledge Base acerca de [Cómo configurar un servidor horario autoritativo en Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402), que describe los valores de configuración de los equipos que ejecutan windows Server 2008 R2, windows Server 2008, windows Server 2003 y Windows Servidor 2003 R2.  
  
-   Para configurar el servicio de hora de Windows en cualquier cliente o servidor miembro del dominio, o incluso controladores de dominio que no estén configurados como el emulador PDC raíz del bosque, vea [configurar un equipo cliente para la sincronización automática de la hora del dominio](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
    > [!WARNING]  
    > Es posible que algunas aplicaciones requieran que sus equipos tengan servicios de tiempo de alta precisión. En ese caso, puede optar por configurar un origen de hora manual, pero tenga en cuenta que el servicio de hora de Windows no se diseñó para funcionar como un origen de hora muy preciso. Asegúrese de que conoce las limitaciones de compatibilidad para entornos de tiempo de alta precisión, como se describe en el artículo 939322 de Microsoft Knowledge base, [compatibilidad con el límite para configurar el servicio de hora de Windows para entornos de alta precisión](support-boundary.md).  
  
-   Para configurar el servicio de hora de Windows en todos los equipos cliente o servidor basados en Windows que estén configurados como miembros del grupo de trabajo en lugar de miembros del dominio, vea [configurar un origen de hora manual para un equipo cliente seleccionado](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816656%28v%3dws.10%29).  
  
-   Para configurar el servicio de hora de Windows en un equipo host que ejecuta un entorno virtual, vea el artículo 816042 de Microsoft Knowledge base, [How to Configure a Authoritative Time Server in Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402). Si está trabajando con un producto de virtualización que no es de Microsoft, asegúrese de consultar la documentación del proveedor de ese producto.  
  
-   Para configurar el servicio de hora de Windows en un controlador de dominio que se ejecuta en una máquina virtual, se recomienda deshabilitar parcialmente la sincronización de hora entre el sistema host y el sistema operativo invitado que actúa como controlador de dominio. Esto permite que el controlador de dominio invitado sincronice la hora de la jerarquía de dominios, pero evita que tenga un sesgo horario si se restaura desde un estado guardado. Para obtener más información, vea el artículo de Microsoft Knowledge Base 976924, [que recibe los ID. de evento 24, 29 y 38 de servicio de hora de Windows en un controlador de dominio virtualizado que se ejecuta en un servidor host basado en Windows server 2008 con Hyper-V](https://go.microsoft.com/fwlink/?LinkID=192236) e [implementación. Consideraciones sobre los controladores de dominio virtualizados](https://go.microsoft.com/fwlink/?LinkID=192235).  
  
-   Para configurar el servicio de hora de Windows en un controlador de dominio que actúa como el emulador PDC raíz del bosque que también se está ejecutando en un equipo virtual, siga las mismas instrucciones para un equipo físico, tal y como se describe en [configuración del servicio de hora de Windows en el PDC. emulador en el dominio raíz del bosque](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
-   Para configurar el servicio de hora de Windows en un servidor miembro que se ejecuta como un equipo virtual, use la jerarquía de tiempo de dominio tal y como se describe en ([configurar un equipo cliente para la sincronización automática de la hora del dominio](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
## <a name="BKMK_WTS"></a>¿Qué es el servicio de hora de Windows?  
El servicio de hora de Windows (W32Time) proporciona sincronización de reloj de red para los equipos sin necesidad de una configuración extensa.  
  
El servicio de hora de Windows es esencial para el correcto funcionamiento de la autenticación Kerberos versión 5 y, por lo tanto, para la autenticación basada en AD DS. Cualquier aplicación compatible con Kerberos, incluida la mayoría de los servicios de seguridad, se basa en la sincronización de la hora entre los equipos que participan en la solicitud de autenticación. AD DS controladores de dominio también deben tener relojes sincronizados para ayudar a garantizar una replicación de datos precisa.  
  
El servicio de hora de Windows se implementa en una biblioteca de vínculos dinámicos denominada W32Time. dll. W32Time. dll se instala de forma predeterminada en la carpeta **%SystemRoot%\System32** durante la instalación e instalación del sistema operativo.  
  
W32Time. dll se desarrolló originalmente para el servidor de Windows 2000 para admitir una especificación del Protocolo de autenticación de Kerberos V5 que necesitaba sincronizar los relojes de una red. A partir de Windows Server 2003, W32Time. dll proporcionó mayor precisión en la sincronización del reloj de red en el sistema operativo Windows 2000 Server y, además, admitía una variedad de dispositivos de hardware y protocolos de tiempo de red por medio de tiempo presta. Aunque originalmente se diseñó para proporcionar sincronización de reloj para la autenticación de Kerberos, muchas aplicaciones actuales usan marcas de tiempo para garantizar la coherencia transaccional, registrar la hora de eventos importantes y otras críticas para la empresa informaciones. Estas aplicaciones se benefician de la sincronización de hora entre los equipos que proporciona el servicio de hora de Windows.  
  
## <a name="BKMK_TimeProtocols"></a>Importancia de los protocolos de tiempo  
Los protocolos de hora se comunican entre dos equipos para intercambiar información de hora y, a continuación, usan esa información para sincronizar sus relojes. Con el protocolo de hora de servicio de hora de Windows, un cliente solicita información de tiempo de un servidor y sincroniza su reloj en función de la información recibida.  
  
El servicio de hora de Windows usa NTP para ayudar a sincronizar el tiempo a través de una red. NTP es un protocolo de hora de Internet que incluye los algoritmos de disciplina necesarios para sincronizar los relojes. NTP es un protocolo de hora más preciso que el Protocolo simple de tiempo de redes (SNTP) que se usa en algunas versiones de Windows. sin embargo, W32Time sigue admitiendo SNTP para habilitar la compatibilidad con versiones anteriores con equipos que ejecutan servicios de hora basados en SNTP como Windows 2000.  
  
## <a name="see-also"></a>Vea también  
[Cómo funciona el servicio de hora de Windows](How-the-Windows-Time-Service-Works.md)  
[Configuración y herramientas del servicio Hora de Windows](Windows-Time-Service-Tools-and-Settings.md)  
[Artículo 902229 de Microsoft Knowledge base](https://go.microsoft.com/fwlink/?LinkId=186066)