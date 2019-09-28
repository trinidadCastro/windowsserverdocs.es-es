---
title: Contadores de rendimiento relacionados con la red
description: Este tema forma parte de la guía de optimización del rendimiento del subsistema de red para Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 7ebaa271-2557-4c24-a679-c3d863e6bf9e
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7ebff972d670f3fd0b8d12959d161bce03ac487e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401850"
---
# <a name="network-related-performance-counters"></a>Contadores de rendimiento relacionados con la red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se enumeran los contadores que son relevantes para la administración del rendimiento de la red y se incluyen las siguientes secciones.  
  
-   [Utilización de recursos](#bkmk_ru)  
  
-   [Posibles problemas de red](#bkmk_np)  
  
-   [Rendimiento de fusión del lado de recepción (RSC)](#bkmk_rsc)  
  
##  <a name="bkmk_ru"></a>Utilización de recursos  

Los siguientes contadores de rendimiento son relevantes para el uso de recursos de red.  
  
- IPv4, IPv6  
  
  -   Datagramas recibidos/s  
  
  -   Datagramas enviados/seg.  
  
- TCPv4, TCPv6  
  
  -   Segmentos recibidos/seg.  
  
  -   Segmentos enviados/s  
  
  -   Segmentos retransmitidos/s  
  
- Interfaz de red (*), adaptador de red (\*)  
  
  - Bytes recibidos/seg.  
  
  - Bytes enviados/seg.  
  
  - Paquetes recibidos/seg.  
  
  - Paquetes enviados/s  
  
  - Longitud de la cola de salida  
  
    Este contador es la longitud de la cola de paquetes de salida \(in paquetes @ no__t-1. Si es mayor que 2, se producen retrasos. Debería encontrar el cuello de botella y eliminarlo si es posible. Dado que NDIS pone en cola las solicitudes, esta longitud siempre debe ser 0.  
  
- Información del procesador  
  
  - % Processor Time  
  
  - Interrupciones/seg.  
  
  - DPC en cola/s  
  
    Este contador es una velocidad media a la que se agregaron las DPC a la cola de DPC del procesador lógico. Cada procesador lógico tiene su propia cola de DPC. Este contador mide la velocidad a la que se agregan las DPC a la cola, no el número de DPC en la cola. Muestra la diferencia entre los valores observados en las dos últimas muestras, dividida por la duración del intervalo de ejemplo.  
  
##  <a name="bkmk_np"></a>Posibles problemas de red  

Los siguientes contadores de rendimiento son relevantes para posibles problemas de red.  
  
-   Interfaz de red (*), adaptador de red (\*)  
  
    -   Paquetes recibidos descartados  
  
    -   Errores de paquetes recibidos  
  
    -   Paquetes salientes descartados  
  
    -   Paquetes de errores de salida  
  
-   WFPv4, WFPv6  
  
    -   Paquetes descartados/seg.

-   UDPv4, UDPv6

    -   Datagramas recibidos con errores  
  
-   TCPv4, TCPv6  
  
    -   Errores de conexión  
  
    -   Restablecimiento de conexiones  
  
-   Directiva QoS de red  
  
    -   Paquetes quitados  
  
    -   Paquetes descartados por segundo  
  
-   Actividad de tarjeta de interfaz de red por procesador  
  
    -   Indicaciones de recepción de recursos bajos/s  
  
    -   Paquetes de pocos recursos recibidos por segundo  
  
-   Microsoft Winsock BSP  
  
    -   Datagramas quitados  
  
    -   Datagramas colocados/seg.  
  
    -   Conexiones rechazadas  
  
    -   Conexiones rechazadas/s  
  
##  <a name="bkmk_rsc"></a>Rendimiento de fusión del lado de recepción (RSC)  

Los siguientes contadores de rendimiento son relevantes para el rendimiento de RSC.  
  
-   Adaptador de red(*)  
  
    -   Conexiones RSC activas TCP  
  
    -   Promedio de tamaño de paquete de RSC TCP  
  
    -   Paquetes fusionados por RSC TCP/s  
  
    -   Excepciones RSC TCP/s

Para obtener vínculos a todos los temas de esta guía, consulte [ajuste del rendimiento del subsistema de red](net-sub-performance-top.md).