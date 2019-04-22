---
title: Contadores de rendimiento relacionados con la red
description: En este tema forma parte de la Guía de ajuste de rendimiento del subsistema de red para Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 7ebaa271-2557-4c24-a679-c3d863e6bf9e
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e5e8abbc19482bcd0dd5670065cde59d5be3169a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824356"
---
# <a name="network-related-performance-counters"></a>Contadores de rendimiento relacionados con la red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se enumera los contadores que son relevantes para la administración del rendimiento de red y contiene las siguientes secciones.  
  
-   [Utilización de recursos](#bkmk_ru)  
  
-   [Posibles problemas de red](#bkmk_np)  
  
-   [Obtener un rendimiento Side Coalescing (RSC)](#bkmk_rsc)  
  
##  <a name="bkmk_ru"></a> Utilización de recursos  

Los siguientes contadores de rendimiento son relevantes para el uso de recursos de red.  
  
-   IPv4, IPv6  
  
    -   Datagramas recibidos/seg.  
  
    -   Datagramas enviados/seg.  
  
-   TCPv4, TCPv6  
  
    -   Segmentos recibidos/seg.  
  
    -   Segmentos enviados/seg.  
  
    -   Segmentos retransmitidos/seg.  
  
-   Red Interface(*), adaptador de red (\*)  
  
    -   Bytes recibidos/seg.  
  
    -   Bytes enviados/seg.  
  
    -   Paquetes recibidos/seg.  
  
    -   Paquetes enviados/seg.  
  
    -   Longitud de la cola de salida  
  
     Este contador es la longitud de la cola de paquetes de salida \(en paquetes\). Si es superior a 2, se producen retrasos. Debe encontrar el cuello de botella y eliminarlo si es posible. Dado que NDIS pone en cola las solicitudes, esta longitud debe ser siempre 0.  
  
-   Información del procesador  
  
    -   % Processor Time  
  
    -   Interrupts/sec  
  
    -   Las DPC en cola/seg.  
  
     Este contador es una velocidad media a la que las DPC se agregaron a la cola DPC del procesador lógico. Cada procesador lógico tiene su propia cola DPC. Este contador mide la velocidad a la que las DPC se agregan a la cola, no el número de DPC en la cola. Muestra la diferencia entre los valores que se han observado en las dos últimas muestras divididos entre la duración del intervalo de muestra.  
  
##  <a name="bkmk_np"></a> Posibles problemas de red  

Los siguientes contadores de rendimiento son relevantes a los posibles problemas de red.  
  
-   Red Interface(*), adaptador de red (\*)  
  
    -   Paquetes recibidos descartados  
  
    -   Paquetes recibidos con errores  
  
    -   Paquetes salientes descartados  
  
    -   Paquetes salientes con errores  
  
-   WFPv4, WFPv6  
  
    -   Paquetes descartados/seg.

-   UDPv4, UDPv6

    -   Datagramas recibidos con errores  
  
-   TCPv4, TCPv6  
  
    -   Errores de conexión  
  
    -   Restablecimiento de conexiones  
  
-   Directiva de QoS de red  
  
    -   Paquetes entrantes descartados  
  
    -   Quita de paquetes/seg.  
  
-   Por Processor Network Interface Card Activity  
  
    -   Recursos insuficientes recibir indicaciones/seg.  
  
    -   Recursos insuficientes reciben paquetes/seg.  
  
-   Microsoft Winsock BSP  
  
    -   Datagramas quitados  
  
    -   Datagramas quitados/s  
  
    -   Conexiones rechazadas  
  
    -   Conexiones rechazada/seg  
  
##  <a name="bkmk_rsc"></a> Obtener un rendimiento Side Coalescing (RSC)  

Los siguientes contadores de rendimiento son relevantes para el rendimiento de RSC.  
  
-   Adaptador de red  
  
    -   Conexiones de TCP RSC activo  
  
    -   Tamaño promedio de paquetes de RSC TCP  
  
    -   RSC TCP fusionadas paquetes/seg.  
  
    -   Excepciones de RSC TCP/seg.

Para obtener vínculos a todos los temas de esta guía, consulte [ajuste de rendimiento del subsistema de red](net-sub-performance-top.md).