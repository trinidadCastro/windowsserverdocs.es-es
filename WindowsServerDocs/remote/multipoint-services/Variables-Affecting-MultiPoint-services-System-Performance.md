---
title: Variables que afectan al rendimiento del sistema MultiPoint Services
description: Información de rendimiento de MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f3e8875-1b5e-4789-b16c-d06d6e31f38e
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 7a06fcc763283114dc12ad106aa7ec146502dbd9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867586"
---
# <a name="variables-affecting-multipoint-services-system-performance"></a>Variables que afectan al rendimiento del sistema MultiPoint Services
Hay muchas variables que pueden afectar al rendimiento global del sistema MultiPoint Services. Puede considerar estas al diseñar el sistema.  
  
## <a name="usage"></a>Uso  
  
-   **Aplicaciones** el tipo y número de aplicaciones que se ejecutan al mismo tiempo, especialmente gráfico\-afectarán al rendimiento general del sistema aplicaciones con uso intensivo de memoria o con mucha actividad. Para obtener más información, consulte [aplicaciones y contenido de Internet](hardware-and-performance-recommendations.md#applications-and-internet-content).  
  
-   **Uso de Internet** considere si los usuarios verán contenido multimedia o páginas web que usan todos los movimientos vídeos. Este tipo de contenido puede sobrecargar el sistema si hay demasiados usuarios están viendo simultáneamente.  
  
    > [!NOTE]  
    > La función de proyección en MultiPoint Services, que permite a los profesores a proyectar sus pantallas en sus monitores de estudiante, no está diseñada para vídeo de todos los movimientos de proyecto. La característica de proyección está diseñada para fines de demostración, como mostrar un procedimiento.  
  
-   **Dispositivos de alta velocidad** si hay demasiados usuarios simultáneamente son usar un dispositivo de alta velocidad, como una cámara web o un Reproductor de DVD, esto afecta al rendimiento general del sistema.  
  
## <a name="configuration"></a>Configuración  
  
-   **Memoria RAM, CPU y GPU** vea [optimizar el rendimiento de sistema de MultiPoint Services](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance) en esta guía para obtener recomendaciones de RAM, CPU y GPU.  
-   **Ancho de banda de red** para RDP a través de LAN estaciones conectadas, el ancho de banda de red y la capacidad del cliente (por ejemplo, un cliente ligero, escritorio PC o portátil) es importante, especialmente si el vídeo se está ejecutando en la sesión del usuario. Si usa a clientes de USB a través de Ethernet de cero, el ancho de banda de red también debe tener en cuenta. Los datos para todos los dispositivos de vídeo se envían a través de la misma conexión de Ethernet, por lo que puede considerar la configuración de una red de Gigabit Ethernet independiente al usar estos dispositivos.  
-   **RemoteFX** las estaciones conectado con RDP a través de LAN, es posible que pueda utilizar RemoteFX para mejorar en gran medida la entrega de contenido multimedia de alta definición.  
-   **Resolución de pantalla** si tiene un uso intensivo de vídeo de pantalla completa, puede optar por reducir la resolución del monitor para maximizar el rendimiento.  
-   **Número de clientes USB cero** el número total de clientes USB cero en un centro de raíz única en el servidor afectará directamente al rendimiento de vídeo. Para obtener más información, consulte [diseño para USB cero cliente conectado estaciones](MultiPoint-services-Site-Planning.md#layout-for-usb-zero-client-connected-stations). El número de estaciones de cliente de USB a través de Ethernet cero que se admiten podría ser ligeramente menor que el número de clientes USB cero.  
-   **Ancho de banda USB** al diseñar el sistema, tenga en cuenta el ancho de banda USB.  Esto es especialmente importante para los clientes USB cero, que envían datos de vídeo a través de la conexión USB. Para optimizar el ancho de banda, minimice el número de dispositivos que están conectados a un único puerto USB en el servidor. Esto se aplica a los centros intermedios y las estaciones de margarita. Para obtener más información, consulte [concentradores de estación](MultiPoint-services-Site-Planning.md#station-hubs) y [intermedio hubs](MultiPoint-services-Site-Planning.md#intermediate-hubs).  
  
-   **Tipo USB** utilizando USB 3.0 en lugar de USB 2.0 aumenta el ancho de banda disponible entre el servidor y el centro de notificaciones intermedio si se conecta al centro de más de tres clientes USB cero o si usa dispositivos USB de gran ancho de banda.  
  
-   **Las estaciones** el número total de las estaciones afecta al rendimiento. Si tiene muchos gráficos, procesamiento o necesidades de vídeo, desea limitar el número total de las estaciones. Para obtener más información, consulte [rendimiento optimizar el sistema de MultiPoint Services](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance).