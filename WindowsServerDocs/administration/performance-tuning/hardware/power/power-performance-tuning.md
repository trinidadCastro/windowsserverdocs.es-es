---
title: Ajuste de potencia y rendimiento
description: Optimización de la administración de energía del procesador (PPM) para el plan de energía del equilibrio de Windows Server
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 2f1d5e2f3f17c40f262b8cea98c04e3347790ba8
ms.sourcegitcommit: 3f9bcd188dda12dc5803defb47b2c3a907504255
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2020
ms.locfileid: "77001830"
---
# <a name="power-and-performance-tuning"></a>Ajuste de potencia y rendimiento

La eficiencia energética es cada vez más importante en entornos empresariales y centros de datos, y agrega otro conjunto de inconvenientes a la combinación de opciones de configuración.

Windows Server 2016 está optimizado para una eficacia energética excelente con un impacto mínimo en el rendimiento en una amplia gama de cargas de trabajo de clientes. La [optimización de la administración de energía del procesador (ppm) para el plan de energía de Windows Server balancedo](processor-power-management-tuning.md) describe las cargas de trabajo que se usan para optimizar los parámetros predeterminados en windows Server 2016 y proporciona sugerencias para las optimizaciones personalizadas.

En esta sección se amplían los inconvenientes de la eficiencia energética para ayudarle a tomar decisiones informadas si necesita ajustar la configuración de energía predeterminada en el servidor. Sin embargo, la mayoría del hardware y las cargas de trabajo del servidor no deben requerir la optimización de energía del administrador al ejecutar Windows Server 2016.

## <a name="calculating-server-energy-efficiency"></a>Calcular la eficacia energética del servidor

Al optimizar el servidor para ahorrar energía, también debe tener en cuenta el rendimiento. La optimización afecta al rendimiento y a la potencia, a veces en cantidades desproporcionadas. Para cada posible ajuste, tenga en cuenta la asignación de energía y los objetivos de rendimiento para determinar si el equilibrio es aceptable.

Puede calcular la proporción de eficiencia energética de su servidor para una métrica útil que incorpora información de rendimiento y energía. La eficiencia energética es la proporción de trabajo que se realiza a la potencia media que se necesita durante un período de tiempo especificado.

![fórmula de eficiencia energética](../../media/perftune-guide-power-formula.png)

Puede usar esta métrica para establecer objetivos prácticos que respeten el equilibrio entre potencia y rendimiento. Por el contrario, un objetivo del 10 por ciento de ahorro energético en el centro de datos no puede capturar los efectos correspondientes en el rendimiento y viceversa.

Del mismo modo, si optimiza el servidor para aumentar el rendimiento en un 5 por ciento y esto da como resultado un consumo de energía de un 10% mayor, el resultado total podría o no ser aceptable para sus objetivos empresariales. La métrica de eficiencia energética permite la toma de decisiones más informadas que las métricas de potencia o rendimiento solas.

## <a name="measuring-system-energy-consumption"></a>Medición del consumo de energía del sistema

Debe establecer una medición de energía de línea base antes de optimizar el servidor para mejorar la eficacia energética.

Si el servidor tiene la compatibilidad necesaria, puede usar las características de disponibilidad y presupuestabilidad de energía de Windows Server 2016 para ver el consumo de energía de nivel de sistema mediante el monitor de rendimiento.

