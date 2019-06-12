---
title: Capacidad y rendimiento de optimización
description: Administración de energía de procesador (PPM) ajuste para el Plan de energía equilibrada de Windows Server
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 4ad58e9b477f61844dedd9f6638efb12f1a96500
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811567"
---
# <a name="power-and-performance-tuning"></a>Energía y ajuste del rendimiento

La eficiencia de energía es cada vez más importante en entornos de centros de datos y enterprise, y agrega otro conjunto de equilibrios a la combinación de opciones de configuración.

Windows Server 2016 está optimizado para la eficiencia de energía excelente con un impacto mínimo en el rendimiento en una amplia variedad de cargas de trabajo cliente. [Optimización de administración de energía (PPM) de procesador para el Windows Server con equilibrio de Plan de energía](processor-power-management-tuning.md) describe las cargas de trabajo que se usan para ajustar los parámetros predeterminados en Windows Server 2016 y proporciona sugerencias de ajustes personalizadas.

En esta sección se expande sobre los inconvenientes de eficiencia energética para ayudarle a tomar decisiones informadas si necesita ajustar la configuración de energía predeterminada en el servidor. Sin embargo, la mayoría de las cargas de trabajo y hardware de servidor no debería requerir optimización de energía de administrador cuando se ejecuta Windows Server 2016.

## <a name="calculating-server-energy-efficiency"></a>Calcular la eficiencia de energía del servidor

Al optimizar el servidor para ahorro de energía, también debe considerar el rendimiento. Optimización afecta al rendimiento y potencia, a veces en una cantidad desproporcionada. Para cada ajuste posible, considere la posibilidad de los objetivos de presupuesto y el rendimiento de energía para determinar si esta reducción resulta aceptable.

Puede calcular la proporción de la eficiencia de energía de su servidor para una métrica útil que incorpora información de capacidad y rendimiento de. La eficiencia de energía es la proporción de trabajo que se realiza a la potencia de promedio que se necesita durante un período de tiempo especificado.

![fórmula de la eficiencia de energía](../../media/perftune-guide-power-formula.png)

Puede usar esta métrica para establecer objetivos prácticos que respetan el equilibrio entre potencia y rendimiento. En cambio, un objetivo de 10 por ciento de ahorro de energía en el centro de datos no puede capturar los efectos correspondientes en el rendimiento y viceversa.

De forma similar, si optimizar el servidor para aumentar el rendimiento al 5 por ciento y que da lugar a un 10 por ciento mayor consumo de energía, el resultado total podría o no puede ser aceptable para sus objetivos empresariales. La métrica de la eficiencia de energía permite más toma de decisiones información de las métricas de alimentación o de rendimiento por sí solo.

## <a name="measuring-system-energy-consumption"></a>Medir el consumo de energía del sistema

Debe establecer una medición de energía de la línea base antes de optimizar el servidor para la eficiencia de energía.

Si el servidor tiene la compatibilidad necesaria, puede utilizar la eficacia de las características de Windows Server 2016 de presupuesto y medición para ver el consumo de energía de nivel de sistema con el Monitor de rendimiento.

