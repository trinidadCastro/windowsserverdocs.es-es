---
title: Orígenes de datos de información del sistema
description: Cuando se escribe una nueva funcionalidad de información del sistema, puede especificar los orígenes de datos nueva o existente para recopilar localmente y analizar. En este tema se describe los orígenes de datos que puede elegir al registrar una nueva funcionalidad.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: system-insights
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 7/31/2018
ms.openlocfilehash: 9b46db90787d24a173ffa472ec1ecb8eaffe054b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845416"
---
# <a name="system-insights-data-sources"></a>Orígenes de datos de información del sistema

>Se aplica a: Windows Server 2019

Información del sistema presenta la funcionalidad de recopilación de datos extensible. Al escribir una nueva funcionalidad, puede especificar los orígenes de datos nueva o existente para recopilar localmente y analizar. En este tema se describe los orígenes de datos que puede elegir al registrar una nueva funcionalidad.

## <a name="data-sources"></a>Orígenes de datos
Al escribir una nueva funcionalidad, debe identificar los orígenes de datos específicos para recopilar para cada capacidad. Los orígenes de datos que especifique se recopilan y se guardan directamente en el equipo, y puede elegir entre tres tipos de orígenes de datos:

- **Contadores de rendimiento**: 
    - Especifique la ruta de acceso de contador, nombre y las instancias y sistema Insights recopila los datos pertinentes notificados por estos contadores de rendimiento. 

- **Eventos del sistema**:
    - Especifique el nombre y el evento Id. del canal y sistema Insights registrará cuántas veces se ha producido ese evento.

- **Serie Well-Known**
    - Información del sistema recopila información básica en el equipo para un algunos recursos bien definidos. Estas series se usan para las funcionalidades de forma predeterminada, pero también puede usarse para cualquier capacidad personalizada. Estos recopilan la información siguiente:

        - **Disco**: 
            - *Propiedades*: GUID
            - *Datos*: Tamaño
        - **Volumen**:
            - *Propiedades*: Identificador único, el tamaño de letra de unidad, FileSystemLabel,
            - *Datos*: Tamaño usado
        - **Adaptador de red**:
            - *Propiedades*: InterfaceGuid, InterfaceDescription, velocidad
            - *Datos*: Bytes recibidos/seg., Bytes enviados/seg., Bytes totales/seg.
        - **CPU**: 
            - *Propiedades*:-
            - *Datos*: % tiempo de procesador

    - Especificar una serie muy conocida y sistema Insights devolverá los datos recopilados por esa serie. 


## <a name="retention-timelines-and-collection-intervals"></a>Intervalos de retención de las escalas de tiempo y colección
Los orígenes de datos anteriores tienen intervalos de retención diferentes escalas de tiempo y la colección. En la tabla siguiente, se muestra cómo cuánto tiempo y a menudo se recopila cada origen de datos:

| Origen de datos | Escala de tiempo de retención | Intervalo de recopilación |
| --------------- | --------------- | ----------- |
| Contadores de rendimiento | 3 meses | 15 minutos |
| Eventos del sistema | 3 meses | 15 minutos |
| Serie Well-Known | 1 año | 1 hora |


### <a name="aggregation-types"></a>Tipos de agregación
Dado que cada serie se registre solo un punto de datos para cada intervalo de recopilación, cada serie tiene una cuenta de tipo de agregación. La siguiente tabla describe el origen de datos y el tipo de agregación correspondiente:

>[!NOTE]
>Para los contadores de rendimiento, puede elegir desde unos pocos tipos de agregación diferentes.

| Origen de datos | Tipos de agregación |
| --------------- | --------------- |
| Contadores de rendimiento | SUM, average, max, min |
| Eventos del sistema | Número |
| Serie de disco muy conocida | Último (valor más reciente en el intervalo de recopilación) |
| Serie conocido de volumen | Último (valor más reciente en el intervalo de recopilación) |
| Serie de CPU conocido | Media |
| Serie de red conocido | Media |

## <a name="data-footprint"></a>Superficie de los datos

Sistema Insights recopila todos los datos localmente en la unidad C (C:). En general, la superficie de los datos del sistema Insights es pequeña. Depende directamente en el tipo y número de orígenes de datos que especifica cada capacidad y la tabla siguiente detallan el uso de almacenamiento para cada tipo de datos:

| Origen de datos | Espacio máximo |
| --------------- | --------------- |
| Contadores de rendimiento | 240 KB |
| Eventos del sistema | 200 KB |
| Serie de disco muy conocida | 200 KB por disco |
| Serie conocido de volumen | 300 KB por volumen |
| Serie de CPU conocido | 100 KB |
| Serie de red conocido | 300 KB por cada adaptador de red |

>[!NOTE]
>**Para la funciones de previsión de manera predeterminada, el consumo máximo debe ser menor que 10 MB para la mayoría de los equipos de forma independiente.** 

## <a name="see-also"></a>Vea también
Para obtener más información acerca de la información del sistema, use los siguientes recursos:

- [Información general de información del sistema](overview.md)
- [Capacidades de descripción](understanding-capabilities.md)
- [Capacidades de administración](managing-capabilities.md)
- [Agregar y desarrollo de capacidades](adding-and-developing-capabilities.md)
- [Preguntas más frecuentes del sistema Insights](faq.md)
