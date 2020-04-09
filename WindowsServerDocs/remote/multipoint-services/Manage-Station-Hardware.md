---
title: Administrar hardware de la estación
description: Proporciona información general sobre cómo administrar el hardware de las estaciones multipoint.
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 429b8539-b17a-4e01-9576-860600466451
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 8ffb6fd714293471a0e9aa020390943b201261c4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853528"
---
# <a name="manage-station-hardware"></a>Administrar hardware de la estación
Un sistema MultiPoint Services consta de un solo equipo y de al menos una estación. El hardware de la estación consta habitualmente de un concentrador de estaciones, un mouse, un teclado y un monitor de vídeo. Por lo general, las estaciones están unidas físicamente mediante un cable al equipo.  
  
En la ilustración siguiente se muestra un ejemplo de un diseño de un sistema MultiPoint Services que tiene cuatro estaciones. Cada estación está conectada al equipo de Multipoint Services mediante el uso de un concentrador USB y tarjetas de vídeo de varios monitores. En esta ilustración no se representan las estaciones conectadas mediante concentradores multifunción.  
   
![Imagen del diseño del sistema basado en USB de Multipoint Services](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
En los temas de esta sección se describe cómo se puede ver el estado del hardware conectado al sistema MultiPoint Services y se proporciona información detallada sobre los tipos de dispositivos USB y otros dispositivos de hardware periféricos que se pueden usar para configurar una estación de MultiPoint Services. Aquí describimos brevemente los temas de esta sección con los que podrá elegir hardware y configurar la estación de MultiPoint Services.  
  
## <a name="view-hardware-status"></a>Ver el estado del hardware  
En Multipoint Manager, puede usar la pestaña **estaciones** para ver el estado de las estaciones y los dispositivos de hardware que están conectados al equipo de Multipoint Services. Para más información sobre cómo ver el estado del equipo de MultiPoint Services y de los dispositivos de hardware conectados a él, vea el tema [Ver el estado del hardware](View-Hardware-Status.md).  
  
## <a name="work-with-usb-devices"></a>Trabajar con dispositivos USB  
Cuando los dispositivos USB y otros dispositivos de hardware periféricos están conectados al sistema MultiPoint Services, funcionan de forma diferente cuando están conectados al equipo de MultiPoint Services que cuando están conectados a una estación de MultiPoint Services. En el tema [Trabajar con dispositivos USB](Work-with-USB-Devices.md) se describe cómo podrían funcionar los diferentes dispositivos de hardware en estas situaciones y se proporciona información detallada sobre cómo trabajar con concentradores de estaciones.  
  
## <a name="work-with-video-devices"></a>Trabajar con dispositivos de vídeo  
En el tema [Trabajar con dispositivos de vídeo](Work-with-Video-Devices.md) se describe cómo funcionan los dispositivos de vídeo, como un monitor o un proyector, cuando están conectados a un equipo en el sistema MultiPoint Services o en una estación de MultiPoint Services.  
  
## <a name="set-up-a-station"></a>Configurar una estación  
En el tema [Configurar una estación](Set-Up-a-Station.md) se describe cómo conectar los dispositivos periféricos de hardware a un concentrador de estaciones de MultiPoint Services para crear una estación de MultiPoint Services. MultiPoint Services admite dos tipos de concentradores de estación:  
  
-   Un *concentrador USB* de varios puertos alimentado de forma externa que puede admitir un mouse, un teclado y otros dispositivos PERIFÉRICOs USB.  
  
-   Un *concentrador multifunción* que puede incluir un controlador de vídeo integrado y puertos para el mouse, el teclado y dispositivos periféricos de audio.  
  
Ambos tipos de concentradores de estaciones se conectan al equipo mediante un cable USB. Los procedimientos del tema [Configurar una estación](Set-Up-a-Station.md) describen cómo conectar dispositivos de hardware a cada tipo de concentrador de estaciones.  
  
## <a name="see-also"></a>Consulta también  
[Ver el estado del hardware](View-Hardware-Status.md)  
[Trabajar con dispositivos USB](Work-with-USB-Devices.md)  
[Trabajar con dispositivos de vídeo](Work-with-Video-Devices.md)  
[Configurar una estación](Set-Up-a-Station.md)