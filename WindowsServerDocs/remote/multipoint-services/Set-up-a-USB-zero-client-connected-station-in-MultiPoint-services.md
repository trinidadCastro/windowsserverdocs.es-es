---
title: Configurar un puerto USB cero estación cliente conectado en MultiPoint Services
description: Aprenda a crear un puerto USB cero estación de cliente en MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d2908865-6be3-474d-88f1-995f40bb61d0
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 1a64373f4ed5e0d1ac96a0257ac5697ff94ffcbe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878936"
---
# <a name="set-up-a-usb-zero-client-connected-station-in-multipoint-services"></a>Configurar un puerto USB cero estación cliente conectado en MultiPoint Services
Cuando usas clientes USB cero para crear estaciones de MultiPoint Services, el monitor para cada estación está conectado al puerto de vídeo en el zero client mediante USB, tal como se muestra en la siguiente ilustración. Para obtener más información sobre este y otros tipos de estación, vea [estaciones de MultiPoint](MultiPoint-services-Stations.md).
  
**Sistema de multiPoint Services con una estación de vídeo directamente conectados y dos estaciones USB cero cliente conectado**  
  
![Estaciones conectadas a Zero Client mediante USB](./media/WMS11_diagram7.gif)  
  
> [!IMPORTANT]  
> Antes de configurar USB cero estaciones cliente conectado, asegúrese de instalar a los controladores más recientes para sus tarjetas de vídeo y el zero client mediante USB. Controladores obsoletos pueden evitar que la configuración de MultiPoint Services complete correctamente. Para obtener instrucciones, consulte [Update e instale los controladores de dispositivos si es necesario](Update-and-install-device-drivers-if-needed.md).  
  
> [!IMPORTANT]  
> Si usa a un cliente de USB a través de Ethernet cero, siga las instrucciones del proveedor, en lugar de este procedimiento, para usar la conexión Ethernet para configurar el dispositivo en la red.  
  
### <a name="to-set-up-a-usb-zero-client-connected-station"></a>Para configurar un puerto USB cero estación cliente conectado  
  
1.  Conecte el cable de monitor de vídeo al puerto de vídeo DVI o VGA en USB cero cliente, tal como se muestra en la siguiente ilustración.  
  
    ![Image of Video connection to USB hub-based system](./media/WMSVideoConnection.gif)  
  
2.  Conecte al zero client mediante USB a un puerto USB abierto en el equipo.  
  
    ![Imagen de conexión de concentrador USB de MultiPoint Services](./media/WMSUSBHubConnection.gif)  
  
3.  Conecte un teclado y mouse a la zero client mediante USB.  
  
    ![Imagen de conexiones de dispositivos de entrada de concentrador USB](./media/WMSUSBDeviceConnection.gif)  
  
4.  Si usas a un alimentado de forma externa zero client mediante USB, conecte el cable de alimentación de la zero client mediante USB a una toma de corriente.  
  
5.  Conecte el cable de alimentación del monitor de vídeo a una toma de corriente.  
  
6.  Si se le pide para asociar dispositivos a la estación, siga las instrucciones en el monitor para completar la instalación. (Por lo general, USB cero estaciones conectadas por el cliente están asociadas las estaciones automáticamente según se agregan al servidor.)