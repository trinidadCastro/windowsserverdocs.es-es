---
title: Configuración de una estación conectada a vídeo directo en Multipoint Services
description: Aprenda a crear una estación conectada a vídeo directo en Multipoint Services
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 82ba3517-9743-4cde-8eea-63a17edb016f
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 4bab07a21c6e2de529797e240325b3ff27446fe0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859358"
---
# <a name="set-up-a-direct-video-connected-station-in-multipoint-services"></a>Configuración de una estación conectada a vídeo directo en Multipoint Services
En una estación conectada a vídeo directa, el monitor está conectado directamente a un puerto de vídeo en el equipo de MultiPoint Server. Un teclado y un mouse se conectan a un concentrador USB y se asocian al monitor.  
  
En la ilustración siguiente se muestra un entorno de servidor Multipoint que tiene un único equipo de MultiPoint Server y cuatro estaciones conectadas a vídeo directo. Para obtener más información, consulte [estaciones de MultiPoint Server](MultiPoint-services-Stations.md).  
  
**Sistema Multipoint Services con cuatro conexiones de vídeo directas**  
  
![Imagen del diseño del sistema basado en USB de Multipoint Services](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
> [!NOTE]  
> Para configurar una estación conectada directamente a vídeo, debe usar un teclado latino (por ejemplo, un teclado de idioma español o inglés).  
  
## <a name="to-set-up-a-direct-video-connected-station"></a>Para configurar una estación conectada a vídeo directa  
  
1.  Conecte el cable del monitor al puerto de visualización del vídeo en el equipo, como se muestra a continuación.  
  
    ![Image of Video connection to USB hub-based system](./media/WMSVideoConnection.gif) 
  
2.  Conecte el cable de alimentación del monitor de vídeo a una toma de corriente.  
  
3.  Conecte un concentrador USB a un puerto USB abierto en el equipo, como se muestra a continuación.  
  
    ![Imagen de conexión del concentrador USB de Multipoint Services](./media/WMSUSBHubConnection.gif)  
  
4.  Conecte un teclado y un mouse al concentrador de estaciones USB.  
  
    ![Imagen de conexiones de dispositivos de entrada de concentrador USB](./media/WMSUSBDeviceConnection.gif)  
  
5.  Conecte los periféricos adicionales, como los auriculares, al concentrador USB.  
  
6.  Si usa un concentrador alimentado externamente, conecte el cable de alimentación del centro a una toma de corriente.  
  
    > [!IMPORTANT]  
    > Se recomienda encarecidamente el uso de un concentrador controlado. El comportamiento errático del sistema puede deberse a condiciones de uso inferior.  
    >   
    > Los usuarios no deben adjuntar los ratones y los teclados directamente a los puertos USB del equipo. Si lo hace, es probable que se produzca una asociación incorrecta de varios teclados y ratones a la misma estación o que no haya ninguna estación.  
  
7.  Siga las instrucciones que aparecen en el monitor para crear la estación.  
  
Si agrega más de una estación conectada a vídeo directa al entorno de Multipoint Services, la estación principal podría cambiar. Puede averiguar fácilmente qué estación conectada de vídeo directo es la estación principal.  
  
## <a name="to-find-out-which-direct-video-connected-station-is-the-primary-station"></a>Para averiguar qué estación conectada de vídeo directo es la estación principal  
  
1.  Active todos los monitores que están conectados directamente a los adaptadores de pantalla (tarjetas de vídeo) del equipo.  
  
2.  Inicie (o reinicie) el equipo de Multipoint Services y vea qué monitor muestra las pantallas de inicio. Esa estación es la estación principal.  
  
    > [!NOTE]  
    > En algunos casos, la información de inicio del BIOS se muestra en varios monitores simultáneamente. En ese caso, cualquiera de los monitores se puede considerar la "estación principal" con el fin de obtener acceso al BIOS.