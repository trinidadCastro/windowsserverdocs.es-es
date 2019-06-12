---
title: Administración de energía de procesador (PPM) ajuste para el Plan de energía equilibrada de Windows Server
description: Administración de energía de procesador (PPM) ajuste para el Plan de energía equilibrada de Windows Server
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 9b8af89992f01712e16d0ef503c8cbbac915df1d
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811592"
---
# <a name="processor-power-management-ppm-tuning-for-the-windows-server-balanced-power-plan"></a>Administración de energía de procesador (PPM) ajuste para el Plan de energía equilibrada de Windows Server

A partir de Windows Server 2008, Windows Server proporciona tres planes de energía: **Equilibrada**, **de alto rendimiento**, y **Economizador de energía**. El **equilibrado** plan de energía es la opción predeterminada que apunta a brindar a la mejor eficiencia de energía para un conjunto de cargas de trabajo de servidor típica. En este tema se describe las cargas de trabajo que se han utilizado para determinar la configuración predeterminada para el **equilibrado** esquema para las distintas versiones anteriores de Windows.

Si ejecuta un sistema de servidor que tiene características muy diferente de la carga de trabajo o un rendimiento y los requisitos de energía que estas cargas de trabajo, desea considere la posibilidad de ajustar la configuración de energía predeterminados (es decir, crear un plan de energía personalizado). Una fuente de información de optimización útil es el [consideraciones de energía del Hardware de servidor](../power.md). Como alternativa, puede decidir que la **de alto rendimiento** plan de energía es la elección correcta para su entorno, reconociendo que probablemente tendrá una gran cantidad de energía de visitas a cambio de un cierto nivel de mayor capacidad de respuesta.

> [!IMPORTANT]
> Deben aprovechar las directivas de energía que se incluyen con Windows Server a menos que tenga una necesidad concreta para crear uno personalizado y tener una muy buena comprensión que sus resultados variarán según las características de la carga de trabajo.

## <a name="windows-processor-power-tuning-methodology"></a>Metodología de optimización de energía de Windows procesador


### <a name="tested-workloads"></a>Cargas de trabajo probados

Las cargas de trabajo se seleccionan para cubrir un conjunto de mejor esfuerzo de las cargas de trabajo de Windows Server "típicas". Obviamente, este conjunto no pretende ser representativos de toda la riqueza de entornos de servidor reales.

El ajuste en cada directiva de energía es datos controlados por las cargas de trabajo de cinco siguientes:

-   **Carga de trabajo de servidor Web de IIS**

    Un comparativas internas de Microsoft denominada Fundamentos de Web se utilizan para optimizar la eficiencia de energía de las plataformas de servidor Web de IIS. El programa de instalación contiene un servidor web y varios clientes que simulan el tráfico de acceso web. La distribución de dynamic, hot estático (en memoria) y estático en frío (acceso de disco necesario) las páginas web se basa en los estudios estadísticos de los servidores de producción. Para insertar los núcleos de CPU del servidor en pleno uso (un extremo del espectro probado), el programa de instalación necesita recursos de red y disco lo suficientemente rápidos.

