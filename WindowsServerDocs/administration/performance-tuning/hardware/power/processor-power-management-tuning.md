---
title: Optimización de la administración de energía del procesador (PPM) para el plan de energía del equilibrio de Windows Server
description: Optimización de la administración de energía del procesador (PPM) para el plan de energía del equilibrio de Windows Server
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 53399c1ff1d9fa60df992b922b99c82d119b2f58
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355027"
---
# <a name="processor-power-management-ppm-tuning-for-the-windows-server-balanced-power-plan"></a>Optimización de la administración de energía del procesador (PPM) para el plan de energía del equilibrio de Windows Server

A partir de Windows Server 2008, Windows Server proporciona tres planes de energía: **Equilibrado**, **alto rendimiento**y **ahorro de energía**. El plan de energía **equilibrado** es la opción predeterminada que pretende ofrecer la mejor eficacia energética para un conjunto de cargas de trabajo de servidor típicas. En este tema se describen las cargas de trabajo que se han utilizado para determinar la configuración predeterminada del esquema **equilibrado** en las últimas versiones de Windows.

Si ejecuta un sistema de servidor que tiene características de carga de trabajo drásticamente diferentes o requisitos de rendimiento y energía que estas cargas de trabajo, puede que desee considerar la posibilidad de optimizar la configuración de energía predeterminada (es decir, crear un plan de energía personalizado). Un origen de información útil sobre la optimización es el [rendimiento del servidor](../power.md). Como alternativa, puede decidir que el plan de energía de **alto rendimiento** es la opción adecuada para su entorno, reconociendo que probablemente tendrá un importante impacto energético en Exchange para algún nivel de mayor capacidad de respuesta.

> [!IMPORTANT]
> Debe aprovechar las directivas de energía que se incluyen con Windows Server, a menos que tenga una necesidad específica de crear una personalizada y tenga una buena idea de que los resultados variarán en función de las características de la carga de trabajo.

## <a name="windows-processor-power-tuning-methodology"></a>Metodología de optimización de energía del procesador de Windows


### <a name="tested-workloads"></a>Cargas de trabajo probadas

Las cargas de trabajo se seleccionan para cubrir un mejor conjunto de cargas de trabajo de Windows Server "típicas". Obviamente, este conjunto no pretende ser representativo de toda la amplitud de entornos de servidor del mundo real.

La optimización de cada directiva de energía se basa en las cinco cargas de trabajo siguientes:

-   **Carga de trabajo del servidor Web de IIS**

    Se utiliza una prueba comparativa interna de Microsoft denominada aspectos básicos web para optimizar la eficacia energética de las plataformas que ejecutan el servidor Web de IIS. El programa de instalación contiene un servidor Web y varios clientes que simulan el tráfico de acceso web. La distribución de páginas web dinámicas, estáticas activas (en memoria) y estáticas en frío (acceso al disco necesario) se basa en los estudios estadísticos de los servidores de producción. Para que los núcleos de CPU del servidor se inserten en el uso completo (un extremo del espectro probado), el programa de instalación necesita suficientes recursos de red y de disco.

