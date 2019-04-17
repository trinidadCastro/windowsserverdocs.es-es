---
title: Contadores de rendimiento relacionados con la red
description: Este tema es parte de la Guía de optimización del rendimiento de red subsistema de Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 7ebaa271-2557-4c24-a679-c3d863e6bf9e
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33551dfd4f76bc13ba69863b782ddae279e0ad16
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="network-related-performance-counters"></a>Contadores de rendimiento relacionados con la red

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema enumeran los contadores que sean relevantes para administrar el rendimiento de red y con las siguientes secciones.  
  
-   [Utilización de recursos](#bkmk_ru)  
  
-   [Posibles problemas de red](#bkmk_np)  
  
-   [Obtener un rendimiento del lado fusión (RSC)](#bkmk_rsc)  
  
##  <a name="bkmk_ru"></a>Utilización de recursos  

Los siguientes contadores de rendimiento son relevantes para el uso de recursos de red.  
  
-   IPv4, IPv6  
  
    -   Datagramas recibidos por segundo  
  
    -   Datagramas enviados por segundo  
  
-   TCPv4, TCPv6  
  
    -   Segmentos recibidos por segundo  
  
    -   Segmentos enviados por segundo  
  
    -   Segmentos s.  
  
-   Redes Interface(*), Adapter(\*) de red  
  
    -   / Seg.  
  
    -   / Seg.  
  
    -   Paquetes recibidos por segundo  
  
    -   Paquetes enviados por segundo  
  
    -   Longitud de la cola de salida  
  
     Este contador es la longitud de la salida paquetes cola \(in packets\). Si es mayor que 2, producen retrasos. Debes buscar el cuello de botella y eliminarlo si es posible. Dado que NDIS pone en cola las solicitudes, este valor de longitud debe ser siempre 0.  
  
-   Información del procesador  
  
    -   % De tiempo de procesador  
  
    -   Interrupciones por segundo  
  
    -   DPC en cola/s.  
  
     Este contador es una velocidad promedio en el que se agregaron DPC a la cola DPC del procesador lógico. Cada procesador lógico tiene su propia cola DPC. Este contador mide la velocidad a la que las DPC se agregan a la cola, no el número de DPC en la cola. Muestra la diferencia entre los valores que se han observado en los dos últimos ejemplos, divididos por la duración del intervalo de muestra.  
  
##  <a name="bkmk_np"></a>Posibles problemas de red  

Los siguientes contadores de rendimiento son relevantes a posibles problemas de red.  
  
-   Redes Interface(*), Adapter(\*) de red  
  
    -   Paquetes recibidos descartados  
  
    -   Paquetes recibidos con errores  
  
    -   Salida de paquetes descartados  
  
    -   Paquetes de salida con errores  
  
-   WFPv4, WFPv6  
  
    -   Paquetes descartados/s

-   UDPv4, UDPv6

    -   Datagramas recibidos con errores  
  
-   TCPv4, TCPv6  
  
    -   Errores de conexión  
  
    -   Restablecimiento de conexiones  
  
-   Directiva de QoS de red  
  
    -   Paquetes descartados  
  
    -   Paquetes descartados por segundo  
  
-   Por actividad de tarjeta de interfaz de red de procesador  
  
    -   Pocos recursos recibir indicaciones por segundo  
  
    -   Pocos recursos recibe paquetes por segundo  
  
-   Microsoft Winsock BSP  
  
    -   Datagramas colocados  
  
    -   Datagramas perdidos por segundo  
  
    -   Conexiones rechazadas  
  
    -   Conexiones/s rechazado  
  
##  <a name="bkmk_rsc"></a>Obtener un rendimiento del lado fusión (RSC)  

Los siguientes contadores de rendimiento son relevantes para el rendimiento de RSC.  
  
-   Adapter(*) de red  
  
    -   Conexiones TCP RSC activo  
  
    -   Tamaño de los paquetes promedio RSC TCP  
  
    -   TCP RSC fusionan paquetes por segundo  
  
    -   Excepciones de TCP RSC por segundo

Para obtener vínculos a todos los temas de esta guía, consulte [optimización del rendimiento de red subsistema](net-sub-performance-top.md).