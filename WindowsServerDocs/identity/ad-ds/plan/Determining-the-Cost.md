---
ms.assetid: e3ea1f67-60d4-4566-b24c-37faa95c3b2a
title: Determinar el costo
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b32d1930fcef944fbd719338797f0f29b70fa29d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="determining-the-cost"></a>Determinar el costo

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Debes asignar valores de costo para vínculos a sitios para favorecer conexiones económicas sobre conexiones costosas. Ciertas aplicaciones y servicios, como localizador de controlador de dominio (DCLocator) y el archivo de sistema distribuido espacios de nombres (DFSN), también usan información sobre los costos para localizar los recursos más cercano. Costo del vínculo puede usarse para determinar qué controlador de dominio se establece contacto con los clientes en un sitio si el controlador de dominio para el dominio especificado no existe en ese sitio. El cliente pone en contacto con el controlador de dominio mediante el vínculo del sitio que tiene el menor costo asignado.  
  
Te recomendamos que el valor de costo definirse en todo el sitio. Por lo general se basa costo no solo en el ancho de banda total del vínculo, sino también en la disponibilidad, la latencia y el coste abonado del vínculo.  
  
Para determinar los costos para colocar en los vínculos a sitios, documenta la velocidad de conexión de cada vínculo. Hacer referencia a la hoja de cálculo de "Geográfica ubicaciones y vínculos de comunicación" (DSSTOPO_1.doc) en [recopilar información de red](../../ad-ds/plan/Collecting-Network-Information.md) para obtener información sobre la velocidad de conexión que has identificado.  
  
La siguiente tabla muestra las velocidades para diferentes tipos de redes.  
  
|Tipo de red|Velocidad|  
|----------------|---------|  
|Muy lento|56 kilobits por segundo (Kbps)|  
|Lento|64 kbps|  
|Servicios integrados (RDSI)|64 o 128 Kbps|  
|Retransmisión de marco|Tipo de variable, normalmente entre 56 Kbps y 1,5 megabits por segundo (Mbps)|  
|T1|1,5 Mbps|  
|T3|45 Mbps|  
|10BaseT|10 Mbps|  
|Modo de transferencia asincrónica (CAJERO)|Tipo de variable, normalmente entre 155 Mbps y 622 Mbps|  
|100BaseT|100 Mbps|  
|Gigabit Ethernet|1 gigabit por segundo (GB/s)|  
  
La tabla siguiente para calcular el coste de cada vínculo del sitio en función de velocidad de la red de área extensa (WAN) velocidad del vínculo. Para la velocidad del vínculo WAN que no aparece en la tabla, se puede calcular un factor de costo relativo dividiendo 1.024 por el registro del ancho de banda disponible, medido en Kbps.  
  
|Ancho de banda disponible (Kbps)|Costo|  
|--------------------------------|--------|  
|9.6|1,042|  
|19.2|798|  
|38.4|644|  
|56|586|  
|64|567|  
|128|486|  
|256|425|  
|512|378|  
|1,024|340|  
|2,048|309|  
|4,096|283|  
  
Estos costos no reflejan las diferencias de confiabilidad entre vínculos de red. Establece mayores costos en los vínculos de red propensa a errores de modo que no tienes que dependen de esos vínculos para la replicación. Por mayores costos de sitio del vínculo configuración, puede controlar la conmutación por error de replicación cuando se produce un error en un vínculo de sitio.  
  


