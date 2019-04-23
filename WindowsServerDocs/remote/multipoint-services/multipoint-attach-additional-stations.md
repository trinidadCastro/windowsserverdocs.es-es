---
title: Asociar estaciones adicionales a su servidor MultiPoint
description: Agregar estaciones más a la implementación de MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d78ebf4e-0968-4014-9a42-9f75cc50cb52
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: 57fc8ed6774c3266298ecd98e8f609ec01f63ef6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889266"
---
# <a name="attach-additional-stations-to-multipoint-services"></a>Adjuntar adicionales estaciones en MultiPoint Services
En su entorno de MultiPoint Services, los usuarios utilizan estaciones para conectar con MultiPoint Services y realizar su trabajo. Las estaciones son los puntos de conexión de usuario para conectarse al equipo que ejecuta Multipoint Services.  
  
MultiPoint services admite tres tipos de estación:  
  
-   Estaciones de vídeo directamente conectados  
  
-   USB cero cliente conectado estaciones (y USB a través de las estaciones de cliente conectado Ethernet cero)  
  
-   Estaciones conectadas de RDP a través de LAN  
  
Las clasificaciones se basan en hardware de la estación y el tipo de conexión que utiliza. Se puede mezclar y combinar los tipos de conexión para sus estaciones. El único requisito es que la estación principal (que instaló anteriormente) debe ser una estación de vídeo directamente conectados. Para obtener más información acerca de las configuraciones de estación, vea [estaciones de MultiPoint](MultiPoint-services-Stations.md).  
  
Para obtener instrucciones que explican cómo configurar cada tipo de estación, vea lo siguiente:  
  
-   [Configurar una estación de vídeo directamente conectados](Set-up-a-direct-video-connected-station-in-MultiPoint-services.md)  
  
-   [Configurar un puerto USB cero estación cliente conectado](Set-up-a-USB-zero-client-connected-station-in-MultiPoint-services.md)  
  
-   [Configurar una estación de RDP a través de LAN conectados](Set-up-an-RDP-over-LAN-connected-station-in-MultiPoint-services.md)  
  
Para obtener una comparación detallada de los tipos de estación, vea [comparación de tipo estación](multipoint-services-stations.md#BKMK_StationTypeComparison).  
  
> [!NOTE]  
> -   Los procedimientos para asociar las estaciones no describen cómo configurar centros intermedios o a centros de nivel inferiores. Para obtener información sobre dónde instalar estos centros, consulte [estaciones de MultiPoint](MultiPoint-services-Stations.md).  
> -   En algunos casos, es posible que necesita crear estaciones de escritorios virtuales, que se ejecutan en máquinas virtuales. Por ejemplo, usa las aplicaciones que no se puede instalar en Windows Server o aplicaciones que no se ejecutarán varias instancias en el mismo equipo host. Para obtener más información, consulte [escritorios virtuales de creación de Windows 10 Enterprise para estaciones](Create-Windows-10-Enterprise-virtual-desktops-for-stations.md).  
  
> [!TIP]  
> Es útil crear sus estaciones en el orden de sus ubicaciones físicas para que se identifican de forma secuencial en MultiPoint Server. Si posteriormente desea cambiar el nombre de una estación, puede hacerlo en MultiPoint Manager. Para obtener más información, consulte la reasignación de todas las estaciones de MultiPoint Server ayuda y soporte técnico.