---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Referencia técnica del servicio de hora de Windows
description: El servicio W32Time proporciona sincronización del reloj de red para los equipos sin necesidad de una extensa configuración. El servicio W32Time es esencial para el correcto funcionamiento de la autenticación Kerberos, versión 5, y, por tanto, para la autenticación basada en AD DS.
author: dcuomo
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: ec141fe8a5e2b1acf0a9f6627dd2d394382424bb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859828"
---
# <a name="windows-time-service-technical-reference"></a>Referencia técnica del servicio de hora de Windows
>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o posterior

El servicio W32Time proporciona sincronización del reloj de red para los equipos sin necesidad de una extensa configuración. El servicio W32Time es esencial para el correcto funcionamiento de la autenticación Kerberos, versión 5, y, por tanto, para la autenticación basada en AD DS. Cualquier aplicación que reconozca Kerberos, incluida la mayoría de los servicios de seguridad, se basa en la sincronización de la hora entre los equipos que participan en la solicitud de autenticación. Los controladores de dominio de AD DS también deben tener los relojes sincronizados para garantizar que la replicación de los datos sea precisa.

> [!NOTE]  
> En Windows Server 2003 y Microsoft Windows 2000 Server, el servicio de directorio se denomina Servicio de directorio de Active Directory. En Windows Server 2008 R2 y Windows Server 2008, el servicio de directorio se denomina Active Directory Domain Services (AD DS). El resto de este tema hace referencia a AD DS, pero la información también se puede aplicar a Active Directory Domain Services en Windows Server 2016.

El servicio W32Time está implementado en una biblioteca de vínculos dinámicos denominada W32Time.dll, que se instala de forma predeterminada en **%Systemroot%\System32**. W32Time.dll se desarrolló originalmente para que Windows 2000 Server admitiera una especificación del protocolo de autenticación de Kerberos versión 5 que requería la sincronización de los relojes de una red. A partir de Windows Server 2003, W32Time.dll proporcionaba mayor precisión en la sincronización del reloj de red hasta el sistema operativo Windows Server 2000. Además, en Windows Server 2003, W32Time.dll admitía una variedad de dispositivos de hardware y protocolos de hora de red mediante proveedores de hora.

Aunque originalmente se diseñó para proporcionar sincronización de reloj para la autenticación de Kerberos, muchas aplicaciones actuales usan marcas de tiempo para garantizar la coherencia transaccional, registrar la hora de eventos importantes y demás información crítica sujeta a limitaciones temporales.  Estas aplicaciones se benefician de la sincronización de la hora entre los equipos que proporciona el servicio de hora de Windows.

## <a name="importance-of-time-protocols"></a>Importancia de los protocolos de hora
Los protocolos de hora se comunican entre dos equipos para intercambiar información de hora y, luego, usan esa información para sincronizar sus relojes. Con el protocolo de hora del servicio de hora de Windows, un cliente solicita información de hora a un servidor y sincroniza su reloj en función de la información recibida.
  
El servicio de hora de Windows usa NTP para ayudar a sincronizar la hora a través de una red. NTP es un protocolo de hora de Internet que incluye los algoritmos de sincronización necesarios para sincronizar los relojes. NTP es un protocolo de hora más preciso que el Protocolo simple de tiempo de redes (SNTP) que se usa en algunas versiones de Windows; sin embargo, W32Time sigue admitiendo SNTP para habilitar la compatibilidad con versiones anteriores con equipos que ejecutan servicios de hora basados en SNTP, como Windows 2000.
## <a name="where-to-find-windows-time-service-configuration-related-information"></a>Dónde encontrar la información relacionada con la configuración del servicio de hora de Windows  
En esta guía **no** se analiza la configuración del servicio de hora de Windows. Hay varios temas diferentes en Microsoft TechNet y en Microsoft Knowledge Base que sí explican los procedimientos para configurar el servicio de hora de Windows. Si necesitas información de configuración, los temas siguientes te ayudarán a ubicar la información adecuada.  
-   Para configurar el servicio de hora de Windows para el emulador del controlador de dominio principal (PDC) raíz del bosque, consulta:
  
    -   [Configuración del servicio de hora de Windows en el emulador de PDC del dominio raíz del bosque](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29) 
  
    -   [Configuración de un origen de la hora para el bosque](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794823%28v%3dws.10%29) 
  
    -   En el artículo 816042 de Microsoft Knowledge Base, [Cómo configurar un servidor horario autoritativo en Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402), que describe los valores de configuración de los equipos que ejecutan Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 y Windows Server 2003 R2.  
  