-   **SQL Server carga de trabajo de base de datos**

    La prueba comparativa [TPC-E](http://www.tpc.org/tpce/default.asp) es un punto de referencia común para el análisis de rendimiento de bases de datos. Se usa para generar una carga de trabajo OLTP para optimizaciones de optimización de PPM. Esta carga de trabajo tiene una e/s de disco significativa y, por tanto, tiene un requisito de alto rendimiento para el sistema de almacenamiento y el tamaño de la memoria.

-   **Carga de trabajo del servidor de archivos**

    Una prueba comparativa desarrollada por Microsoft denominada [FSCT](http://www.snia.org/sites/default/files2/sdc_archives/2009_presentations/tuesday/BartoszNyczkowski-JianYan_FileServerCapacityTool.pdf) se usa para generar una carga de trabajo de servidor de archivos SMB. Crea un conjunto de archivos grandes en el servidor y utiliza muchos sistemas cliente (reales o virtualizados) para generar operaciones de apertura, cierre, lectura y escritura de archivos. La combinación de operaciones se basa en los estudios estadísticos de los servidores de producción. Enfatiza los recursos de CPU, disco y red.

-   **SPECpower: carga de trabajo de JAVA**

    [SPECpower\_ssj2008](http://spec.org/power_ssj2008/) es el primer Benchmark de especificación estándar del sector que evalúa conjuntamente las características de potencia y rendimiento. Es una carga de trabajo de Java del lado servidor con distintos niveles de carga de CPU. No requiere muchos recursos de disco o de red, pero tiene ciertos requisitos para el ancho de banda de memoria. Casi toda la actividad de la CPU se realiza en modo de usuario; la actividad de modo kernel no tiene un gran impacto en las características de potencia y rendimiento de las pruebas comparativas, excepto en las decisiones de administración de energía.

-   **Carga de trabajo del servidor de aplicaciones**

    La prueba comparativa [de SAP-SD](http://global.sap.com/campaigns/benchmark/index.epx) se usa para generar una carga de trabajo del servidor de aplicaciones. Se utiliza una configuración de dos niveles, con la base de datos y el servidor de aplicaciones en el mismo host de servidor. Esta carga de trabajo también emplea el tiempo de respuesta como una métrica de rendimiento, que difiere de otras cargas de trabajo probadas. Por lo tanto, se usa para comprobar el impacto de los parámetros PPM en la capacidad de respuesta. No obstante, no pretende ser representativa de todas las cargas de trabajo de producción sensibles a la latencia.

Todas las pruebas comparativas excepto SPECpower se diseñaron originalmente para el análisis de rendimiento y, por tanto, se crearon para ejecutarse en niveles de carga máxima. Sin embargo, los niveles de carga medio a ligero son más comunes para los servidores de producción del mundo real y son más interesantes para las optimizaciones de planes **equilibrados** . Ejecutamos intencionadamente las pruebas comparativas en diferentes niveles de carga desde el 100% hasta el 10% (en un 10% de los pasos) mediante el uso de varios métodos de limitación (por ejemplo, reduciendo el número de usuarios o clientes activos).

### <a name="hardware-configurations"></a>Configuraciones de hardware

En cada versión de Windows, se usan los servidores de producción más recientes en el proceso de análisis y optimización del plan de energía. En algunos casos, las pruebas se realizaron en sistemas de preproducción cuya programación de versión coincidía con la de la siguiente versión de Windows.

Dado que la mayoría de los servidores se venden con 1 a 4 sockets de procesador, y como los servidores de escalado vertical tienen menos probabilidades de tener eficiencia energética como preocupación principal, las pruebas de optimización del plan de energía se ejecutan principalmente en sistemas de 2 y 4 sockets. La cantidad de recursos de RAM, de disco y de red para cada prueba se eligen para permitir que cada sistema se ejecute hasta su capacidad máxima, al tiempo que se tienen en cuenta las restricciones de costos que normalmente se aplicarían a los entornos de servidor reales, como mantener el configuraciones razonables.

> [!IMPORTANT]
> Aunque el sistema puede ejecutarse en su carga máxima, normalmente se optimiza para niveles de carga inferiores, ya que los servidores que se ejecutan de forma coherente en sus niveles de carga máxima serían muy aconsejables usar el plan de energía de **alto rendimiento** , a menos que la eficiencia energética sea un alto Prior.

### <a name="metrics"></a>metrics

Todas las pruebas comparativas probadas usan el rendimiento como la métrica de rendimiento. El tiempo de respuesta se considera un requisito de acuerdo de nivel de servicio para estas cargas de trabajo (excepto para SAP, donde es una métrica principal). Por ejemplo, una ejecución de pruebas comparativas se considera "válida" si el tiempo de respuesta medio o máximo es menor que cierto valor.

Por lo tanto, el análisis de optimización de PPM también utiliza el rendimiento como su métrica de rendimiento.  En el nivel de carga más alto (100% de uso de CPU), nuestro objetivo es que el rendimiento no debe disminuir más que un porcentaje debido a las optimizaciones de administración de energía. Pero la principal consideración es maximizar la eficacia de la energía (como se define a continuación) en los niveles de carga medio y bajo.

![fórmula de eficiencia de energía](../../media/serverperf-ppm-formula.jpg)

La ejecución de núcleos de CPU a frecuencias bajas reduce el consumo energético. Sin embargo, las frecuencias bajas suelen disminuir el rendimiento y aumentar el tiempo de respuesta. Para el plan de energía **equilibrado** , existe un equilibrio intencionado de la capacidad de respuesta y la eficacia de la energía. Las pruebas de carga de trabajo de SAP, así como los acuerdos de nivel de rendimiento de tiempo de respuesta de las otras cargas de trabajo, asegúrese de que el aumento del tiempo de respuesta no supera el umbral (5% como ejemplo) para estas cargas de trabajo específicas.

> [!NOTE]
> Si la carga de trabajo utiliza el tiempo de respuesta como la métrica de rendimiento, el sistema debe cambiar al plan **de energía de** **alto rendimiento** o cambiar el plan de energía como se sugiere en parámetros de [plan de energía equilibrado recomendado para una respuesta rápida. Hora](recommended-balanced-plan-parameters.md).

### <a name="tuning-results"></a>Resultados de la optimización

A partir de Windows Server 2008, Microsoft trabajó con Intel y AMD para optimizar los parámetros de PPM para los procesadores de servidor más actualizados para cada versión de Windows. Se probó un gran número de combinaciones de parámetros PPM en cada una de las cargas de trabajo descritas anteriormente para encontrar la mejor eficacia energética en diferentes niveles de carga. A medida que se refinan los algoritmos de software y se evolucionen las arquitecturas de energía de hardware, cada Windows Server nuevo siempre tenía una eficiencia de energía mejor o igual que sus versiones anteriores en el intervalo de cargas de trabajo probadas.

En la ilustración siguiente se proporciona un ejemplo de la eficacia energética en diferentes niveles de carga TPC-E en un servidor de producción de 4 sockets que ejecuta Windows Server 2008 R2. Muestra una mejora del 8% en los niveles de carga media en comparación con Windows Server 2008.

![comparación de eficiencia energética](../../media/serverperf-ppm-figure1.jpg)

## <a name="customized-tuning-suggestions"></a>Sugerencias de optimización personalizadas

Si las características de la carga de trabajo principal difieren significativamente de las cinco cargas de trabajo que se usan para el **ajuste predeterminado del** plan de energía en ppm, puede experimentar modificando uno o varios parámetros de ppm para encontrar el mejor ajuste para su entorno.

Debido al número y la complejidad de los parámetros, puede tratarse de una tarea desafiante, pero si busca el mejor equilibrio entre el consumo de energía y la eficacia de la carga de trabajo para su entorno concreto, puede merecer la pena el esfuerzo.

 El conjunto completo de parámetros de PPM ajustables puede encontrarse en la optimización de la [Administración de energía del procesador](https://msdn.microsoft.com/windows/hardware/gg566941.aspx). Algunos de los parámetros de energía más sencillos para empezar pueden ser:

-   Aumento del **rendimiento del procesador y tiempo de aumento del rendimiento del procesador** : los valores mayores ralentizan la respuesta de rendimiento a la actividad mayor

-   **Umbral de reducción de rendimiento de procesador** : valores grandes Quicken la respuesta de energía a períodos de inactividad

-   **Tiempo de reducción del rendimiento del procesador** : los valores más grandes reducen gradualmente el rendimiento durante los períodos de inactividad

-   **Directiva de aumento de rendimiento del procesador** : la Directiva "única" reduce la respuesta de rendimiento a la actividad aumentada y sostenida. la Directiva "Rocket" reacciona rápidamente a la mayor actividad

-   **Directiva de reducción de rendimiento del procesador** : la Directiva "única" reduce gradualmente el rendimiento durante períodos de inactividad más largos. la política "Rocket" reduce la potencia rápidamente al entrar en un período de inactividad.

>[!Important]
> Antes de comenzar cualquier experimento, primero debe comprender las cargas de trabajo, lo que le ayudará a tomar las opciones correctas del parámetro PPM y reducir el esfuerzo de optimización.

### <a name="understand-high-level-performance-and-power-requirements"></a>Comprender los requisitos de rendimiento y energía de alto nivel

Si la carga de trabajo es "tiempo real" (por ejemplo, es susceptible a problemas o a otros impactos visibles para el usuario final) o tiene un requisito de capacidad de respuesta muy estrecho (por ejemplo, un valor bursátil), y si el consumo de energía no es un criterio principal para su entorno, probablemente simplemente cambie al plan de energía de **alto rendimiento** . De lo contrario, debe comprender los requisitos de tiempo de respuesta de las cargas de trabajo y, a continuación, ajustar los parámetros de PPM para lograr la mejor eficacia de energía posible que aún cumpla esos requisitos.

### <a name="understand-underlying-workload-characteristics"></a>Comprender las características de la carga de trabajo subyacente

Debe conocer sus cargas de trabajo y diseñar los conjuntos de parámetros del experimento para la optimización. Por ejemplo, si es necesario aumentar la velocidad de las frecuencias de los núcleos de CPU (quizás tenga una carga de trabajo incremental con períodos de inactividad significativos, pero necesita una capacidad de respuesta muy rápida cuando llega una nueva transacción), la Directiva de aumento del rendimiento del procesador es posible que deba establecerse en "Rocket" (que, como su nombre implica, toma la frecuencia de núcleo de CPU en su valor máximo en lugar de ejecutarla en un período de tiempo).

Si la carga de trabajo es muy alta, el intervalo de comprobación de PPM puede reducirse para que la frecuencia de la CPU empiece a ejecutarse antes de que llegue una ráfaga. Si la carga de trabajo no tiene una simultaneidad alta de subprocesos, se puede habilitar el estacionamiento principal para forzar la ejecución de la carga de trabajo en un número menor de núcleos, lo que también podría mejorar las proporciones de aciertos de caché del procesador.

Si solo desea aumentar las frecuencias de la CPU en niveles de uso medio (es decir, no en niveles de carga de trabajo ligeros), los umbrales de aumento o disminución del rendimiento del procesador pueden ajustarse para no reaccionar hasta que se observen ciertos niveles de actividad.

### <a name="understand-periodic-behaviors"></a>Comprender los comportamientos periódicos

Puede haber diferentes requisitos de rendimiento para el día y el nocturno, o durante los fines de semana, o puede haber diferentes cargas de trabajo que se ejecuten en momentos diferentes. En este caso, un conjunto de parámetros de PPM podría no ser óptimo para todos los períodos de tiempo. Dado que se pueden diseñar varios planes de energía personalizados, es posible incluso ajustar los distintos períodos de tiempo y cambiar entre los planes de energía a través de scripts u otros medios de configuración dinámica del sistema.

Una vez más, esto se suma a la complejidad del proceso de optimización, por lo que es una cuestión de cuánto se ganará en este tipo de optimización, lo que probablemente tendrá que repetirse cuando haya importantes actualizaciones de hardware o cambios de carga de trabajo.

Esta es la razón por la que Windows proporciona un plan de energía **equilibrado** en primer lugar, porque en muchos casos probablemente no merece la pena el esfuerzo de la optimización manual para una carga de trabajo específica en un servidor específico.

## <a name="see-also"></a>Vea también
- [Consideraciones de rendimiento de hardware de servidor](../index.md)
- [Server Hardware Power Considerations](../power.md) (Consideraciones de alimentación del hardware de servidor)
- [Power and Performance Tuning](power-performance-tuning.md) (Optimización de potencia y rendimiento)
- [Processor Power Management Tuning](processor-power-management-tuning.md) (Optimización de la administración de energía del procesador)
- [Recommended Balanced Plan Parameters](recommended-balanced-plan-parameters.md) (Parámetros recomendados del plan equilibrado)
