---
ms.assetid: e3ea1f67-60d4-4566-b24c-37faa95c3b2a
title: Determinar el costo
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0b34f1672311768d644c467fda10dc2fc643282d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847086"
---
# <a name="determining-the-cost"></a>Determinar el costo

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Asignar valores de costo a vínculos a sitios que dé prioridad a las conexiones de bajo costosos a través de conexiones costosas. Determinadas aplicaciones y servicios, como el ubicador del controlador de dominio (del ubicador de DC) y el archivo de sistema distribuido espacios de nombres (DFSN), también usan información de costo para localizar los recursos más cercanos. Costo del vínculo de sitio puede utilizarse para determinar qué controlador de dominio se puede establecer contacto con los clientes en un sitio si el controlador de dominio para el dominio especificado no existe en ese sitio. El cliente contacta con el controlador de dominio mediante el vínculo de sitio que tiene el menor costo asignado a él.  
  
Se recomienda que el valor del costo definirse en todo el sitio. Costo se basa normalmente no solo en el ancho de banda total del vínculo, sino también en la disponibilidad, latencia y costo económico del vínculo.  
  
Para determinar los costos para colocar en los vínculos a sitios, documente la velocidad de conexión para cada vínculo de sitio. Hacer referencia a la hoja de cálculo "Geográfica ubicaciones y vínculos de comunicación" (DSSTOPO_1.doc) [recopilar información de red](../../ad-ds/plan/Collecting-Network-Information.md) para obtener información acerca de la velocidad de conexión que ha identificado.  
  
En la siguiente tabla muestra las velocidades para distintos tipos de redes.  
  
|Tipo de red|Velocidad|  
|----------------|---------|  
|Muy lento|56 kilobits por segundo (Kbps)|  
|Nivel lento|64 Kbps|  
|Red Digital de servicios integrados (RDSI)|64 o 128 Kbps|  
|Retransmisión de marco|Tipo de variable, normalmente entre 56 Kbps y 1,5 megabits por segundo (Mbps)|  
|T1|1,5 Mbps|  
|T3|45 Mbps|  
|10BaseT|10 Mbps|  
|Modo de transferencia asincrónico (ATM)|Velocidad variable, normalmente entre 155 y 622 Mbps|  
|100BaseT|100 Mbps|  
|Gigabit Ethernet|1 gigabit por segundo (Gbps)|  
  
Use la tabla siguiente para calcular el costo de cada vínculo de sitio en función de la velocidad de red de área extensa (WAN) velocidad de vínculo. Para la velocidad del vínculo WAN que no aparece en la tabla, puede calcular un factor de costo relativo al dividir 1.024 por el registro del ancho de banda disponible, medido en Kbps.  
  
|Ancho de banda disponible (Kbps)|Coste|  
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
  
Estos costos no reflejan las diferencias de confiabilidad entre vínculos de red. Establecer costos más elevados en los vínculos de red propensos a errores para que no tengan que depender de esos vínculos de replicación. Por costos de vínculo de sitio superior configuración, puede controlar la conmutación por error de replicación cuando se produce un error en un vínculo a sitios.  
  


