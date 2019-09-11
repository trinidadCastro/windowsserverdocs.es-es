---
title: Variables que afectan al rendimiento del sistema MultiPoint Services
description: Información de rendimiento de Multipoint Services
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
ms.openlocfilehash: 23bde4a65e3bf41d8968d55bf9641ca6a44b7d96
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871492"
---
# <a name="variables-affecting-multipoint-services-system-performance"></a>Variables que afectan al rendimiento del sistema MultiPoint Services
Hay muchas variables que pueden afectar al rendimiento general del sistema Multipoint Services. Puede que desee considerarlas al diseñar el sistema.  
  
## <a name="usage"></a>Uso  
  
-   **Aplicaciones** de El tipo y el número de aplicaciones que se ejecutan al mismo tiempo\-, especialmente las aplicaciones de gran volumen de gráficos o de memoria, afectarán al rendimiento global del sistema. Para obtener más información, vea [aplicaciones y contenido de Internet](hardware-and-performance-recommendations.md#applications-and-internet-content).  
  
-   **Uso de Internet** Tenga en cuenta si los usuarios van a ver contenido multimedia o páginas web que usan vídeos de movimiento completo. Este tipo de contenido puede sobrecargar el sistema si hay demasiados usuarios viendo simultáneamente.  
  
    > [!NOTE]  
    > La característica de proyección de Multipoint Services, que permite a los profesores proyectar sus pantallas en sus monitores de estudiantes, no está diseñada para proyectar vídeos de movimiento completo. La característica de proyección está diseñada para fines de demostración, como mostrar un procedimiento.  
  
-   **Dispositivos de alta velocidad** Si hay demasiados usuarios utilizando un dispositivo de alta velocidad, como una cámara web o un reproductor de DVD, esto afecta al rendimiento global del sistema.  
  
## <a name="configuration"></a>Configuración  
  
-   **CPU, GPU y RAM** Consulte [optimizar el rendimiento del sistema Multipoint Services](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance) en esta guía para obtener recomendaciones sobre CPU, GPU y RAM.  
-   **Ancho de banda de red** En el caso de las estaciones conectadas RDP a través de LAN, el ancho de banda de red y la capacidad del cliente (por ejemplo, un cliente ligero, un equipo de escritorio o un portátil) es importante, especialmente si el vídeo se está ejecutando en la sesión del usuario. Si usa clientes de USB a través de Ethernet, también debe tener en cuenta el ancho de banda de red. Los datos de vídeo de todos los dispositivos se envían a través de la misma conexión Ethernet, por lo que es posible que desee considerar la posibilidad de configurar una red Gigabit Ethernet independiente al usar estos dispositivos.  
-   **RemoteFX** En el caso de las estaciones conectadas a través de LAN con RDP, es posible que pueda usar RemoteFX para mejorar considerablemente la entrega de contenido multimedia de alta definición.  
-   **Resolución de pantalla** Si tiene un uso intensivo de vídeo a pantalla completa, puede que desee considerar la posibilidad de reducir la resolución del monitor para maximizar el rendimiento.  
-   **Número de clientes USB cero** El número total de clientes USB en un solo concentrador raíz del servidor afectará directamente al rendimiento del vídeo. Para obtener más información, consulte [diseño de estaciones conectadas al cliente USB sin conexión](MultiPoint-services-Site-Planning.md#layout-for-usb-zero-client-connected-stations). El número de estaciones de cliente de USB a través de Ethernet que se admiten puede ser ligeramente menor que el número de clientes USB.  
-   **Ancho de banda USB** Tenga en cuenta el ancho de banda USB al diseñar el sistema.  Esto es especialmente importante para los clientes USB, que envían datos de vídeo a través de la conexión USB. Para optimizar el ancho de banda, minimice el número de dispositivos que están conectados a un único puerto USB en el servidor. Esto se aplica a las estaciones encadenadas en Margarita y a los concentradores intermedios. Para obtener más información, consulte [concentradores de estaciones](MultiPoint-services-Site-Planning.md#station-hubs) y [concentradores intermedios](MultiPoint-services-Site-Planning.md#intermediate-hubs).  
  
-   **Tipo USB** El uso de USB 3,0 en lugar de USB 2,0 aumenta el ancho de banda disponible entre el servidor y el concentrador intermedio si está conectando más de tres clientes USB a la central o si usa dispositivos USB de ancho de banda alto.  
  
-   **Estaciones** de El número total de estaciones afecta al rendimiento. Si tiene grandes necesidades de gráficos, procesamiento o vídeo, es posible que desee limitar el número total de estaciones. Para obtener más información, consulte [optimizar el rendimiento del sistema Multipoint Services](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance).