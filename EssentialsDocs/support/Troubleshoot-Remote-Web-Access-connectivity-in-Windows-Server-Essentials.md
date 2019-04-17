---
title: Solucionar problemas de conectividad de acceso Web remoto en Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3642575-b3ee-4488-b654-5bf9d3b8c935
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: af4725fd3b1861c847434e3ed62c3da030689fb5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-remote-web-access-connectivity-in-windows-server-essentials"></a>Solucionar problemas de conectividad de acceso Web remoto en Windows Server Essentials
 
>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Por lo general, Windows Server Essentials puede configurar automáticamente un enrutador de banda ancha si el enrutador es un certificado de dispositivo y si se habilita la configuración de UPnP en el enrutador por UPnP.  
  
## <a name="possible-issues"></a>Posibles problemas  
 Puedes encontrar los siguientes problemas con la conectividad de acceso Web remoto:  
  
-   El enrutador no está activado o no está conectado a la red.  
  
-   La configuración de UPnP para el enrutador está desactivada.  
  
-   El enrutador no podrá totalmente compatible con el estándar de UPnP. Microsoft mantiene una lista de enrutadores que funcionan con los sistemas operativos Windows. Para ver la lista de enrutadores (incluidos los enrutadores inalámbricos) que son compatibles con Windows Server Essentials, visite la [centro de compatibilidad de Windows](https://www.microsoft.com/windows/compatibility/CompatCenter/Home).  
  
## <a name="possible-fixes"></a>Posibles soluciones  
 Las siguientes acciones pueden solucionar estos problemas:  
  
-   Comprueba que el enrutador está activado y funciona correctamente.  
  
-   Asegúrate de que el servidor está conectado directamente al enrutador o que está conectado a un conmutador que está conectado al enrutador.  
  
-   Comprueba que el dispositivo de banda ancha que se conecta a tu proveedor de servicios de Internet (ISP) está activado, funciona correctamente, y que el enrutador está conectado al dispositivo de banda ancha.  
  
-   Activar la configuración de UPnP del enrutador. Conectarse a la página web de configuración del enrutador activar la configuración de UPnP. Para obtener información sobre cómo iniciar sesión en el enrutador y cómo activar la configuración de UPnP, consulta la documentación del enrutador. Después de activar la configuración de UPnP, ejecutar activa en remoto Web acceso nuevo el Asistente para configurar el enrutador.  
  
-   Si el enrutador no es totalmente compatible con el estándar de UPnP, no puede configurarse automáticamente. Manualmente, debes configurar el enrutador o comprar un enrutador que admite el estándar de UPnP.  
  
     Para configurar manualmente enrutador, completa las siguientes tareas:  
  
    -   Crear la reserva de la dirección IP del servidor de Windows Server Essentials.  
  
         Antes de configurar manualmente el enrutador para reenviar los puertos necesarios para Windows Server Essentials, debes configurar una reserva de protocolo de configuración dinámica de Host (DHCP) para que el servidor que ejecuta Windows Server Essentials en el enrutador. Este paso, garantiza que la dirección IP que se va a reenviar los puertos que no cambie.  
  
         Para obtener información sobre cómo configurar manualmente una reserva DHCP de tu servidor del enrutador, consulta la documentación de s del fabricante del enrutador.  
  
    -   Configurar el reenvío de puerto del enrutador para los siguientes puertos:  
  
        |Servicio o protocolo|Puerto|  
        |-------------------------|----------|  
        |HTTP|TCP 80|  
        |HTTPS|TCP 443|  
  
     Para obtener información acerca de cómo configurar manualmente el reenvío de puerto del enrutador, consulta la documentación del fabricante s.  
  
     Una página de configuración de enrutador típico incluye una tabla similar al siguiente.  
  
    > [!NOTE]
    >  En esta tabla, la dirección IP del equipo que ejecuta Windows Server Essentials es 192.168.0.100. Debes determinar la dirección IP del equipo y sustituir esa dirección IP para la dirección IP que se muestra en la tabla.  
  
    |Dirección IP|Protocolo (TCP o UDP)|Programación|Filtro de entrada|  
    |----------------|---------------------------|--------------|--------------------|  
    |192.168.0.100|TCP 80|Siempre|Permitir todo|  
    |192.168.0.100|TCP 443|Siempre|Permitir todo|  
  
     Después de configurar manualmente el enrutador, ejecutar la activa en Web Asistente para acceso remoto, siempre que seleccione la **el programa de instalación de omitir router** opción en la **Introducción** página.  
  
-   Compra un nuevo enrutador si el enrutador no es totalmente compatible con el estándar de UPnP.  
  
> [!TIP]
>  Asegúrate de que el enrutador tiene instalado el firmware de BIOS más reciente. A menudo puede actualizar el firmware de BIOS para el enrutador de la página web de configuración del enrutador. Para obtener más información, consulta la documentación del enrutador. Después de actualiza el enrutador, ejecuta al desde cualquier lugar acceso asistente Configurar.  
  
## <a name="see-also"></a>Consulta también  
  
-   [Usar el acceso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Administrar el acceso Web remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Administrar el acceso desde cualquier lugar](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [Compatibilidad con Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [Compatibilidad con Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

