---
title: Orígenes de datos de System Insights
description: Al escribir una nueva funcionalidad en System Insights, puede especificar orígenes de datos nuevos o existentes para recopilar localmente y analizar. En este tema se describen los orígenes de datos que puede elegir al registrar una nueva funcionalidad.
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 7/31/2018
ms.openlocfilehash: 5dc44d9309c25ca1475e512a11d9868d7fa49e97
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85473672"
---
# <a name="system-insights-data-sources"></a>Orígenes de datos de System Insights

>Se aplica a: Windows Server 2019

System Insights presenta la funcionalidad de recopilación de datos extensible. Al escribir una nueva funcionalidad, puede especificar orígenes de datos nuevos o existentes para recopilar localmente y analizar. En este tema se describen los orígenes de datos que puede elegir al registrar una nueva funcionalidad.

## <a name="data-sources"></a>Orígenes de datos
Al escribir una nueva funcionalidad, debe identificar los orígenes de datos específicos que se van a recopilar para cada capacidad. Los orígenes de datos que especifique se recopilarán y se conservarán directamente en el equipo y podrá elegir entre tres tipos de orígenes de datos:

- **Contadores de rendimiento**:
    - Especifique la ruta de acceso del contador, el nombre y las instancias, y System Insights recopila los datos pertinentes que se indican en estos contadores de rendimiento.

- **Eventos del sistema**:
    - Especifique el nombre del canal y el ID. del evento, y System Insights registrará el número de veces que se ha producido ese evento.

- **Series conocidas**
    - System Insights recopila información básica de su equipo por algunos recursos bien definidos... Estas series se utilizan para las funciones predeterminadas, pero también se pueden usar con cualquier funcionalidad personalizada. Estos recopilan la siguiente información:

        - **Disco**:
            - *Propiedades*: GUID
            - *Datos*: tamaño
        - **Volumen**:
            - *Propiedades*: UniqueID, letraDeUnidad, FileSystemLabel, size
            - *Datos*: tamaño usado
        - **Adaptador de red**:
            - *Propiedades*: InterfaceGuid, InterfaceDescription, Speed
            - *Datos*: bytes recibidos/seg., bytes enviados/seg., bytes totales/seg.
        - **CPU**:
            - *Propiedades*:-
            - *Datos*:% de tiempo de procesador

    - Especifique una serie conocida y System Insights devolverá los datos recopilados por esa serie.


## <a name="retention-timelines-and-collection-intervals"></a>Escalas de tiempo de retención e intervalos de recopilación
Los orígenes de datos anteriores tienen diferentes escalas de tiempo de retención e intervalos de recopilación. En la tabla siguiente se revela el tiempo y la frecuencia de recopilación de cada origen de datos:

| Origen de datos | Escala de tiempo de retención | Intervalo de recopilación |
| --------------- | --------------- | ----------- |
| Contadores de rendimiento | 3 meses | 15 minutos |
| Eventos del sistema | 3 meses | 15 minutos |
| Series conocidas | 1 año | 1 hora |


### <a name="aggregation-types"></a>Tipos de agregación
Dado que cada serie registra solo un punto de datos para cada intervalo de recopilación, cada serie tiene un tipo de agregación asociadas. En la tabla siguiente se describe el origen de datos y el tipo de agregación correspondiente:

>[!NOTE]
>Para los contadores de rendimiento, puede elegir entre varios tipos de agregación diferentes.

| Origen de datos | Tipos de agregación |
| --------------- | --------------- |
| Contadores de rendimiento | SUM, Average, Max, min |
| Eventos del sistema | Count |
| Serie de discos conocidos | Last (último valor en el intervalo de recopilación) |
| Serie de volúmenes conocidos | Last (último valor en el intervalo de recopilación) |
| Serie de CPU conocidas | Average |
| Serie de redes conocidas | Average |

## <a name="data-footprint"></a>Superficie de datos

System Insights recopila todos los datos localmente en la unidad C (C:). En general, la superficie de los datos de System Insights es modesta. Depende directamente del tipo y el número de orígenes de datos que cada capacidad especifica, y en la tabla siguiente se detalla el uso de almacenamiento para cada tipo de datos:

| Origen de datos | Superficie máxima |
| --------------- | --------------- |
| Contadores de rendimiento | 240 KB |
| Eventos del sistema | 200 KB |
| Serie de discos conocidos | 200 KB por disco |
| Serie de volúmenes conocidos | 300 KB por volumen |
| Serie de CPU conocidas | 100 KB |
| Serie de redes conocidas | 300 KB por adaptador de red |

>[!NOTE]
>**En el caso de las capacidades de previsión predeterminadas, la superficie máxima debe ser inferior a 10 MB para la mayoría de los equipos independientes.**

## <a name="additional-references"></a>Referencias adicionales
Para obtener más información acerca de System Insights, utilice los siguientes recursos:

- [Introducción a información del sistema](overview.md)
- [Descripción de funcionalidades](understanding-capabilities.md)
- [Administración de funcionalidades](managing-capabilities.md)
- [Funcionalidades de adición y desarrollo](adding-and-developing-capabilities.md)
- [Preguntas más frecuentes sobre System Insights](faq.md)