-   **Carga de trabajo de base de datos de SQL Server**

    El [TPC-E](http://www.tpc.org/tpce/default.asp) banco de pruebas es una prueba comparativa popular para el análisis de rendimiento de base de datos. Sirve para generar una carga de trabajo OLTP para optimizaciones de optimización de PPM. Esta carga de trabajo tiene E/S de disco significativo y, por tanto, tiene un requisito de alto rendimiento para el tamaño de almacenamiento del sistema y la memoria.

-   **Carga de trabajo de servidor de archivos**

    Llama una prueba comparativa de Microsoft desarrolló [FSCT](http://www.snia.org/sites/default/files2/sdc_archives/2009_presentations/tuesday/BartoszNyczkowski-JianYan_FileServerCapacityTool.pdf) se usa para generar una carga de trabajo del servidor de archivos SMB. Crea un archivo grande que se establezca en el servidor y usa muchos sistemas cliente (reales o virtualizados) para generar el archivo abrir, cerrar, leer y escribir las operaciones. La combinación de la operación se basa en los estudios estadísticos de los servidores de producción. Hace hincapié en CPU, disco y los recursos de red.

-   **SPECpower: carga de trabajo de JAVA**

    [SPECpower\_ssj2008](http://spec.org/power_ssj2008/) es el primer estándar del sector SPEC pruebas comparativas que en conjunto se evalúa como las características de capacidad y rendimiento. Es una carga de trabajo de Java del lado servidor con distintos niveles de carga de CPU. No requiere muchos recursos de disco o de red, pero tiene determinados requisitos de ancho de banda de memoria. Casi toda la actividad de CPU se realiza en modo de usuario; actividad de modo kernel no tiene un impacto mucho de alimentación de los bancos de pruebas y las características de rendimiento excepto para las decisiones de administración de energía.

-   **Carga de trabajo de servidor de aplicaciones**

    El [SAP SD](http://global.sap.com/campaigns/benchmark/index.epx) banco de pruebas se usa para generar una carga de trabajo del servidor de aplicaciones. Se usa un programa de instalación de dos niveles, con la base de datos y el servidor de aplicaciones en el mismo host de servidor. Esta carga de trabajo también utiliza el tiempo de respuesta como una métrica de rendimiento, que es diferente de otras cargas de trabajo probados. Por lo tanto se usa para comprobar el efecto de los parámetros PPM en la capacidad de respuesta. No obstante, no está pensado para ser representativos de todas las cargas de trabajo de producción sensibles a la latencia.

Todas las pruebas comparativas excepto SPECpower se diseñaron originalmente para el análisis de rendimiento y, por tanto, se crearon para ejecutarse en los niveles de carga máxima. Sin embargo, los niveles de medianas y carga ligera son más comunes para los servidores de producción del mundo real y son más interesante para **equilibrado** planear las optimizaciones. Intencionadamente ejecutamos las pruebas comparativas en diferentes niveles de carga de 100% hasta el 10% (en los pasos del 10%) mediante varios métodos de limitación (por ejemplo, al reducir el número de usuarios/clientes activos).

### <a name="hardware-configurations"></a>Configuraciones de hardware

Para cada versión de Windows, se usan los servidores de producción más recientes en el proceso de análisis y la optimización de plan de energía. En algunos casos, las pruebas se realizaron en los sistemas de preproducción cuya programación versión coincidente de la próxima versión de Windows.

Dado que la mayoría de los servidores se vende con sockets de procesador de 1 a 4, y puesto que los servidores de escalado vertical están menos probables que tiene la eficiencia de energía como una preocupación principal, las pruebas de optimización del plan de energía se ejecutan principalmente en sistemas de 2 y 4 sockets. La cantidad de RAM, disco y los recursos de red para cada prueba se eligen para permitir que cada sistema ejecutar hasta su capacidad máxima, teniendo en cuenta las restricciones de costo que normalmente estarían en su lugar para entornos de servidor reales, como mantener la configuraciones razonables.

> [!IMPORTANT]
> Aunque el sistema puede ejecutar en su carga máxima, normalmente optimizamos para los niveles inferiores de carga, ya que los servidores que ejecutan de forma coherente en los niveles de su carga pico es muy recomendable que use el **de alto rendimiento** plan de energía a menos energía la eficiencia es una prioridad alta.

### <a name="metrics"></a>metrics

Todas las pruebas comparativas probadas utilizan rendimiento como la métrica de rendimiento. Tiempo de respuesta se considera como un requisito del SLA para estas cargas de trabajo (excepto para SAP, donde es una métrica principal). Por ejemplo, una ejecución de pruebas comparativas se considera "valid" si la media o el tiempo de respuesta máximo es menor que el valor determinado.

Por lo tanto, el análisis de optimización también de PPM consume rendimiento como su métrica de rendimiento.  En el nivel de carga superior (uso de CPU del 100%), nuestro objetivo es que el rendimiento no debe reducir más de un pequeño porcentaje debido a optimizaciones de administración de energía. Pero la consideración principal es maximizar la eficiencia de energía (tal y como se define a continuación) en los niveles de carga media y baja.

![fórmula de la eficiencia de energía](../../media/serverperf-ppm-formula.jpg)

Ejecución de los núcleos de CPU a frecuencias bajas reduce el consumo de energía. Sin embargo, frecuencias bajas suele reducir el rendimiento y aumentan el tiempo de respuesta. Para el **equilibrado** plan de energía, hay una correlación intencionada de la capacidad de respuesta y de la eficiencia. Las pruebas de carga de trabajo SAP, así como el SLA de tiempo de respuesta de las otras cargas de trabajo, asegúrese de que el aumento del tiempo de respuesta no supere cierto umbral (5% como ejemplo) para estas cargas de trabajo específicas.

> [!NOTE]
> Si la carga de trabajo usa el tiempo de respuesta como la métrica de rendimiento, el sistema debe cambiar a la **de alto rendimiento** plan de energía o cambiar **equilibrado** plan de energía como se sugiere en [ Recomienda los parámetros del Plan de energía equilibrada para el tiempo de respuesta rápida](recommended-balanced-plan-parameters.md).

### <a name="tuning-results"></a>Resultados de optimización

A partir de Windows Server 2008, Microsoft ha trabajado con Intel y AMD para optimizar los parámetros PPM para los procesadores del servidor actualizados para cada versión de Windows. Una gran cantidad de combinaciones de parámetros PPM se probaron en cada una de las cargas de trabajo tratadas anteriormente para encontrar la mejor eficiencia de energía en niveles diferentes de carga. Como software algoritmos se han perfeccionado y arquitecturas de hardware power evolucionadas, cada nuevo Windows Server siempre tenía mejor o igual a la eficiencia de energía que en sus versiones anteriores en la gama de cargas de trabajo probados.

La ilustración siguiente proporciona un ejemplo de la eficiencia de energía en diferentes niveles de carga de TPC-E en un servidor de producción de 4 sockets con Windows Server 2008 R2. Muestra una mejora del 8% en los niveles de carga Media en comparación con Windows Server 2008.

![comparación de la eficiencia de energía](../../media/serverperf-ppm-figure1.jpg)

## <a name="customized-tuning-suggestions"></a>Personalizar las sugerencias de ajuste

Si las características de la carga de trabajo principal difieren significativamente de las cinco cargas de trabajo utilizados para el valor predeterminado **equilibrado** plan de energía PPM optimización, puede experimentar modificando uno o más parámetros PPM para encontrar la mejor opción para su entorno.

Debido a la cantidad y complejidad de los parámetros, esto puede ser una tarea complicada, pero si desea obtener el mejor equilibrio entre eficacia de carga de trabajo y el consumo de energía para su entorno particular, es posible que vale la pena el esfuerzo.

 El conjunto completo de los parámetros ajustables de PPM puede encontrarse en [optimización de administración de energía de procesador](https://msdn.microsoft.com/windows/hardware/gg566941.aspx). Algunos de los parámetros de energía más simple para comenzar podrían ser:

-   **Aumentar el umbral de rendimiento de procesador y la hora de aumentar el rendimiento de procesador** : valores mayores ralentizar la respuesta de rendimiento al aumento de la actividad

-   **Umbral de reducir el rendimiento de procesador** : la respuesta de energía para los períodos de inactividad de quicken de valores grandes

-   **Tiempo de reducir el rendimiento de procesador** : valores mayores reducen gradualmente más el rendimiento durante los períodos de inactividad

-   **Directiva de aumentar el rendimiento del procesador** : la directiva "Única" ralentiza la respuesta de rendimiento sostenido y una mayor actividad; la directiva "Rocket" reacciona rápidamente a una mayor actividad

-   **Directiva de reducir el rendimiento del procesador** : la directiva "Única" reduce gradualmente más rendimiento durante los períodos de inactividad ya; la directiva "Rocket" quita power muy rápidamente al entrar en un periodo de inactividad

>[!Important]
> Antes de comenzar cualquier experimentos, primero debe conocer las cargas de trabajo, que le ayudarán a tomar las decisiones adecuadas de parámetro PPM y reducir el esfuerzo de optimización.

### <a name="understand-high-level-performance-and-power-requirements"></a>Comprender los requisitos de potencia y rendimiento de alto nivel

Si la carga de trabajo es "tiempo real" (p. ej., susceptibles de sufrir problemas técnicos con u otros visible para el usuario final afecta a) o tiene requisitos muy estrictos de la capacidad de respuesta (por ejemplo, un bursátil) y, si el consumo de energía no es un criterio principal para su entorno, probablemente deberá simplemente cambie a la **de alto rendimiento** plan de energía. En caso contrario, debe comprender los requisitos de tiempo de respuesta de las cargas de trabajo y, a continuación, ajuste los parámetros PPM para la mejor eficiencia de energía posibles que todavía cumple esos requisitos.

### <a name="understand-underlying-workload-characteristics"></a>Comprender las características de carga de trabajo subyacente

Debe conocer las cargas de trabajo y los conjuntos de parámetros de experimento para la optimización de diseño. Por ejemplo, si las frecuencias de los núcleos de CPU deben ser aplicar rampas muy rápida (es posible que tenga una carga de trabajo por ráfagas con períodos de inactividad significativo, pero necesita la capacidad de respuesta muy rápido cuando surja una nueva transacción) y, a continuación, el rendimiento del procesador aumenta directiva es posible que debe establecerse en "rocket" (que, como el nombre implica, tira la frecuencia de núcleo de CPU a su valor máximo, en lugar de paso a través de un período de tiempo).

Si la carga de trabajo está muy por ráfagas, se puede reducir el intervalo de comprobación PPM para hacer que la frecuencia de CPU iniciar paso antes una vez que llega una ráfaga. Si la carga de trabajo no tiene simultaneidad excesiva de subprocesos, entonces estacionamiento core puede habilitarse para forzar la carga de trabajo para ejecutar en un menor número de núcleos, lo que podría mejorar potencialmente la frecuencia de aciertos caché del procesador.

Si desea aumentar las frecuencias de CPU en los niveles de uso medio (es decir, los niveles de carga de trabajo no claro), se pueden ajustar los umbrales de aumentar o disminuir de rendimiento de procesador para reaccionar no hasta que se observan determinados niveles de actividad.

### <a name="understand-periodic-behaviors"></a>Comprender los comportamientos periódicos

Puede haber requisitos de rendimiento diferente para el día y por la noche o durante los fines de semana, o puede haber diferentes cargas de trabajo que se ejecutan en momentos diferentes. En este caso, un conjunto de parámetros PPM podría no ser óptimo para todos los períodos de tiempo. Puesto que pueden desarrollarse varios planes de energía personalizados, es posible incluso optimizar para distintos períodos de tiempo y cambiar entre los planes de energía mediante scripts u otro medio de configuración dinámica del sistema.

Nuevamente, esto aumenta la complejidad del proceso de optimización, por lo que es una pregunta se obtenerse el valor de este tipo de optimización, que es probable que deba repetir cuando hay actualizaciones importantes de hardware o los cambios de carga de trabajo.

Es por esta razón Windows proporciona un **equilibrado** plan de energía en primer lugar, porque en muchos casos es probablemente no vale la pena el esfuerzo de optimización de mano las cargas de trabajo específico en un servidor específico.

## <a name="see-also"></a>Vea también
- [Consideraciones de rendimiento del Hardware de servidor](../index.md)
- [Server Hardware Power Considerations](../power.md) (Consideraciones de alimentación del hardware de servidor)
- [Power and Performance Tuning](power-performance-tuning.md) (Optimización de potencia y rendimiento)
- [Processor Power Management Tuning](processor-power-management-tuning.md) (Optimización de la administración de energía del procesador)
- [Recommended Balanced Plan Parameters](recommended-balanced-plan-parameters.md) (Parámetros recomendados del plan equilibrado)
