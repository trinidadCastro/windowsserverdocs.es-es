---
ms.assetid: e3ea1f67-60d4-4566-b24c-37faa95c3b2a
title: Determinar el costo
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: f99c151840fcd32fd94567bba8a0bd9ce5196c69
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87943099"
---
# <a name="determining-the-cost"></a>Determinar el costo

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede asignar valores de costo a vínculos de sitio para favorecer conexiones económicas a través de conexiones caras. Ciertas aplicaciones y servicios, como el localizador de controladores de dominio (Ubicador) y los espacios de nombres Sistema de archivos distribuido (DFSN), también usan información de costos para buscar los recursos más cercanos. El costo del vínculo de sitio se puede usar para determinar a qué controlador de dominio se pone en contacto los clientes en un sitio si el controlador de dominio para el dominio especificado no existe en ese sitio. El cliente se pone en contacto con el controlador de dominio mediante el vínculo a sitios que tiene asignado el costo más bajo.

Se recomienda definir el valor de costo en todo el sitio. El costo normalmente se basa no solo en el ancho de banda total del vínculo, sino también en la disponibilidad, la latencia y el costo monetario del vínculo.

Para determinar los costos que se deben incluir en los vínculos a sitios, documente la velocidad de conexión de cada vínculo a sitios. Consulte la hoja de cálculo "ubicaciones geográficas y vínculos de comunicación" (DSSTOPO_1.doc) en [recopilar información de red](../../ad-ds/plan/Collecting-Network-Information.md) para obtener información sobre la velocidad de conexión que identificó.

En la tabla siguiente se enumeran las velocidades de los distintos tipos de redes.

|Tipo de red|Velocidad|
|----------------|---------|
|Muy lento|56 kilobits por segundo (Kbps)|
|Lenta|64 Kbps|
|Red digital de servicios integrados (RDSI)|64 kbps o 128 kbps|
|Frame Relay|Velocidad variable, normalmente entre 56 kbps y 1,5 megabits por segundo (Mbps)|
|T1|1,5 Mbps|
|T3|45 MBps|
|10Baset|10 Mbps|
|Modo de transferencia asincrónico (ATM)|Velocidad variable, normalmente entre 155 Mbps y 622 Mbps|
|100Baset|100 Mbps|
|Gigabit Ethernet|1 gigabit por segundo (Gbps)|

Use la tabla siguiente para calcular el costo de cada vínculo a sitio basado en la velocidad de vínculo de la velocidad de red de área extensa (WAN). Para la velocidad de vínculo WAN que no aparece en la tabla, puede calcular un factor de costo relativo dividiendo 1.024 por el registro del ancho de banda disponible, medido en kbps.

|Ancho de banda disponible (kbps)|Coste|
|--------------------------------|--------|
|9,6|1.042|
|19,2|798|
|38,4|644|
|56|586|
|64|567|
|128|486|
|256|425|
|512|378|
|1024|340|
|2 048|309|
|4 096|283|

Estos costos no reflejan las diferencias de confiabilidad entre los vínculos de red. Establezca costos mayores en los vínculos de red propensos a errores para que no tenga que depender de esos vínculos para la replicación. Al establecer los costos de vínculo de sitio más altos, puede controlar la conmutación por error de replicación cuando se produce un error en un vínculo de sitio.



