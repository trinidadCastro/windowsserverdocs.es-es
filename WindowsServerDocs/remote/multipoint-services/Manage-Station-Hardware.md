---
title: Administrar hardware de la estación
description: Proporciona información general sobre cómo administrar el hardware de estaciones MultiPoint
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 429b8539-b17a-4e01-9576-860600466451
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 3a1cdfd12c9bd6a21fec9cbfffae04573ef5ea98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841636"
---
# <a name="manage-station-hardware"></a>Administrar hardware de la estación
Un sistema MultiPoint Services consta de un único equipo y al menos una estación. Hardware de la estación normalmente consta de un concentrador de estaciones, mouse, teclado y monitor de vídeo. Por lo general, las estaciones están unidas físicamente mediante un cable al equipo.  
  
En la ilustración siguiente se muestra un ejemplo de un diseño de un sistema MultiPoint Services que tiene cuatro estaciones. Cada estación está conectada al equipo de MultiPoint Services mediante el uso de un concentrador USB y tarjetas de vídeo multimonitor. En esta ilustración no representan las estaciones que están conectadas mediante el uso de concentradores multifunción.  
   
![Imagen del diseño del sistema basado en USB de MultiPoint Services](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
En los temas de esta sección se describe cómo se puede ver el estado del hardware conectado al sistema MultiPoint Services y se proporciona información detallada sobre los tipos de dispositivos USB y otros dispositivos de hardware periféricos que se pueden usar para configurar una estación de MultiPoint Services. Aquí describimos brevemente los temas de esta sección con los que podrá elegir hardware y configurar la estación de MultiPoint Services.  
  
## <a name="view-hardware-status"></a>Ver el estado del hardware  
En el Administrador de MultiPoint, puede usar el **estaciones** pestaña para ver el estado de las estaciones y dispositivos de hardware que están conectados al equipo de MultiPoint Services. Para más información sobre cómo ver el estado del equipo de MultiPoint Services y de los dispositivos de hardware conectados a él, vea el tema [Ver el estado del hardware](View-Hardware-Status.md).  
  
## <a name="work-with-usb-devices"></a>Trabajar con dispositivos USB  
Cuando los dispositivos USB y otros dispositivos de hardware periféricos están conectados al sistema MultiPoint Services, funcionan de forma diferente cuando están conectados al equipo de MultiPoint Services que cuando están conectados a una estación de MultiPoint Services. En el tema [Trabajar con dispositivos USB](Work-with-USB-Devices.md) se describe cómo podrían funcionar los diferentes dispositivos de hardware en estas situaciones y se proporciona información detallada sobre cómo trabajar con concentradores de estaciones.  
  
## <a name="work-with-video-devices"></a>Trabajar con dispositivos de vídeo  
En el tema [Trabajar con dispositivos de vídeo](Work-with-Video-Devices.md) se describe cómo funcionan los dispositivos de vídeo, como un monitor o un proyector, cuando están conectados a un equipo en el sistema MultiPoint Services o en una estación de MultiPoint Services.  
  
## <a name="set-up-a-station"></a>Configurar una estación  
En el tema [Configurar una estación](Set-Up-a-Station.md) se describe cómo conectar los dispositivos periféricos de hardware a un concentrador de estaciones de MultiPoint Services para crear una estación de MultiPoint Services. MultiPoint Services admite dos tipos de concentradores de estación:  
  
-   Un puerto de múltiples externamente con tecnología o alimentado por bus *concentrador USB* que puede admitir un mouse, teclado y otros dispositivos periféricos USB  
  
-   Un *concentrador multifunción* que puede incluir un controlador de vídeo integrado y puertos para el mouse, el teclado y dispositivos periféricos de audio.  
  
Ambos tipos de concentradores de estaciones se conectan al equipo mediante un cable USB. Los procedimientos del tema [Configurar una estación](Set-Up-a-Station.md) describen cómo conectar dispositivos de hardware a cada tipo de concentrador de estaciones.  
  
## <a name="see-also"></a>Vea también  
[Ver el estado de Hardware](View-Hardware-Status.md)  
[Trabajar con dispositivos USB](Work-with-USB-Devices.md)  
[Trabajar con dispositivos de vídeo](Work-with-Video-Devices.md)  
[Configurar una estación](Set-Up-a-Station.md)