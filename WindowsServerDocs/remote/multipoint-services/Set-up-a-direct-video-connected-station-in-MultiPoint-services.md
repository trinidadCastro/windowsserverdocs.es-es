---
title: Configurar una estación de vídeo directamente conectados en MultiPoint Services
description: Obtenga información sobre cómo crear una estación de vídeo directamente conectados en MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82ba3517-9743-4cde-8eea-63a17edb016f
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 58197164c91ab6b69b0ef331c025287f593f94c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850746"
---
# <a name="set-up-a-direct-video-connected-station-in-multipoint-services"></a>Configurar una estación de vídeo directamente conectados en MultiPoint Services
En una estación de vídeo conectado directa, el monitor está conectado directamente a un puerto de vídeo en el equipo servidor MultiPoint. Un teclado y mouse, a continuación, están conectados a un concentrador USB y están asociadas con el monitor.  
  
La siguiente ilustración muestra un entorno de MultiPoint Server que tiene un solo equipo de MultiPoint Server y cuatro estaciones vídeo directamente conectados. Para obtener más información, consulte [estaciones de MultiPoint Server](MultiPoint-services-Stations.md).  
  
**Sistema de multiPoint Services con cuatro conexiones directas de vídeo**  
  
![Imagen del diseño del sistema basado en USB de MultiPoint Services](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
> [!NOTE]  
> Para configurar una estación de vídeo directamente conectados, debe usar un teclado latino (por ejemplo, un teclado de idioma inglés o español).  
  
## <a name="to-set-up-a-direct-video-connected-station"></a>Para configurar una estación de vídeo conectado directa  
  
1.  Conecte el cable del monitor al puerto de vídeo en el equipo, como se muestra a continuación.  
  
    ![Image of Video connection to USB hub-based system](./media/WMSVideoConnection.gif) 
  
2.  Conecte el cable de alimentación del monitor de vídeo a una toma de corriente.  
  
3.  Conecte un concentrador USB a un puerto USB abierto en el equipo, como se muestra a continuación.  
  
    ![Imagen de conexión de concentrador USB de MultiPoint Services](./media/WMSUSBHubConnection.gif)  
  
4.  Conecte un teclado y mouse al concentrador de estaciones USB.  
  
    ![Imagen de conexiones de dispositivos de entrada de concentrador USB](./media/WMSUSBDeviceConnection.gif)  
  
5.  Conecte los periféricos adicionales, como los auriculares, al concentrador USB.  
  
6.  Si usas un concentrador alimentado de forma externa, conecte el cable de alimentación del concentrador a una toma de corriente.  
  
    > [!IMPORTANT]  
    > Se recomienda encarecidamente el uso de un concentrador alimentado. Comportamiento del sistema errático puede deberse a condiciones debajo del actual.  
    >   
    > Los usuarios no deben adjuntar teclados y ratones directamente a los puertos USB del equipo. Si lo hace, es probable que provoque la asociación de varios teclados y mouse a la misma estación o a ninguna estación incorrecta en absoluto.  
  
7.  Siga las instrucciones que aparecen en el monitor de creación de la estación.  
  
Si agrega más de una estación de vídeo conectado directa para el entorno de MultiPoint Services, puede cambiar la estación principal. Puede averiguar fácilmente que dirigir el vídeo estación conectada es la estación principal.  
  
## <a name="to-find-out-which-direct-video-connected-station-is-the-primary-station"></a>Para averiguar que dirigir estación conectada de vídeo es la estación principal  
  
1.  Activar todos los monitores que están conectados directamente a los adaptadores de pantalla del equipo (tarjetas de vídeo).  
  
2.  Inicie (o reinicie) el equipo de MultiPoint Services y vea qué monitor muestra las pantallas de inicio. Esa estación es la estación principal.  
  
    > [!NOTE]  
    > En algunos casos, se muestra información de inicio de BIOS en varios monitores simultáneamente. En ese caso, cualquiera de los monitores pueden considerarse "estación principal" con el fin de obtener acceso a la BIOS.