Una manera de determinar si el servidor es compatible con la medición y el presupuesto es revisar el [Catálogo de Windows Server](https://www.windowsservercatalog.com). Si el modelo de servidor cumple las nuevas calificaciones de administración de energía mejoradas en el programa de certificación de hardware de Windows, se garantiza que admite la funcionalidad de medición y presupuestabilidad.

Otra manera de comprobar la compatibilidad con la medición es buscar manualmente los contadores en el monitor de rendimiento. Abra el monitor de rendimiento, seleccione **Agregar contadores**y, a continuación, busque el grupo de contadores de **medidor de energía** .

Si las instancias con nombre de los medidores de energía aparecen en el cuadro con la etiqueta **instancias del objeto seleccionado**, la plataforma admite la medición. El contador de **energía** que muestra la potencia en vatios aparece en el grupo de contadores seleccionado. No se ha especificado la derivación exacta del valor de los datos de energía. Por ejemplo, podría ser un dibujo de energía instantáneo o un corte de energía promedio en un intervalo de tiempo.

Si la plataforma de servidor no admite la medición, puede usar un dispositivo de medición física conectado a la entrada de la fuente de alimentación para medir el consumo energético o de energía del sistema.

Para establecer una línea base, debe medir la potencia media necesaria en varios puntos de carga del sistema, desde inactivo hasta el 100 por ciento (rendimiento máximo) para generar una línea de carga. En la ilustración siguiente se muestran líneas de carga para tres configuraciones de ejemplo:

![líneas de carga de ejemplo](../../media/perftune-guide-sample-loadlines.png)

Puede usar líneas de carga para evaluar y comparar el rendimiento y el consumo energético de las configuraciones en todos los puntos de carga. En este ejemplo concreto, es fácil ver cuál es la mejor configuración. Sin embargo, puede haber escenarios fácilmente en los que una configuración funcione mejor para cargas de trabajo pesadas y una que funcione mejor para cargas de trabajo ligeras.

Necesita comprender exhaustivamente los requisitos de carga de trabajo para elegir una configuración óptima. No asuma que cuando encuentre una buena configuración, siempre seguirá siendo óptima. Debe medir el uso del sistema y el consumo de energía de forma regular y después de los cambios en las cargas de trabajo, los niveles de carga de trabajo o el hardware del servidor.

## <a name="diagnosing-energy-efficiency-issues"></a>Diagnóstico de problemas de eficiencia energética

**PowerCfg. exe** admite una opción de línea de comandos que puede usar para analizar la eficiencia energética inactiva de su servidor. Al ejecutar PowerCfg. exe con la opción **/Energy** , la herramienta realiza una prueba de 60 segundos para detectar posibles problemas de eficiencia energética. La herramienta genera un informe HTML sencillo en el directorio actual.

> [!Important]
> Para garantizar un análisis preciso, asegúrese de que todas las aplicaciones locales estén cerradas antes de ejecutar **PowerCfg. exe**. 

Las tasas de TICs de temporizador reducidas, los controladores que carecen de compatibilidad con la administración de energía y un uso excesivo de la CPU son algunos de los problemas de comportamiento detectados por el comando **powercfg/Energy** . Esta herramienta proporciona una manera sencilla de identificar y corregir problemas de administración de energía, lo que puede dar lugar a importantes ahorros en los costos de un centro de recursos de gran tamaño.

Para obtener más información acerca de PowerCfg. exe, consulte [uso de powercfg para evaluar la eficacia energética del sistema](https://msdn.microsoft.com/windows/hardware/gg463250.aspx).

## <a name="using-power-plans-in-windows-server"></a>Uso de planes de energía en Windows Server

Windows Server 2016 tiene tres planes de energía integrados diseñados para satisfacer diferentes conjuntos de necesidades empresariales. Estos planes proporcionan una manera sencilla de personalizar un servidor para satisfacer los objetivos de potencia o rendimiento. En la tabla siguiente se describen los planes, se enumeran los escenarios comunes en los que se usa cada plan y se proporcionan algunos detalles de implementación para cada plan.

| **Planificar** | **Descripción** | **Escenarios aplicables comunes** | **Aspectos destacados de la implementación** |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Equilibrado (recomendado) | Configuración predeterminada. Se dirige a una buena eficacia energética con un impacto mínimo en el rendimiento. | Informática general | Coincide con la capacidad de demanda. Las características de ahorro de energía equilibran la eficacia y el rendimiento. |
| Alto rendimiento | Aumenta el rendimiento a costa de un consumo de energía elevado. Se aplican las limitaciones de energía y térmica, los gastos operativos y las consideraciones de confiabilidad. | Aplicaciones de baja latencia y código de la aplicación que es sensible a los cambios de rendimiento del procesador | Los procesadores siempre están bloqueados en el estado de rendimiento más alto (incluidas las frecuencias "Turbo"). Se desactivan todos los núcleos. La salida térmica puede ser significativa. |
| Economizador de energía | Limita el rendimiento para ahorrar energía y reducir el costo operativo. No se recomienda sin realizar pruebas exhaustivas para asegurarse de que el rendimiento es adecuado. | Implementaciones con presupuestos de energía limitados y restricciones térmicas | La frecuencia del procesador es un porcentaje del máximo (si se admite) y habilita otras características de ahorro de energía. |


Estos planes de energía existen en Windows para los sistemas de corriente alterna (AC) y corriente directa (DC), pero se supone que los servidores siempre usan una fuente de alimentación de CA.

Para obtener más información sobre los planes de energía y las configuraciones de directivas de energía, vea [configuración e implementación de directivas de energía en Windows](https://msdn.microsoft.com/windows/hardware/gg463243.aspx).

> [!Note]
> Algunos fabricantes de servidores tienen sus propias opciones de administración de energía disponibles a través de la configuración del BIOS. Si el sistema operativo no tiene control sobre la administración de energía, el cambio de los planes de energía en Windows no afectará a la eficacia y el rendimiento del sistema.

## <a name="tuning-processor-power-management-parameters"></a>Ajustar los parámetros de administración de energía del procesador

Cada plan de energía representa una combinación de numerosos parámetros de administración de energía subyacentes. Los planes integrados son tres colecciones de configuraciones recomendadas que abarcan una amplia variedad de cargas de trabajo y escenarios. Sin embargo, reconocemos que estos planes no responderán a las necesidades de cada cliente.

En las secciones siguientes se describen las formas de ajustar algunos parámetros de administración de energía específicos del procesador para satisfacer los objetivos no abordados por los tres planes integrados. Si necesita comprender una matriz más amplia de parámetros de energía, consulte [configuración e implementación de directivas de energía en Windows](https://msdn.microsoft.com/windows/hardware/gg463243.aspx).

## <a name="processor-performance-boost-mode"></a>Modo de mejora del rendimiento del procesador

Las tecnologías Intel Turbo Boost y AMD Turbo CORE son características que permiten a los procesadores obtener un rendimiento adicional cuando resultan más útiles (es decir, en cargas de sistema altas). Sin embargo, esta característica aumenta el consumo de energía de núcleos de CPU, por lo que Windows Server 2016 configura Turbo Technologies en función de la Directiva de energía que se usa y de la implementación del procesador específico.

Turbo está habilitado para los planes de energía de alto rendimiento en todos los procesadores Intel y AMD, y está deshabilitado para los planes de energía del ahorro de energía. En el caso de los planes de energía equilibrados en sistemas que se basan en la administración de frecuencias basada en el estado de P tradicional, turbo está habilitada de forma predeterminada solo si la plataforma es compatible con el registro EPB.

> [!Note]
> El registro EPB solo se admite en procesadores Intel Westmere y posteriores.

En el caso de los procesadores Intel Nehalem y AMD, turbo está deshabilitada de forma predeterminada en las plataformas basadas en el estado P. Sin embargo, si un sistema admite el control de rendimiento de procesador de colaboración (CPPC), que es un nuevo modo alternativo de comunicación de rendimiento entre el sistema operativo y el hardware (definido en ACPI 5,0), se puede utilizar Turbo si el funcionamiento de Windows el sistema solicita dinámicamente el hardware para ofrecer los mayores niveles de rendimiento posibles.

Para habilitar o deshabilitar la característica Turbo Boost, el administrador debe configurar el parámetro de modo mejora del rendimiento del procesador o la configuración predeterminada del parámetro para el plan de energía elegido. El modo de mejora del rendimiento del procesador tiene cinco valores permitidos, como se muestra en la tabla 5.

En el caso del control basado en el estado P, las opciones están deshabilitadas, habilitadas (Turbo está disponible para el hardware cuando se solicita un rendimiento nominal) y eficaz (Turbo solo está disponible si se implementa el registro EPB).

En el caso del control basado en CPPC, las opciones están deshabilitadas y habilitadas de forma eficaz (Windows especifica la cantidad exacta de Turbo que se debe proporcionar) y agresiva (Windows solicita el "rendimiento máximo" para habilitar Turbo).

En Windows Server 2016, el valor predeterminado para el modo Boost es 3.

| **Nombre** | **Comportamiento basado en el estado P** | **Comportamiento de CPPC** |
|--------------------------|------------------------|-------------------|
| 0 (deshabilitado) | Deshabilitada | Deshabilitada |
| 1 (habilitado) | Habilitado | Eficaz habilitado |
| 2 (agresivo) | Habilitado | Agresiva |
| 3 (eficaz habilitado) | Eficaz | Eficaz habilitado |
| 4 (agresiva eficaz) | Eficaz | Agresiva |

 
Los siguientes comandos habilitan el modo de mejora del rendimiento del procesador en el plan de energía actual (especifique la Directiva mediante un alias de GUID):

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_current
```

> [!Important]
> Debe ejecutar el comando **powercfg-SETACTIVE** para habilitar la nueva configuración. No es necesario reiniciar el servidor.

Para establecer este valor para planes de energía distintos del plan seleccionado actualmente, puede usar alias como esquema\_máximo (economizador de energía), esquema\_mínimo (alto rendimiento) y esquema\_EQUILIBRAda (equilibrada) en lugar del esquema\_actual. Reemplace "Scheme Current" en los comandos powercfg-SETACTIVE mostrados anteriormente con el alias deseado para habilitar ese plan de energía.

Por ejemplo, para ajustar el modo de mejora en el plan economizador de energía y hacer que el economizador de energía sea el plan actual, ejecute los siguientes comandos:

``` syntax
Powercfg -setacvalueindex scheme_max sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_max
```

## <a name="minimum-and-maximum-processor-performance-state"></a>Estado de rendimiento mínimo y máximo del procesador

Los procesadores cambian entre los Estados de rendimiento (P-States) muy rápidamente para que coincidan con la demanda, proporcionando rendimiento cuando sea necesario y ahorrando energía cuando sea posible. Si el servidor tiene requisitos específicos de alto rendimiento o de consumo mínimo de energía, considere la posibilidad de configurar el parámetro de **Estado de rendimiento de procesador mínimo** o el parámetro de **Estado de rendimiento de procesador máximo** .

Los valores para los parámetros de estado de **rendimiento de procesador mínimo** y **rendimiento de procesador máximo** se expresan como un porcentaje de la frecuencia máxima del procesador, con un valor comprendido entre 0 y 100.

Si el servidor requiere una latencia muy baja, una frecuencia de CPU invariable (por ejemplo, para pruebas repetibles) o los niveles de rendimiento más altos, es posible que no desee que los procesadores cambien a Estados de menor rendimiento. Para este tipo de servidor, puede limitar el estado de rendimiento mínimo del procesador al 100 por ciento mediante los siguientes comandos:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMIN 100
Powercfg -setactive scheme_current
```

Si el servidor requiere un consumo de energía inferior, es posible que desee limitar el estado de rendimiento del procesador en un porcentaje de máximo. Por ejemplo, puede restringir el procesador al 75 por ciento de su frecuencia máxima mediante el uso de los siguientes comandos:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMAX 75
Powercfg -setactive scheme_current
```

> [!Note]
> Limitar el rendimiento del procesador en un porcentaje del máximo requiere compatibilidad con el procesador. Consulte la documentación del procesador para determinar si existe dicha compatibilidad o visualice el contador del monitor **de rendimiento% de la frecuencia máxima** en el grupo de **procesadores** para ver si se aplicó alguna Cap de frecuencia.

## <a name="processor-performance-increase-and-decrease-of-thresholds-and-policies"></a>Aumento y disminución del rendimiento del procesador de umbrales y directivas

La velocidad a la que aumenta o disminuye un estado de rendimiento del procesador se controla mediante varios parámetros. Los cuatro parámetros siguientes tienen el impacto más visible:

-   **Umbral de aumento de rendimiento de procesador** define el valor de uso por encima del cual aumentará el estado de rendimiento de un procesador. Los valores mayores ralentizan la velocidad del aumento del estado de rendimiento en respuesta a las actividades mayores.

-   **Umbral de reducción de rendimiento de procesador** define el valor de uso por debajo del cual disminuirá el estado de rendimiento de un procesador. Los valores mayores aumentan la tasa de disminución del estado de rendimiento durante los períodos de inactividad.

-   **Disminución del rendimiento del procesador y de la Directiva de aumento de rendimiento del procesador** La Directiva determina qué estado de rendimiento se debe establecer cuando se produce un cambio. La Directiva "única" significa que elige el estado siguiente. "Rocket" significa el estado de rendimiento máximo o mínimo de energía. "Ideal" intenta encontrar un equilibrio entre potencia y rendimiento.

Por ejemplo, si el servidor requiere una latencia muy baja y, al mismo tiempo, quiere beneficiarse de una baja energía durante los períodos de inactividad, puede aumentar el aumento del estado de rendimiento para cualquier aumento en la carga y ralentizar la disminución cuando la carga deje de funcionar. Los siguientes comandos establecen la Directiva de aumento en "Rocket" para un aumento de estado más rápido y establecen la Directiva de reducción en "single". Los umbrales de aumento y reducción se establecen en 10 y 8, respectivamente.

``` syntax
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCPOL 2
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECPOL 1
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCTHRESHOLD 10
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECTHRESHOLD 8
Powercfg.exe /setactive scheme_current
```

## <a name="processor-performance-core-parking-maximum-and-minimum-cores"></a>Núcleos máximos y mínimos del aparcamiento de núcleos de rendimiento de procesador

El aparcamiento de núcleos es una característica que se presentó en Windows Server 2008 R2. El motor de administración de energía del procesador (PPM) y el programador trabajan juntos para ajustar de forma dinámica el número de núcleos disponibles para ejecutar subprocesos. El motor de PPM elige un número mínimo de núcleos para los subprocesos que se programarán.

Los núcleos que se detienen normalmente no tienen ningún subproceso programado y se colocarán en Estados de energía muy bajos cuando no estén procesando interrupciones, DPC u otro trabajo estrictamente afinidad con. Los núcleos restantes son responsables del resto de la carga de trabajo. El estacionamientodor principal puede aumentar la eficiencia energética durante un uso inferior.

Para la mayoría de los servidores, el comportamiento predeterminado del estacionamiento de núcleos proporciona un equilibrio razonable entre rendimiento y eficiencia energética. En el caso de los procesadores en los que es posible que el estacionamiento básico no muestre tanta ventaja en las cargas de trabajo genéricas, se puede deshabilitar de forma predeterminada.

Si el servidor tiene requisitos de estacionamiento de núcleos específicos, puede controlar el número de núcleos que están disponibles para el parque mediante el parámetro núcleos **máximos de aparcamiento de núcleos de rendimiento de procesador** o el parámetro de núcleos **mínimos de estacionamiento de núcleos de rendimiento** en Windows Server 2016.

Un escenario en el que el estacionamiento básico no es siempre óptimo para es cuando hay uno o más subprocesos activos afinidad con a un subconjunto no trivial de CPU en un nodo NUMA (es decir, más de 1 CPU, pero menor que el conjunto completo de CPU en el nodo). Cuando el algoritmo de estacionamiento principal es la selección de núcleos para desactivación (suponiendo que se produzca un aumento en la intensidad de la carga de trabajo), puede que no siempre elija los núcleos del subconjunto (o subconjuntos) afinidad con activos para desactivarlos y, por tanto, puede acabar con la reactivación de núcleos que no sean realmente utiliza.

Los valores de estos parámetros son porcentajes en el intervalo comprendido entre 0 y 100. El parámetro **núcleos máximos de estacionamiento de núcleos de rendimiento** controla el porcentaje máximo de núcleos que se pueden desactivar (disponibles para ejecutar subprocesos) en cualquier momento, mientras que el parámetro Core **performance Core estacionamiento** el número mínimo de núcleos que pueden desactivarse. Para desactivar el estacionamiento de núcleos, establezca el parámetro de **núcleos mínimos de estacionamiento de rendimiento de procesador** en 100 por ciento mediante los siguientes comandos:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMINCORES 100
Powercfg -setactive scheme_current
```

Para reducir el número de núcleos programables al 50 por ciento del recuento máximo, establezca el parámetro **núcleos máximos de estacionamiento de núcleos de rendimiento de procesador** en 50 como se indica a continuación:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMAXCORES 50
Powercfg -setactive scheme_current
```

## <a name="processor-performance-core-parking-utility-distribution"></a>Distribución de la utilidad de estacionamiento de núcleos de rendimiento

La distribución de utilidades es una optimización algorítmica en Windows Server 2016 que está diseñada para mejorar la eficacia de la energía de algunas cargas de trabajo. Realiza un seguimiento de la actividad de CPU no movible (es decir, DPC, interrupciones o subprocesos estrictamente afinidad con) y predice el trabajo futuro en cada procesador en función de la suposición de que cualquier trabajo móvil se puede distribuir equitativamente entre todos los núcleos desactivados.

La distribución de la utilidad está habilitada de forma predeterminada para el plan de energía equilibrado en algunos procesadores. Puede reducir el consumo de energía del procesador reduciendo las frecuencias de CPU solicitadas de las cargas de trabajo que se encuentran en un Estado razonablemente estable. Sin embargo, la distribución de la utilidad no es necesariamente una buena opción algorítmica para cargas de trabajo que están sujetas a ráfagas de actividad elevadas o a programas en los que la carga de trabajo se desplaza rápida y aleatoriamente entre procesadores.

Para estas cargas de trabajo, se recomienda deshabilitar la distribución de la utilidad mediante el uso de los siguientes comandos:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor DISTRIBUTEUTIL 0
Powercfg -setactive scheme_current
```

## <a name="see-also"></a>Consulta también
- [Consideraciones de rendimiento de hardware de servidor](../index.md)
- [Server Hardware Power Considerations](../power.md) (Consideraciones de alimentación del hardware de servidor)
- [Processor Power Management Tuning](processor-power-management-tuning.md) (Optimización de la administración de energía del procesador)
- [Recommended Balanced Plan Parameters](recommended-balanced-plan-parameters.md) (Parámetros recomendados del plan equilibrado)
