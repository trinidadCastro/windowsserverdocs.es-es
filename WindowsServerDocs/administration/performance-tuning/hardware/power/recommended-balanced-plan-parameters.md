---
title: Recomienda los parámetros del Plan de energía equilibrada para tiempos de respuesta rápidos
description: Recomienda los parámetros del Plan de energía equilibrada para el tiempo de respuesta rápida
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 134e868e1400729f754039fc8120cea0c73945bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878796"
---
# <a name="recommended-balanced-power-plan-parameters-for-workloads-requiring-quick-response-times"></a>Recomienda los parámetros del Plan de energía equilibrada para las cargas de trabajo que requieren tiempos de respuesta rápidos

El valor predeterminado **equilibrado** power plan usa **rendimiento** como la métrica de rendimiento para la optimización. Durante el estado estable, **rendimiento** no cambia con diversos usos hasta que el sistema está completamente sobrecargado (~ 100% de uso).  Como resultado, el **equilibrado** power bastante con frecuencia del procesador de minimizar y maximizar la utilización de favorece el plan de energía.

Sin embargo **tiempo de respuesta** podría aumentar exponencialmente con mayor utilización. En la actualidad, el requisito de tiempo de respuesta rápida ha aumentado considerablemente. Aunque Microsoft recomienda los usuarios para cambiar a la **de alto rendimiento** plan de energía cuando la necesitan una respuesta rápida, algunos usuarios no desean perder la ventaja de power durante la luz para los niveles de carga Media. Por lo tanto, Microsoft proporciona el siguiente conjunto de cambios de parámetros sugeridos para las cargas de trabajo que requieren una respuesta rápida.


| Parámetro | Descripción | Valor predeterminado | Valor propuesto |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Umbral de aumento de rendimiento de procesador | Umbral de utilización anterior que es aumentar la frecuencia | 90 | 60 |
| Umbral de disminución del rendimiento de procesador | Umbral de uso siguiente que es reducir la frecuencia | 80 | 40 |
| Tiempo de aumento de rendimiento de procesador | Número de ventanas PPM comprobación antes de aumentar la frecuencia | 3 | 1 |
| Directiva de aumento de rendimiento de procesador | ¿Con qué rapidez es aumentar la frecuencia | único | Ideal |

Para establecer los valores propuestos, los usuarios pueden ejecutar los siguientes comandos en una ventana con el administrador:

``` syntax
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTHRESHOLD 60
Powercfg -setacvalueindex scheme_balanced sub_processor PERFDECTHRESHOLD 40
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTIME 1
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCPOL 0
Powercfg -setactive scheme_balanced
```

Este cambio se basa en el rendimiento y análisis de contrapartida de energía mediante las siguientes cargas de trabajo. Para los usuarios que deseen ajustar aún más optimización la eficiencia de energía con ciertos requisitos de SLA, consulte [consideraciones de rendimiento del Hardware de servidor](../power.md).

>[!Note]
> Para obtener más recomendaciones e información sobre el aprovechamiento de los planes de energía para optimizar cargas de trabajo virtualizadas, leer [configuración de Hyper-v](../../role/hyper-v-server/configuration.md)

## <a name="specpower--java-workload"></a>SPECpower: carga de trabajo de JAVA

[SPECpower\_ssj2008](http://spec.org/power_ssj2008/), más popular benchmark especificaciones estándar del sector para características de capacidad y rendimiento de servidor, se usa para comprobar el impacto de energía. Puesto que solo usa **rendimiento** como métrica de rendimiento, el valor predeterminado **equilibrado** plan de energía proporciona la mejor eficiencia de energía.

El cambio propuesto parámetro consume energía ligeramente superior a la luz (es decir, < = 20%) niveles de carga. Pero con mayor nivel de carga, que aumenta la diferencia y empieza a consumir igual de eficaz que la **de alto rendimiento** plan de energía después el nivel de carga de un 60%. Para usar los parámetros del cambio propuesto, los usuarios debe tener en cuenta el coste de energía en medio de los niveles de carga alta durante su planeamiento de capacidad del bastidor.

## <a name="geekbench-3"></a>GeekBench 3

[3 GeekBench](http://www.geekbench.com/geekbench3/) es una prueba comparativa de procesador multiplataforma que separa las puntuaciones de rendimiento de un núcleo y varios núcleo. Simula un conjunto de cargas de trabajo incluidas cargas de trabajo de entero (cifrados, compresiones, procesamiento de imágenes, etc.), cargas de trabajo de punto flotante (modelado fractal, imagen enfoque, desenfoque de la imagen, etc.) y cargas de trabajo de memoria (streaming).

**Tiempo de respuesta** es una medida principal en su cálculo de la puntuación. En nuestro sistema probado, el valor predeterminado **equilibrado** plan de energía tiene 18 ~ % regresión en la regresión de un núcleo las pruebas y ~ 40% en las pruebas de varios núcleos en comparación con el **de alto rendimiento** plan de energía. El cambio propuesto quita estas regresiones.

## <a name="diskspd"></a>DiskSpd

[Diskspd](https://en.wikipedia.org/wiki/Diskspd) es una herramienta de línea de comandos para las pruebas comparativas de almacenamiento desarrollado por Microsoft. Ampliamente sirve para generar una variedad de solicitudes en los sistemas de almacenamiento para el análisis de rendimiento de almacenamiento.

Configurar un clúster y se usa Diskspd para generar aleatorio y secuenciales y de lectura y de E/s de escritura a los sistemas de almacenamiento local y remoto con distintos tamaños de E/S. Nuestras pruebas muestran que el tiempo de respuesta de E/S es muy sensible a la frecuencia del procesador en los planes de energía diferente. El **equilibrado** plan de energía pudo duplicar el tiempo de respuesta de desde el **de alto rendimiento** plan de energía en ciertas cargas de trabajo. El cambio propuesto, quita la mayoría de las regresiones.

>[!Important]
>A partir de los procesadores Intel [Broadwell] que ejecuta Windows Server 2016, la mayoría de las decisiones de administración de energía de procesador se realiza en el procesador en lugar de nivel de sistema operativo para lograr una adaptación más rápida a los cambios de carga de trabajo. Los parámetros PPM heredados utilizados por el sistema operativo tienen un impacto mínimo en las decisiones de la frecuencia real, excepto que indica que el procesador de si debe favorecer a alimentación o rendimiento o las frecuencias mínima y máxima de límite. Por lo tanto, el cambio propuesto de parámetro PPM sólo está destinada a los sistemas de pre-Broadwell.

## <a name="see-also"></a>Vea también
- [Consideraciones de rendimiento del Hardware de servidor](../index.md)
- [Consideraciones sobre la alimentación de Hardware de servidor](../power.md)
- [Energía y ajuste del rendimiento](power-performance-tuning.md)
- [Optimización de la administración de energía del procesador](processor-power-management-tuning.md)
- [Clúster de conmutación por error](https://technet.microsoft.com/library/cc725923.aspx)