-   Para configurar el servicio de hora de Windows en cualquier cliente o servidor miembro del dominio, o incluso en controladores de dominio que no estén configurados como el emulador de PDC de la raíz del bosque, consulta [Configuración de un equipo cliente para la sincronización automática de hora del dominio](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
    > [!WARNING]  
    > Es posible que algunas aplicaciones requieran que sus equipos tengan servicios de hora de alta precisión. En ese caso, puedes optar por configurar un origen de hora manual, pero ten en cuenta que el servicio de la hora de Windows no se diseñó para funcionar como origen de hora de alta precisión. Asegúrate de conocer las limitaciones de compatibilidad para los entornos de hora de alta precisión, como se describe en el artículo 939322 de Microsoft Knowledge Base, [Límite de compatibilidad para configurar el servicio de hora de Windows para entornos de alta precisión](support-boundary.md).  
  
-   Para configurar el servicio de hora de Windows en equipos cliente o servidor basados en Windows que estén configurados como miembros del grupo de trabajo en lugar de miembros del dominio, consulta [Configuración de un origen de la hora manual para un equipo cliente seleccionado](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816656%28v%3dws.10%29).  
  
-   Para configurar el servicio de hora de Windows en un equipo host que ejecuta un entorno virtual, consulta el artículo 816042 de Microsoft Knowledge Base, [Cómo configurar un servidor horario autoritativo en Windows Server](https://go.microsoft.com/fwlink/?LinkID=60402). Si trabajas con un producto de virtualización que no es de Microsoft, asegúrate de consultar la documentación del proveedor de ese producto.  
  
-   Para configurar el servicio de hora de Windows en un controlador de dominio que se ejecuta en una máquina virtual, se recomienda deshabilitar parcialmente la sincronización de hora entre el sistema host y el sistema operativo invitado que funciona como controlador de dominio. Esto permite que el controlador de dominio invitado sincronice la hora de la jerarquía de dominios, pero evita que tenga un sesgo horario si se restaura a partir de un estado guardado. Para obtener más información, consulta el artículo 976924 de Microsoft Knowledge Base, [Recibir servicio de hora de Windows de los identificadores de sucesos 24, 29 y 38 en un controlador de dominio virtualizado que se ejecuta en un servidor basado en Windows Server 2008 con Hyper-V](https://go.microsoft.com/fwlink/?LinkID=192236) y [Consideraciones de implementación para controladores de dominio virtualizados](https://go.microsoft.com/fwlink/?LinkID=192235).  
  
-   Para configurar el servicio de hora de Windows en un controlador de dominio que funciona como emulador PDC raíz del bosque que también se ejecuta en un equipo virtual, sigue las mismas instrucciones que para un equipo físico, tal como se describe en [Configuración del servicio de hora de Windows en el emulador de PDC del dominio raíz del bosque](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
-   Para configurar el servicio de hora de Windows en un servidor miembro que se ejecuta como equipo virtual, usa la jerarquía de hora del dominio, tal y como se describe en [Configuración de un equipo cliente para la sincronización automática de hora del dominio](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).


> [!IMPORTANT]  
> Antes de Windows Server 2016, el servicio W32Time no estaba diseñado para satisfacer las necesidades de las aplicaciones sujetas a limitaciones temporales.  Sin embargo, las actualizaciones de Windows Server 2016 ahora permiten implementar una solución con una precisión de 1 ms en el dominio.  Para obtener más información, consulta [Hora precisa para Windows Server 2016](accurate-time.md) y [Límite de compatibilidad para configurar el servicio de hora de Windows para entornos de alta precisión](support-boundary.md).

## <a name="related-topics"></a>Temas relacionados
- [Hora precisa para Windows Server 2016](accurate-time.md)
- [Mejoras a la precisión temporal para Windows Server 2016](windows-server-2016-improvements.md)  
- [Funcionamiento del servicio Hora de Windows](How-the-Windows-Time-Service-Works.md)  
- [Configuración y herramientas del servicio Hora de Windows](Windows-Time-Service-Tools-and-Settings.md)  
- [Límite de compatibilidad para configurar el servicio de hora de Windows para entornos de alta precisión](support-boundary.md)
- [Artículo 902229 de Microsoft Knowledge Base](https://go.microsoft.com/fwlink/?LinkId=186066)
