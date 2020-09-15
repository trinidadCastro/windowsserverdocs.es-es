---
title: Parámetros de plan de energía equilibrado recomendados para tiempos de respuesta rápidos
description: Parámetros de plan de energía equilibrado recomendados para tiempos de respuesta rápidos
ms.topic: conceptual
ms.author: qizha
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 77c33c80598645c4823748663d52fa3aa265e1dd
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077562"
---
# <a name="recommended-balanced-power-plan-parameters-for-workloads-requiring-quick-response-times"></a>Parámetros de plan de energía equilibrado recomendados para cargas de trabajo que requieren tiempos de respuesta rápidos

El plan de energía predeterminado **equilibrado** utiliza el **rendimiento** como la métrica de rendimiento para la optimización. Durante el estado estable, el **rendimiento** no cambia con usos variables hasta que el sistema está totalmente sobrecargado (~ 100% de uso).  Como resultado, el plan de energía **equilibrado** favorece el consumo de energía con una reducción de la frecuencia del procesador y maximización del uso.

Sin embargo, **el tiempo de respuesta** podría aumentar exponencialmente con aumentos de utilización. En la actualidad, el requisito del tiempo de respuesta rápida ha aumentado drásticamente. Aunque Microsoft sugirió a los usuarios a cambiar al plan de energía de **alto rendimiento** cuando necesitan un tiempo de respuesta rápido, algunos usuarios no quieren perder el beneficio de alimentación durante los niveles de carga de luz a medio. Por lo tanto, Microsoft proporciona el siguiente conjunto de cambios de parámetro sugeridos para las cargas de trabajo que requieren un tiempo de respuesta rápido.


| Parámetro | Descripción | Valor predeterminado | Valor propuesto |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Umbral de aumento de rendimiento de procesador | Umbral de uso por encima del cual se aumenta la frecuencia | 90 | 60 |
| Umbral de reducción de rendimiento de procesador | Umbral de uso por debajo del cual se va a reducir la frecuencia | 80 | 40 |
| Tiempo de aumento del rendimiento del procesador | Número de ventanas de comprobación de PPM antes de que la frecuencia sea aumentar | 3 | 1 |
| Directiva de aumento de rendimiento de procesador | Velocidad de aumento de la frecuencia | Single | Ideal |

Para establecer los valores propuestos, los usuarios pueden ejecutar los siguientes comandos en una ventana con administrador:

``` syntax
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTHRESHOLD 60
Powercfg -setacvalueindex scheme_balanced sub_processor PERFDECTHRESHOLD 40
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTIME 1
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCPOL 0
Powercfg -setactive scheme_balanced
```

Este cambio se basa en el análisis de rendimiento y de equilibrio de energía mediante las siguientes cargas de trabajo. En el caso de los usuarios que deseen ajustar aún más la eficiencia energética con determinados requisitos del SLA, consulte [consideraciones sobre el rendimiento del hardware del servidor](../power.md).

>[!Note]
> Para obtener recomendaciones adicionales y obtener información sobre cómo aprovechar los planes de energía para optimizar cargas de trabajo virtualizadas, lea [configuración de Hyper-v](../../role/hyper-v-server/configuration.md) .

## <a name="specpower--java-workload"></a>SPECpower: carga de trabajo de JAVA

[SPECpower \_ ssj2008](http://spec.org/power_ssj2008/), la prueba comparativa de especificación estándar del sector más popular para las características de potencia y rendimiento del servidor, se usa para comprobar el impacto en la energía. Puesto que solo usa el **rendimiento** como métrica de rendimiento, el plan de energía predeterminado **equilibrado** proporciona la mejor eficacia energética.

El cambio de parámetro propuesto consume una potencia ligeramente mayor a la luz (es decir, <= 20%) niveles de carga. Pero con el nivel de carga superior, la diferencia aumenta y comienza a consumir la misma potencia que el plan de energía de **alto rendimiento** después del nivel de carga del 60%. Para usar los parámetros de cambio propuestos, los usuarios deben tener en cuenta el costo de energía en niveles de carga medio y alto durante el planeamiento de la capacidad del bastidor.

## <a name="geekbench-3"></a>GeekBench 3

[GeekBench 3](http://www.geekbench.com/geekbench3/) es una prueba comparativa del procesador multiplataforma que separa las puntuaciones de rendimiento de núcleo único y de varios núcleos. Simula un conjunto de cargas de trabajo, incluidas las cargas de trabajo de enteros (cifrados, compresiones, procesamiento de imágenes, etc.), cargas de trabajo de punto flotante (modelado, fractal, nitidez de imagen, desenfoque de imagen, etc.) y cargas de trabajo de memoria (streaming).

El **tiempo de respuesta** es una medida importante en el cálculo de la puntuación. En nuestro sistema probado, el plan de energía **equilibrado** predeterminado tiene aproximadamente 18% de regresión en las pruebas de un solo núcleo y una regresión del 40% en las pruebas de varios núcleos en comparación con el plan de energía de **alto rendimiento** . El cambio propuesto quita estas regresiones.

## <a name="diskspd"></a>DiskSpd

[Diskspd](https://en.wikipedia.org/wiki/Diskspd) es una herramienta de línea de comandos para la prueba comparativa de almacenamiento desarrollada por Microsoft. Se utiliza ampliamente para generar una variedad de solicitudes en los sistemas de almacenamiento para el análisis de rendimiento del almacenamiento.

Configuramos un [clúster de conmutación por error] y usabamos Diskspd para generar índices aleatorios y secuenciales, así como para leer y escribir en IOs en los sistemas de almacenamiento local y remoto con distintos tamaños de e/s. Nuestras pruebas muestran que el tiempo de respuesta de e/s es muy sensible a la frecuencia del procesador en diferentes planes de energía. El plan de energía **equilibrado** podría duplicar el tiempo de respuesta del plan de energía de **alto rendimiento** en determinadas cargas de trabajo. El cambio propuesto quita la mayoría de las regresiones.

>[!Important]
>A partir de procesadores Intel [Broadwell] que ejecutan Windows Server 2016, la mayoría de las decisiones de administración de energía del procesador se realizan en el procesador en lugar de en el nivel de sistema operativo para lograr una adaptación más rápida a los cambios de carga de trabajo. Los parámetros de PPM heredados que usa el sistema operativo tienen un impacto mínimo en las decisiones de frecuencia reales, excepto en indicar al procesador si se debe favorecer la potencia o el rendimiento, o bien limitar las frecuencias mínima y máxima. Por lo tanto, el cambio propuesto del parámetro PPM solo se dirige a los sistemas Broadwell.

## <a name="see-also"></a>Consulte también
- [Consideraciones de rendimiento del hardware de servidor](../index.md)
- [Server Hardware Power Considerations](../power.md) (Consideraciones de alimentación del hardware de servidor)
- [Power and Performance Tuning](power-performance-tuning.md) (Optimización de potencia y rendimiento)
- [Processor Power Management Tuning](processor-power-management-tuning.md) (Optimización de la administración de energía del procesador)
- [Clúster de conmutación por error](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc725923(v=ws.10))