Una manera de determinar si el servidor dispone de soporte técnico para la medición y presupuestos consiste en revisar el [Windows Server Catalog](http://www.windowsservercatalog.com). Si el modelo de servidor es apto para la nueva calificación de administración de energía mejorada en el programa de certificación de Hardware de Windows, se garantiza para admitir la funcionalidad de elaboración de presupuestos y medición.

Comprobar disponibilidad de soporte técnico de otra manera consiste en buscar manualmente los contadores del Monitor de rendimiento. Abra el Monitor de rendimiento, seleccione **agregar contadores**y, a continuación, busque el **medidor de energía** grupo de contadores.

Si las instancias de medidores de energía con nombre aparecen en la casilla de verificación **instancias del objeto seleccionado**, admite la plataforma de medición. El **Power** contador que muestra la energía en vatios aparece en el grupo de contadores seleccionado. No se especificó la derivación exacta de los datos encendido. Por ejemplo, podría ser un consumo de energía instantáneo o un consumo de energía promedio en cierto intervalo de tiempo.

Si su plataforma de servidor no admite la medición, puede usar un dispositivo de medición físico conectado a la entrada de la fuente de alimentación de energía para medir el consumo de dibujo o la energía de energía del sistema.

Para establecer una línea base, debe medir la eficacia de promedio necesaria en diversos puntos de carga de inactivo al 100 por ciento (rendimiento máximo) para generar una línea de la carga del sistema. La siguiente figura muestra líneas de carga para las tres configuraciones de ejemplo:

![líneas de carga de ejemplo](../../media/perftune-guide-sample-loadlines.png)

Puede usar líneas de carga para evaluar y comparar el rendimiento y consumo de energía de las configuraciones de carga en todos los puntos. En este ejemplo concreto, es fácil ver lo que es la mejor configuración. Sin embargo, fácilmente puede haber escenarios donde una configuración funciona mejor para cargas de trabajo y uno funciona mejor para cargas de trabajo ligeras.

Debe entender perfectamente los requisitos de carga de trabajo para elegir una configuración óptima. No dé por sentado que cuando encuentre una buena configuración, siempre permanecerá óptimo. Debe medir sistema utilización y consumo de energía de forma periódica y después de realizar cambios en las cargas de trabajo, los niveles de carga de trabajo o en hardware de servidor.

## <a name="diagnosing-energy-efficiency-issues"></a>Diagnóstico de problemas de la eficiencia de energía

**PowerCfg.exe** admite una opción de línea de comandos que puede usar para analizar la eficacia de energía de inactividad del servidor. Al ejecutar PowerCfg.exe con el **/energy** opción, la herramienta realiza una prueba de 60 segundos para detectar problemas de la eficiencia de energía potencial. La herramienta genera un informe HTML sencillo en el directorio actual.

> [!Important]
> Para garantizar un análisis preciso, asegúrese de que todas las aplicaciones locales se cierran antes de ejecutar **PowerCfg.exe**. 

Abreviado tasas de graduación temporizador, los controladores que compatibilidad con la administración de energía de ausencia y el uso excesivo de CPU son algunos de los problemas de comportamiento que se detectan mediante el **powercfg /energy** comando. Esta herramienta proporciona una manera sencilla de identificar y corregir problemas de administración de energía, lo que puede producir un importante ahorro económico en un centro de datos grande.

Para obtener más información acerca de PowerCfg.exe, consulte [usar PowerCfg para evaluar la eficiencia de energía del sistema](https://msdn.microsoft.com/windows/hardware/gg463250.aspx).

## <a name="using-power-plans-in-windows-server"></a>Uso de los planes de energía en Windows Server

Windows Server 2016 tiene tres planes de energía integrados diseñados para satisfacer diferentes conjuntos de necesidades empresariales. Estos planes proporcionan una manera sencilla de personalizar un servidor para cumplir los objetivos de rendimiento o de alimentación. En la tabla siguiente se describe los planes, se enumera los escenarios comunes en los que se va a usar cada plan y proporciona algunos detalles de implementación para cada plan.

| **Plan** | **Descripción** | **Escenarios aplicables comunes** | **Información destacada de la implementación** |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Equilibrado (recomendado) | Configuración predeterminada. Se dirige a la eficiencia de energía buena con un impacto mínimo en el rendimiento. | Informática general | Coincide con la capacidad a petición. Características de ahorro de energía equilibran la eficacia y el rendimiento. |
| Alto rendimiento | Aumenta el rendimiento a costa de elevado consumo de energía. Energía y térmicas limitaciones, funcionamiento de gastos y las consideraciones de confiabilidad se aplican. | Las aplicaciones de baja latencia y el código de aplicación que es sensible a los cambios de rendimiento del procesador | Siempre se bloquean los procesadores en el estado de rendimiento más alto (incluidas las frecuencias de "turbo"). Todos los núcleos se unparked. Salida térmica puede ser importante. |
| Ahorro de energía | Limita el rendimiento para ahorrar energía y reducir los costos operativos. No se recomienda sin realizar pruebas minuciosas ofrecer un rendimiento seguro es adecuada. | Implementaciones con presupuestos limitada de energía y restricciones térmicas | Limita la frecuencia del procesador en un porcentaje de máximo (si se admite) y permite que otras características de ahorro de energía. |


Estos planes de energía existen en Windows de corriente alterna (CA) y los sistemas con tecnología de corriente (continua CC), pero, supondremos que servidores siempre usa una fuente de alimentación de CA.

Para obtener más información sobre los planes de energía y las configuraciones de directiva de energía, consulte [configuración de directiva de energía e implementación en Windows](https://msdn.microsoft.com/windows/hardware/gg463243.aspx).

> [!Note]
> Algunos fabricantes de servidor tienen sus propias opciones de administración de energía disponibles a través de la configuración del BIOS. Si el sistema operativo no tiene control sobre la administración de energía, cambiar los planes de energía en Windows no afectará a rendimiento y la alimentación del sistema.

## <a name="tuning-processor-power-management-parameters"></a>Ajuste los parámetros de administración de energía de procesador

Cada plan de energía representa una combinación de los numerosos parámetros de administración de energía subyacente. Los planes integrados son tres colecciones de la configuración recomendada que abarcan una amplia variedad de escenarios y las cargas de trabajo. Sin embargo, reconocemos que estos planes no cumple las necesidades de cada cliente.

Las siguientes secciones describen formas de ajustar algunos parámetros de administración de power de procesador específico para cumplir los objetivos no resuelve mediante los tres planes. Si necesita comprender una variedad de parámetros de energía, consulte [configuración de directiva de energía e implementación en Windows](https://msdn.microsoft.com/windows/hardware/gg463243.aspx).

## <a name="processor-performance-boost-mode"></a>Modo de aumento de rendimiento de procesador

Las tecnologías de Intel Turbo Boost y AMD Turbo CORE son características que permiten procesadores lograr un rendimiento adicional cuando resulta más útil (es decir, en el sistema de alta carga). Sin embargo, esta característica aumenta el consumo de energía del núcleo de CPU, por lo que Windows Server 2016 configura tecnologías Turbo según la directiva de energía que está en uso y la implementación de procesador específico.

Turbo está habilitada para los planes de energía de alto rendimiento en todos los procesadores de Intel y AMD y está deshabilitada para los planes de energía ahorro de energía. Los planes de energía equilibrada en los sistemas que se basan en la administración tradicional de frecuencia basado en estado P, Turbo está habilitado de forma predeterminada solo si la plataforma es compatible con el registro EPB.

> [!Note]
> Solo se admite el registro EPB en Intel Westmere y procesadores más adelante.

Para los procesadores de AMD e Intel Nehalem, Turbo está deshabilitada de forma predeterminada en plataformas basadas en el estado de P. Sin embargo, si un sistema es compatible con Control de rendimiento de colaboración procesador (CPPC), es un nuevo modo alternativo de comunicación de rendimiento entre el sistema operativo y el hardware (definido en ACPI 5.0), Turbo podría exponerse a si el funcionamiento de Windows sistema solicita dinámicamente el hardware para ofrecer los mayores niveles de rendimiento posible.

Para habilitar o deshabilitar la función Turbo Boost, se debe configurar el parámetro de modo de aumento de rendimiento de procesador por el administrador o por la configuración de parámetros predeterminados para el plan de energía seleccionado. Modo de aumento de rendimiento de procesador tiene cinco valores permitidos, como se muestra en la tabla 5.

Para un control basado en el estado de P, se deshabilitan las opciones, habilitado (está disponible para el hardware Turbo siempre que se solicita el rendimiento nominal) y eficaz (Turbo solo está disponible si se implementa el registro EPB).

Para un control basado en CPPC, se deshabilitan las opciones, habilitado eficaz (Windows especifica la cantidad exacta de Turbo para proporcionar) y dinámico (Windows pidan "máximo rendimiento" para habilitar Turbo).

En Windows Server 2016, el valor predeterminado para el modo de aumento es 3.

| **Name** | **Comportamiento basado en estado P** | **Comportamiento CPPC** |
|--------------------------|------------------------|-------------------|
| 0 (deshabilitado) | Deshabilitada | Deshabilitada |
| 1 (habilitado) | Enabled | Eficaz habilitado |
| 2 (dinámico) | Enabled | Agresiva |
| 3 (eficaz habilitado) | Eficaz | Eficaz habilitado |
| 4 (eficaz agresiva) | Eficaz | Agresiva |

 
Los siguientes comandos de habilitar el modo de aumento de rendimiento de procesador en el plan de energía actual (especificar la directiva mediante el uso de un alias de GUID):

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_current
```

> [!Important]
> Debe ejecutar el **powercfg - setactive** comando para habilitar la nueva configuración. No es necesario reiniciar el servidor.

Para establecer este valor para los planes de energía que no sea el plan seleccionado actualmente, puede utilizar alias como esquema\_MAX (Economizador de energía), combinación\_MIN (alto rendimiento) y el esquema\_EQUILIBRADO (equilibrada) en lugar de esquema\_Actual. Reemplace "esquema actual" en los comandos de powercfg - setactive mostrado anteriormente con el alias deseado para habilitar ese plan de energía.

Por ejemplo ajustar el modo de aumento en el plan economizador de energía y hacer que ese Economizador de energía es el plan actual, ejecute los siguientes comandos:

``` syntax
Powercfg -setacvalueindex scheme_max sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_max
```

## <a name="minimum-and-maximum-processor-performance-state"></a>Como mínimo y el estado de rendimiento máximo del procesador

Procesadores cambian entre los Estados de rendimiento (Estados P) muy rápidamente a la oferta de coincidencia a la demanda, proporcionando un rendimiento cuando sea necesario y ahorra energía cuando sea posible. Si el servidor tiene requisitos concretos de alto rendimiento o consumo de energía como mínimo, es posible que considere la posibilidad de configurar el **estado de rendimiento del procesador mínima** parámetro o el **máximo rendimiento del procesador Estado** parámetro.

Los valores de la **estado de rendimiento del procesador mínima** y **estado de rendimiento del procesador máximo** los parámetros se expresan como un porcentaje de frecuencia máxima del procesador, con un valor en el intervalo de 0: 100.

Si el servidor requiere una latencia muy baja, frecuencia de CPU invariable (por ejemplo, para realizar pruebas repetibles) o los niveles más altos de rendimiento, no podría los procesadores de conmutación a Estados de rendimiento inferior. Para este tipo de servidor, puede limitar el estado de rendimiento del procesador mínima al 100 por cien mediante los comandos siguientes:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMIN 100
Powercfg -setactive scheme_current
```

Si el servidor requiere menor consumo de energía, puede limitar el estado de rendimiento del procesador en un porcentaje del máximo. Por ejemplo, puede restringir el procesador al 75 por ciento de su frecuencia máxima mediante el uso de los siguientes comandos:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMAX 75
Powercfg -setactive scheme_current
```

> [!Note]
> Limita el rendimiento del procesador en un porcentaje de máximo, requiere compatibilidad con procesadores. Consulte la documentación de procesador para determinar si existe dicha compatibilidad, o ver el contador del Monitor de rendimiento **% de frecuencia máxima** en el **procesador** grupo para ver si se produjeron los límites de frecuencia aplicar.

## <a name="processor-performance-increase-and-decrease-of-thresholds-and-policies"></a>Rendimiento del procesador aumentan y disminuyen de umbrales y directivas

La velocidad a la que un estado de rendimiento del procesador aumenta o disminuye se controla mediante varios parámetros. Los cuatro parámetros siguientes tienen el impacto más visible:

-   **Umbral de aumentar el rendimiento de procesador** define el valor de uso anterior lo que aumentará el estado de rendimiento del procesador. Los valores mayores ralentizar la tasa de crecimiento para el estado de rendimiento en respuesta a las actividades de mayor.

-   **Umbral de reducir el rendimiento de procesador** define el valor de uso siguiente que se reducirá el estado de rendimiento del procesador. Los valores mayores aumentan la tasa de disminución del estado de rendimiento durante los períodos de inactividad.

-   **Directiva de aumentar el rendimiento del procesador y reducir el rendimiento de procesador** directiva determinar qué estado de rendimiento se debe establecer cuando se produce un cambio. Directiva de "Único" significa que elige el siguiente estado. "Rocket" significa que el estado de rendimiento máximo o mínimo de energía. "Ideal" intenta encontrar un equilibrio entre la capacidad y rendimiento.

Por ejemplo, si el servidor requiere una latencia muy baja aún desean beneficiarse de bajo consumo de energía durante los períodos de inactividad, podría quicken el aumento de estado de rendimiento para el aumento de carga y ralentizar la disminución cuando carga deja de funcionar. Los siguientes comandos de establecer la directiva de aumento a "Rocket" para un rápido aumento de estado y establecer la directiva de disminución a "Single". Los umbrales de aumento y disminución se establecen a 10 y 8 respectivamente.

``` syntax
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCPOL 2
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECPOL 1
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCTHRESHOLD 10
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECTHRESHOLD 8
Powercfg.exe /setactive scheme_current
```

## <a name="processor-performance-core-parking-maximum-and-minimum-cores"></a>Núcleo del procesador rendimiento máximo y mínimos de núcleos de estacionamiento

Estacionamiento Core es una característica que se introdujo en Windows Server 2008 R2. El motor de procesador power management (PPM) y el programador colaboran para ajustar el número de núcleos que están disponibles para ejecutar subprocesos de forma dinámica. El motor de PPM elige un número mínimo de núcleos para los subprocesos que se programará.

Núcleos que están aparcados generalmente no tienen ningún subproceso programado y eliminará en los Estados de muy bajo consumo de energía cuando no están procesando otros trabajos estrictamente con afinidad, DPC o interrupciones. Los núcleos restantes son responsables para el resto de la carga de trabajo. Estacionamiento Core potencialmente puede aumentar la eficiencia de energía durante el uso más bajo.

Para la mayoría de los servidores, el comportamiento de core estacionamiento predeterminado proporciona un equilibrio razonable de rendimiento y la eficiencia energética. En procesadores donde estacionamiento core no puede mostrar tanta ventaja en cargas de trabajo genéricas, se puede deshabilitar de forma predeterminada.

Si el servidor tiene core específico de estacionamiento requisitos, puede controlar el número de núcleos que están disponibles para estacionar utilizando el **estacionamiento máximo núcleos del procesador rendimiento Core** parámetro o la **procesador Núcleos de rendimiento principales de estacionamiento mínimo** parámetro en Windows Server 2016.

Un escenario que estacionamiento core no siempre es óptimo para es cuando hay uno o más subprocesos activos con afinidad con un subconjunto no trivial de CPU en un nodo NUMA (es decir, más de 1 CPU, pero menor que el conjunto completo de la CPU en el nodo). Cuando el algoritmo de estacionamiento principal está recopilando núcleos a unpark (suponiendo que se produce un aumento de la intensidad de la carga de trabajo), es posible que no siempre elegir los núcleos dentro del subconjunto con afinidad activo (o subconjuntos) a unpark y, por tanto, es posible que acaban unparking de núcleos que realmente no será la designación.

Los valores para estos parámetros son porcentajes en el intervalo de 0 a 100. El **estacionamiento máximo núcleos del procesador rendimiento Core** parámetro controla el porcentaje máximo de núcleos que puede ser unparked (disponible para ejecutar subprocesos) en cualquier momento, mientras que el **estacionamiento del núcleo de procesador rendimiento Núm. mínimo de núcleos** parámetro controla el porcentaje mínimo de núcleos que puede ser unparked. Para desactivar el estacionamiento core, establezca el **núcleos del procesador rendimiento Core estacionamiento mínimo** parámetro al 100 por ciento mediante los comandos siguientes:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMINCORES 100
Powercfg -setactive scheme_current
```

Para reducir el número de núcleos programables al 50 por ciento del número máximo, establezca el **estacionamiento máximo núcleos del procesador rendimiento Core** parámetro a 50 como sigue:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMAXCORES 50
Powercfg -setactive scheme_current
```

## <a name="processor-performance-core-parking-utility-distribution"></a>Distribución de la utilidad de estacionamiento núcleo del procesador rendimiento

Utilidad de distribución es una optimización algorítmica en Windows Server 2016 está diseñado para mejorar la eficiencia de energía para algunas cargas de trabajo. Realiza un seguimiento de actividad de CPU inamovible (es decir, las DPC, interrupciones o estrictamente con afinidad de subprocesos), y predice el trabajo futuro en cada procesador según la suposición de que se puede distribuir cualquier trabajo que se puede mover por igual en todos los núcleos unparked.

Utilidad de distribución está habilitado de forma predeterminada para el plan de energía equilibrada para algunos procesadores. Puede reducir el consumo de energía de procesador reduciendo las frecuencias de CPU solicitadas de cargas de trabajo que se encuentran en un estado razonablemente estable. Sin embargo, utilidad de distribución no es necesariamente una buena elección algorítmica para cargas de trabajo que están sujetos a picos de actividad alta o para programas donde la carga de trabajo de forma rápida y aleatoriamente se desplaza entre los procesadores.

Para estas cargas de trabajo, se recomienda deshabilitar la distribución de utilidad mediante el uso de los siguientes comandos:

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor DISTRIBUTEUTIL 0
Powercfg -setactive scheme_current
```

## <a name="see-also"></a>Vea también
- [Consideraciones de rendimiento del Hardware de servidor](../index.md)
- [Server Hardware Power Considerations](../power.md) (Consideraciones de alimentación del hardware de servidor)
- [Processor Power Management Tuning](processor-power-management-tuning.md) (Optimización de la administración de energía del procesador)
- [Recommended Balanced Plan Parameters](recommended-balanced-plan-parameters.md) (Parámetros recomendados del plan equilibrado)
