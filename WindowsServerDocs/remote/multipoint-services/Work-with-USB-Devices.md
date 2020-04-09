---
title: Trabajar con dispositivos USB
description: Obtenga información sobre cómo funcionan los dispositivos USB con Multipoint Services
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: a33f2b83-bbc2-4fc1-8a94-aaa985dfe1f9
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: d366e8c61da86d0e47b2ce99d08a2046c8f8bd0a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858038"
---
# <a name="work-with-usb-devices"></a>Trabajar con dispositivos USB
Puede conectar dispositivos al equipo en su sistema MultiPoint Services o a un concentrador de estaciones MultiPoint. La ubicación donde está conectado un dispositivo y el tipo de dispositivo afectan a si este está disponible para todos los usuarios del sistema, únicamente para usuarios individuales o para ningún usuario. Estos son algunos ejemplos de los distintos tipos de conexión:  
  
-   Si conecta un dispositivo directamente al equipo, como una impresora o dispositivo de almacenamiento masivo USB, todos los usuarios de la sesión pueden tener acceso al dispositivo en el sistema MultiPoint Services. Los usuarios de estaciones de escritorios virtuales no podrán acceder a dispositivos conectados directamente al equipo.  
  
-   Si conecta un dispositivo a un concentrador de estaciones, como un teclado o un mouse, un *dispositivo de audio* o un dispositivo de almacenamiento masivo, el dispositivo solo está disponible para los usuarios con sesión iniciada en esa estación MultiPoint Services.  
  
-   Si conecta ciertos tipos de dispositivos al equipo, como un teclado o un mouse, los dispositivos no están disponibles para ningún usuario del sistema.  
  
En la tabla de abajo se muestra una lista de dispositivos y su comportamiento, en función de dónde están conectados al sistema. La información sobre cómo conectar concentradores de estaciones se describe en [trabajar con concentradores de estaciones](#working-with-station-hubs). Encontrará más información sobre cómo conectar monitores de vídeo a una estación en [trabajar con dispositivos de vídeo](Work-with-Video-Devices.md).  
  
|||||  
|-|-|-|-|  
|**Dispositivos**|**Comportamiento cuando se conecta directamente al equipo**|**Comportamiento cuando se conecta a una estación**|**Apunte**|  
|Teclado|No se recomienda conectar un teclado directamente al equipo.|Accesible únicamente al usuario de la estación.|Si el teclado cuenta con un puerto USB, el concentrador USB del interior del teclado puede ser el concentrador de estaciones. Otros dispositivos USB conectados a ese puerto solo están disponibles para el usuario que usa el teclado.<p>Algunos concentradores de estaciones están equipados con un puerto de mouse PS\/2 que se convierte en una conexión USB dentro del concentrador.|  
|Mouse|No se recomienda conectar un mouse directamente al equipo.|Accesible únicamente al usuario de la estación.|Algunos concentradores de estaciones están equipados con un puerto de mouse PS\/2 que se convierte en una conexión USB dentro del concentrador.|  
|Concentrador USB|Consulte [trabajar con concentradores de estaciones](#working-with-station-hubs).|Consulte [trabajar con concentradores de estaciones](#working-with-station-hubs).||  
|Monitor de vídeo|Consulte [dispositivos de vídeo de Multipoint Services](work-with-video-devices.md).|Consulte [dispositivos de vídeo de Multipoint Services](work-with-video-devices.md).||  
|Dispositivos de salida de audio como auriculares|No se recomienda conectar un dispositivo de salida de audio directamente al equipo.|Accesible únicamente al usuario de la estación.|Algunos concentradores de estaciones están equipados con un puerto de audio analógico que se convierte en una conexión de audio USB dentro del concentrador.|  
|Dispositivos de entrada de audio como micrófonos|No se recomienda conectar un dispositivo de entrada de audio directamente al equipo.|Accesible únicamente al usuario de la estación.|Algunos concentradores de estaciones están equipados con un puerto de audio analógico que se convierte en una conexión de audio USB dentro del concentrador.|  
|Impresoras|Accesible para todos los usuarios del sistema. *|Accesible únicamente al usuario de la estación.||  
|Dispositivo de almacenamiento masivo USB|Accesible para todos los usuarios del sistema.\*|Accesible únicamente al usuario de la estación.|Estos dispositivos incluyen unidades flash USB, unidades de disco duro externas y cámaras digitales.|  
|Cámaras web|Accesible para todos los usuarios del sistema. *|Accesible únicamente al usuario de la estación.|Solo un usuario puede conectar la cámara a la vez.|  
  
\* Los dispositivos que están conectados al equipo host no son visibles para los usuarios que han iniciado sesión en estaciones de escritorios virtuales.  
  
Para más información sobre cómo configurar una estación, vea [Configurar una estación](Set-Up-a-Station.md).  
  
### <a name="working-with-station-hubs"></a>Trabajar con concentradores de estaciones  
Hay cuatro escenarios para usar un concentrador USB cuando está conectado a un sistema MultiPoint Services. Cada uno de los siguientes escenarios proporciona un acceso diferente a los dispositivos conectados a él, en función del tipo de concentrador y de dónde está conectado al sistema.  
  
-   Se puede usar un concentrador de estaciones conectado al equipo en el sistema MultiPoint Services con un teclado conectado para crear una estación de MultiPoint Services. El teclado y el mouse están conectados al concentrador de estaciones a través de los puertos disponibles en el concentrador. Un monitor de vídeo está conectado a un puerto de vídeo en el equipo o a un adaptador de vídeo en el concentrador de estaciones, si está disponible. El teclado, el mouse y el monitor *se asocian* entonces a una estación de MultiPoint Services.  
  
-   Se puede usar un concentrador USB conectado al equipo en el sistema MultiPoint Services sin teclado conectado, para conectar otros dispositivos al equipo cuando no haya suficientes puertos en el equipo para los dispositivos necesarios. Todos los dispositivos conectados a este concentrador USB están disponibles para todos los usuarios del sistema MultiPoint Services. Esto no se considera un concentrador de estaciones de MultiPoint Services.  
  
-   Se puede usar un concentrador USB alimentado conectado al equipo en su sistema MultiPoint Services, también conocido como concentrador intermedio, para conectar concentradores USB adicionales que se usan para crear estaciones MultiPoint.  
  
-   Se puede usar un concentrador USB conectado a un concentrador de estaciones para conectar dispositivos adicionales al concentrador de estaciones. Los teclados deben conectarse directamente al concentrador de estaciones.  
  
Para más información sobre cómo configurar una estación de MultiPoint Services, vea [Configurar una estación](Set-Up-a-Station.md).  
  
## <a name="see-also"></a>Consulta también  
[Trabajar con dispositivos de vídeo](Work-with-Video-Devices.md)  
[Administrar hardware de la estación](Manage-Station-Hardware.md)  
[Configurar una estación](Set-Up-a-Station.md)