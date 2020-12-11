---
ms.assetid: 01c8cece-66ce-4a83-a81e-aa6cc98e51fc
title: Configuración avanzada de Desduplicación de datos
description: 'Más información sobre: configuración avanzada de desduplicación de datos'
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/15/2016
ms.openlocfilehash: aa5f34f1c1fdc2b2a706cb0dfe1b7cf5a30456eb
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97039203"
---
# <a name="advanced-data-deduplication-settings"></a>Configuración avanzada de Desduplicación de datos

> Se aplica a Windows Server (canal semianual), Windows Server 2016

En este documento se describe cómo modificar la configuración avanzada de [Desduplicación de datos](overview.md). Para las [cargas de trabajo recomendadas](install-enable.md#enable-dedup-candidate-workloads), la configuración predeterminada debería ser suficiente. La razón principal para modificar esta configuración es mejorar el rendimiento de Desduplicación de datos con otros tipos de cargas de trabajo.

## <a name="modifying-data-deduplication-job-schedules"></a><a id="modifying-job-schedules"></a>Modificación de programaciones de trabajos de Desduplicación de datos
Las [programaciones predeterminadas de trabajos de Desduplicación de datos](understand.md#job-info) están diseñadas para funcionar bien con las cargas de trabajo recomendadas y ser lo menos intrusivas como sea posible (sin incluir el trabajo *Optimización de prioridad* que está habilitado para el tipo de uso [**Copia de seguridad**](understand.md#usage-type-backup)). Cuando las cargas de trabajo tienen muchos requisitos de recursos, es posible garantizar que los trabajos se ejecuten solo durante las horas de inactividad, o bien reducir o aumentar la cantidad de recursos del sistema que un trabajo de Desduplicación de datos puede consumir.

### <a name="changing-a-data-deduplication-schedule"></a><a id="modifying-job-schedules-change-schedule"></a>Modificación de una programación de Desduplicación de datos
Los trabajos de Desduplicación de datos se programan a través del programador de tareas de Windows y pueden verse y modificarse en la ruta de acceso Microsoft\Windows\Deduplication. Desduplicación de datos incluye varios cmdlets que facilitan la programación.
* [`Get-DedupSchedule`](https://docs.microsoft.com/powershell/module/deduplication/get-dedupschedule) muestra los trabajos programados actuales.
* [`New-DedupSchedule`](https://docs.microsoft.com/powershell/module/deduplication/new-dedupschedule) crea un nuevo trabajo programado.
* [`Set-DedupSchedule`](https://docs.microsoft.com/powershell/module/deduplication/set-dedupschedule) modifica un trabajo programado existente.
* [`Remove-DedupSchedule`](https://docs.microsoft.com/powershell/module/deduplication/remove-dedupschedule) quita un trabajo programado.

La razón más común para cambiar cuando se ejecutan los trabajos de Desduplicación de datos es asegurarse de que los trabajos se ejecutan fuera del horario laboral. En el siguiente ejemplo paso a paso muestra cómo modificar la programación de Desduplicación de datos para un escenario de *día soleado*: un host de Hyper-V hiperconvergido que está inactivo los fines de semana y después de las 19:00. Para cambiar la programación, ejecute los siguientes cmdlets de PowerShell en un contexto de administrador.

1. Deshabilite los trabajos programados de [Optimización](understand.md#job-info-optimization) cada hora.
    ```PowerShell
    Set-DedupSchedule -Name BackgroundOptimization -Enabled $false
    Set-DedupSchedule -Name PriorityOptimization -Enabled $false
    ```

2. Quite los trabajos actualmente programados [Recolección de elementos no deseados](understand.md#job-info-gc) y [Limpieza de integridad](understand.md#job-info-scrubbing).
    ```PowerShell
    Get-DedupSchedule -Type GarbageCollection | ForEach-Object { Remove-DedupSchedule -InputObject $_ }
    Get-DedupSchedule -Type Scrubbing | ForEach-Object { Remove-DedupSchedule -InputObject $_ }
    ```

3. Cree una tarea nocturna de optimización que se ejecute a las 19:00, con prioridad alta y todas las CPU y memoria disponible en el sistema.
    ```PowerShell
    New-DedupSchedule -Name "NightlyOptimization" -Type Optimization -DurationHours 11 -Memory 100 -Cores 100 -Priority High -Days @(1,2,3,4,5) -Start (Get-Date "2016-08-08 19:00:00")
    ```

    >[!NOTE]
    > La parte de *fecha* de `System.Datetime` proporcionada para `-Start` es irrelevante (mientras esté en el pasado), pero la parte de *hora* especifica cuándo debe comenzar el trabajo.
4. Cree un trabajo semanal de recolección de elementos no deseados que se ejecuta el sábado desde las 7:00 con prioridad elevada y todas las CPU y memoria disponibles en el sistema.
    ```PowerShell
    New-DedupSchedule -Name "WeeklyGarbageCollection" -Type GarbageCollection -DurationHours 23 -Memory 100 -Cores 100 -Priority High -Days @(6) -Start (Get-Date "2016-08-13 07:00:00")
    ```

5. Cree un trabajo semanal de limpieza de integridad que se ejecuta el domingo desde las 7:00 con prioridad elevada y con todas las CPU y memoria disponibles en el sistema.
    ```PowerShell
    New-DedupSchedule -Name "WeeklyIntegrityScrubbing" -Type Scrubbing -DurationHours 23 -Memory 100 -Cores 100 -Priority High -Days @(0) -Start (Get-Date "2016-08-14 07:00:00")
    ```

### <a name="available-job-wide-settings"></a><a id="modifying-job-schedules-available-settings"></a>Configuración para todo el trabajo disponible
Puede alternar la siguiente configuración para los trabajos de Desduplicación de datos nuevos o programados:

<table>
    <thead>
        <tr>
            <th style="min-width:125px">Nombre de parámetro</th>
            <th>Definición</th>
            <th>Valores aceptados</th>
            <th>¿Por qué se quiere establecer este valor?</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Tipo</td>
            <td>El tipo de trabajo que se debe programar</td>
            <td>
                <ul>
                    <li>Optimization</li>
                    <li>GarbageCollection</li>
                    <li>Scrubbing</li>
                </ul>
            </td>
            <td>Este valor es necesario porque es el tipo de trabajo que quiere programar. No se puede cambiar este valor después una vez programada la tarea.</td>
        </tr>
        <tr>
            <td>Priority</td>
            <td>La prioridad del trabajo programado del sistema</td>
            <td>
                <ul>
                    <li>Alto</li>
                    <li>Media</li>
                    <li>Bajo</li>
                </ul>
            </td>
            <td>Este valor ayuda a que el sistema determinar cómo asignar el tiempo de CPU. <em>High</em> utilizará más tiempo de CPU, <em>Low</em> utilizará menos.</td>
        </tr>
        <tr>
            <td>Días</td>
            <td>Los días en que los que el trabajo está programado</td>
            <td>Una matriz de enteros de 0 a 6 que representan los días de la semana:<ul>
                <li>0 = Domingo</li>
                <li>1 = Lunes</li>
                <li>2 = Martes</li>
                <li>3 = Miércoles</li>
                <li>4 = Jueves</li>
                <li>5 = Viernes</li>
                <li>6 = Sábado</li>
            </ul></td>
            <td>Las tareas programadas se tienen que ejecutar en al menos un día.</td>
        </tr>
        <tr>
            <td>Núcleos</td>
            <td>El porcentaje de núcleos en el sistema que debe usar un trabajo</td>
            <td>Enteros de 0 a 100 (indica un porcentaje)</td>
            <td>Para controlar qué nivel de impacto tendrá un trabajo en los recursos de proceso en el sistema</td>
        </tr>
        <tr>
            <td>DurationHours</td>
            <td>Número máximo de horas que puede ejecutarse un trabajo</td>
            <td>Números enteros positivos</td>
            <td>Para evitar que un trabajo se ejecute en una carga de trabajo&#39;s horas de inactividad</td>
        </tr>
        <tr>
            <td>Habilitado</td>
            <td>Si el trabajo se ejecuta</td>
            <td>Verdadero o falso</td>
            <td>Para deshabilitar un trabajo sin quitarlo</td>
        </tr>
        <tr>
            <td>Completo</td>
            <td>Para programar un trabajo de recolección completa de elementos no utilizados</td>
            <td>Modificador (verdadero/falso)</td>
            <td>De forma predeterminada, cada cuarto trabajo es un trabajo de recolección completa de elementos no utilizados. Con este modificador, puede programar la recolección completa de elementos no utilizados con más frecuencia.</td>
        </tr>
        <tr>
            <td>InputOutputThrottle</td>
            <td>Especifica la cantidad de limitación de entrada y salida aplicada al trabajo</td>
            <td>Enteros de 0 a 100 (indica un porcentaje)</td>
            <td>La limitación garantiza que los trabajos que no&#39;t interfieren con otros procesos intensivos de e/s.</td>
        </tr>
        <tr>
            <td>Memoria</td>
            <td>El porcentaje de memoria en el sistema que debe usar un trabajo</td>
            <td>Enteros de 0 a 100 (indica un porcentaje)</td>
            <td>Para controlar qué nivel de impacto tendrá un trabajo en los recursos de memoria del sistema</td>
        </tr>
        <tr>
            <td>NOMBRE</td>
            <td>Nombre del trabajo programado</td>
            <td>String</td>
            <td>Un trabajo debe tener un nombre identificable de forma única.</td>
        </tr>
        <tr>
            <td>ReadOnly</td>
            <td>Indica los informes y procesos de trabajo de limpieza en los daños que encuentra, pero no se ejecuta ninguna acción de reparación.</td>
            <td>Modificador (verdadero/falso)</td>
            <td>Quiere restaurar manualmente los archivos que residen en las secciones defectuosas del disco.</td>
        </tr>
        <tr>
            <td>Inicio</td>
            <td>Especifica la hora en la que se debe iniciar un trabajo.</td>
            <td><code>System.DateTime</code></td>
            <td>La parte de <em>fecha</em> del <code>System.Datetime</code> proporcionado para <em>iniciar</em> es irrelevante (siempre y cuando&#39;s en el pasado), pero la parte de <em>hora</em> especifica cuándo debe comenzar el trabajo.</td>
        </tr>
        <tr>
            <td>StopWhenSystemBusy</td>
            <td>Especifica si debe detenerse Desduplicación de datos si el sistema está ocupado.</td>
            <td>Modificador (verdadero/falso)</td>
            <td>Este modificador le ofrece la capacidad de controlar el comportamiento de Desduplicación de datos. Esto es especialmente importante si desea ejecutar Desduplicación de datos mientras la carga de trabajo no esté inactiva.</td>
        </tr>
    </tbody>
</table>

## <a name="modifying-data-deduplication-volume-wide-settings"></a><a id="modifying-volume-settings"></a>Modificación de la configuración para todo el volumen de Desduplicación de datos
### <a name="toggling-volume-settings"></a><a id="modifying-volume-settings-how-to-toggle"></a>Alternancia de la configuración del volumen
Puede establecer la configuración predeterminada para todo el volumen para Desduplicación de datos a través del [tipo de uso](understand.md#usage-type) que se selecciona cuando se habilita la desduplicación para un volumen. Desduplicación de datos incluye cmdlets que facilitan la modificación de la configuración para todos los volúmenes:

* [`Get-DedupVolume`](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730705(v=ws.11))
* [`Set-DedupVolume`](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730705(v=ws.11))

Las razones principales para modificar la configuración de volumen desde el tipo de uso seleccionado son mejorar el rendimiento de lectura para archivos específicos (como multimedia u otros tipos de archivo que ya están comprimidos) u optimizar Desduplicación de datos para una mejor optimización de la carga de trabajo específica. En el ejemplo siguiente se muestra cómo modificar la configuración de volumen de Desduplicación de datos para una carga de trabajo que más se parece a una carga de trabajo de servidor de archivos de propósito general, pero utiliza archivos grandes que cambian con frecuencia.

1. Vea la configuración actual del volumen para Volumen compartido de clúster 1.
    ```PowerShell
    Get-DedupVolume -Volume C:\ClusterStorage\Volume1 | Select *
    ```

2. Habilite OptimizePartialFiles en Volumen compartido de clúster 1 para que la directiva MinimumFileAge se aplique a las secciones del archivo en lugar de a todo el archivo. Esto garantiza que la mayor parte del archivo se optimice aunque secciones del archivo cambien regularmente.
    ```PowerShell
    Set-DedupVolume -Volume C:\ClusterStorage\Volume1 -OptimizePartialFiles
    ```

### <a name="available-volume-wide-settings"></a><a id="modifying-volume-settings-available-settings"></a>Configuración para todo el volumen disponible
<table>
    <thead>
        <tr>
            <th style="min-width:125px">Nombre del valor</th>
            <th>Definición</th>
            <th>Valores aceptados</th>
            <th>¿Por qué se quiere modificar este valor?</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ChunkRedundancyThreshold</td>
            <td>El número de veces al que se hace referencia a un fragmento antes de que se duplique un fragmento en la sección de la zona activa del almacén de fragmentos. El valor de la sección HotSpot es que, a los &quot; que &quot; se hace referencia con frecuencia, los fragmentos activos a los que se hace referencia tienen varias rutas de acceso para mejorar el tiempo de acceso.</td>
            <td>Números enteros positivos</td>
            <td>La razón principal para modificar este número es aumentar la velocidad del ahorro de volúmenes con desduplicación alta. En general, el valor predeterminado (100) es la configuración recomendada y debe tener&#39;t para modificarlo.</td>
        </tr>
        <tr>
            <td>ExcludeFileType</td>
            <td>Tipos de archivo que se excluyen de la optimización</td>
            <td>Matriz de extensiones de archivos</td>
            <td>Algunos tipos de archivo, especialmente multimedia o archivos que ya están comprimidos, no se beneficiarán mucho de la optimización. Esta configuración le permite configurar los tipos que se excluyen.</td>
        </tr>
        <tr>
            <td>ExcludeFolder</td>
            <td>Especifica las rutas de acceso de la carpeta no deben considerarse para la optimización.</td>
            <td>Matriz de rutas de acceso de carpeta</td>
            <td>Si desea mejorar el rendimiento o impedir que el contenido en rutas de acceso particulares se optimice, puede excluir determinadas rutas de acceso en el volumen de la optimización.</td>
        </tr>
        <tr>
            <td>InputOutputScale</td>
            <td>Especifica el nivel de la paralelización de E/S (colas de E/S) para que Desduplicación de datos la usará en un volumen durante un trabajo de procesamiento posterior.</td>
            <td>Números enteros positivos entre 1 y 36</td>
            <td>La razón principal para modificar este valor es reducir el impacto en el rendimiento de una carga de trabajo de elevada E/S al restringir el número de colas de E/S que Desduplicación de datos puede usar en un volumen. Tenga en cuenta que la modificación de esta configuración de forma predeterminada puede provocar que los trabajos de desduplicación de datos&#39;s posteriores al procesamiento se ejecuten lentamente.</td>
        </tr>
        <tr>
            <td>MinimumFileAgeDays</td>
            <td>Número de días después de crear el archivo antes de que el archivo se considere apto para la directiva de optimización.</td>
            <td>Números enteros positivos (incluido el cero)</td>
            <td>Los tipos de uso <strong>Predeterminado</strong> e <strong>Hyper-v</strong> establecen este valor en 3 para maximizar el rendimiento en los archivos activos o recientemente creados. Puede modificar este comportamiento si desea que Desduplicación de datos sea más agresivo o si no le importa la latencia adicional asociada con la desduplicación.</td>
        </tr>
        <tr>
            <td>MinimumFileSize</td>
            <td>Tamaño mínimo de archivo que un archivo debe tener para considerarse apto para la directiva de optimización</td>
            <td>Enteros positivos (bytes) mayores de 32 kB</td>
            <td>La razón principal para cambiar este valor es excluir archivos pequeños que pueden haber limitado el valor de optimización para conservar el tiempo de proceso.</td>
        </tr>
        <tr>
            <td>NoCompress</td>
            <td>Si los fragmentos se deben comprimir antes de pasar al almacén de fragmentos</td>
            <td>True/False</td>
            <td>Algunos tipos de archivos, especialmente archivos multimedia y tipos de archivos ya comprimidos, pueden no comprimirse bien. Esta configuración permite desactivar la compresión para todos los archivos del volumen. Esto sería ideal si se está optimizando un conjunto de datos que tiene una gran cantidad de archivos que ya están comprimidos.</td>
        </tr>
        <tr>
            <td>NoCompressionFileType</td>
            <td>Tipos de archivo cuyos fragmentos no se deben comprimir antes de pasar al almacén de fragmentos</td>
            <td>Matriz de extensiones de archivos</td>
            <td>Algunos tipos de archivos, especialmente archivos multimedia y tipos de archivos ya comprimidos, pueden no comprimirse bien. Esta configuración permite que la compresión se desactive en esos archivos, ahorrando recursos de CPU.</td>
        </tr>
        <tr>
            <td>OptimizeInUseFiles</td>
            <td>Cuando está habilitada esta opción, los archivos que tienen controladores activos en ellos se considerarán aptos para la directiva de optimización.</td>
            <td>Verdadero o falso</td>
            <td>Habilite esta opción si la carga de trabajo mantiene los archivos abiertos durante largos períodos. Si esta opción no está habilitada, un archivo nunca se optimizará si la carga de trabajo tiene un identificador abierto, incluso si&#39;s solo se anexan datos al final de forma ocasional.</td>
        </tr>
        <tr>
            <td>OptimizePartialFiles</td>
            <td>Cuando está habilitada, el valor de MinimumFileAge se aplica a los segmentos de un archivo en lugar de todo el archivo.</td>
            <td>Verdadero o falso</td>
            <td>Habilite a esta opción si la carga de trabajo funciona con archivos grandes, a menudo archivos modificados en los que la mayor parte del contenido del archivo está sin tocar. Si esta opción no está habilitada, estos archivos no se optimizarán nunca porque conservan los cambios, aunque la mayor parte del contenido del archivo está listo para su optimización.</td>
        </tr>
        <tr>
            <td>Verify</td>
            <td>Cuando se habilita, si el hash de un fragmento coincide con un fragmento que ya se encuentra en nuestro almacén de fragmentos, los fragmentos se comparan byte a byte para asegurarse de que son idénticos.</td>
            <td>Verdadero o falso</td>
            <td>Se trata de una característica de integridad que garantiza que el algoritmo hash que compara los fragmentos no comete un error al comparar dos fragmentos de datos que son realmente diferentes pero tienen el mismo valor hash. En la práctica, es muy improbable que esto ocurra. Si habilita la característica de comprobación, se agrega una sobrecarga considerable al trabajo de optimización.</td>
        </tr>
    </tbody>
</table>

## <a name="modifying-data-deduplication-system-wide-settings"></a><a id="modifying-dedup-system-settings"></a>Modificación de la configuración para todo el sistema de Desduplicación de datos
Desduplicación de datos tiene opciones adicionales para todo el sistema que se pueden configurar a través del [Registro](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755256(v=ws.11)). Esta configuración se aplica a todos los trabajos y volúmenes que se ejecutan en el sistema. Deben extremarse las precauciones siempre que se modifique el Registro.

Por ejemplo, puede querer deshabilitar la recolección de elementos no utilizados completa. Para más información acerca de por qué puede resultar útil para el escenario, consulte las [preguntas más frecuentes](#faq-why-disable-full-gc). Para editar el Registro con PowerShell:

* Si Desduplicación de datos se ejecuta en un clúster:
    ```PowerShell
    Set-ItemProperty -Path HKLM:\System\CurrentControlSet\Services\ddpsvc\Settings -Name DeepGCInterval -Type DWord -Value 0xFFFFFFFF
    Set-ItemProperty -Path HKLM:\CLUSTER\Dedup -Name DeepGCInterval -Type DWord -Value 0xFFFFFFFF
    ```

* Si Desduplicación de datos no se ejecuta en un clúster:
    ```PowerShell
    Set-ItemProperty -Path HKLM:\System\CurrentControlSet\Services\ddpsvc\Settings -Name DeepGCInterval -Type DWord -Value 0xFFFFFFFF
    ```

### <a name="available-system-wide-settings"></a><a id="modifying-dedup-system-settings-available-settings"></a>Configuración para todo el sistema disponible
<table>
    <thead>
        <tr>
            <th style="min-width:125px">Nombre del valor</th>
            <th>Definición</th>
            <th>Valores aceptados</th>
            <th>¿Por qué quiere cambiarlo?</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>WlmMemoryOverPercentThreshold</td>
            <td>Esta configuración permite que los trabajos utilicen más memoria que la que Desduplicación de datos considera que está disponible. Por ejemplo, un valor de 300 significaría que el trabajo tendría que usar tres veces la memoria asignada para cancelarse.</td>
            <td>Números enteros positivos (un valor de 300 significa 300 % o 3 veces)</td>
            <td>Si tiene otra tarea que se detendrá si Desduplicación de datos asume más memoria</td>
        </tr>
        <tr>
            <td>DeepGCInterval</td>
            <td>Esta opción configura el intervalo en el que trabajos de recolección normal de elementos no utilizados se convierten en <a href="advanced-settings.md#faq-full-v-regular-gc" data-raw-source="[full Garbage Collection jobs](advanced-settings.md#faq-full-v-regular-gc)">trabajos de recolección completa de elementos no utilizados</a>. Un valor de n significa que cada n<sup></sup> trabajos había un trabajo de recolección completa de elementos no utilizados. Tenga en cuenta que la recolección de elementos no utilizados completa siempre está deshabilitada (independientemente del valor del registro) de los volúmenes con el <a href="understand.md#usage-type-backup" data-raw-source="[Backup Usage Type](understand.md#usage-type-backup)">tipo de uso de copia de seguridad</a>. <code>Start-DedupJob -Type GarbageCollection -Full</code> se puede usar si se desea una recolección completa de elementos no utilizados en un volumen de copia de seguridad.</td>
            <td>Enteros (-1 indica deshabilitado)</td>
            <td>Consulte <a href="advanced-settings.md#faq-why-disable-full-gc" data-raw-source="[this frequently asked question](advanced-settings.md#faq-why-disable-full-gc)">esta pregunta frecuente</a></td>
        </tr>
    </tbody>
</table>

## <a name="frequently-asked-questions"></a><a id="faq"></a>Preguntas más frecuentes
<a id="faq-use-responsibly"></a>**He cambiado una configuración de desduplicación de datos y ahora los trabajos son lentos o no finalizan, o bien el rendimiento de la carga de trabajo ha disminuido. ¿Por qué?**
Esta configuración le permiten controlar cómo se ejecuta Desduplicación de datos. Úselos de forma responsable y [supervise el rendimiento](run.md#monitoring-dedup).

<a id="faq-running-dedup-jobs-manually"></a>**Deseo ejecutar un trabajo de desduplicación de datos ahora mismo, pero no deseo crear una nueva programación. ¿puedo hacerlo?**
Sí, [todos los trabajos se pueden ejecutar manualmente](run.md#running-dedup-jobs-manually).

<a id="faq-full-v-regular-gc"></a>**¿Cuál es la diferencia entre la recolección de elementos no utilizados completa y normal?**
Hay dos tipos de [recolección de elementos no deseados](understand.md#job-info-gc):

- *Recolección normal de elementos no utilizados*: utiliza un algoritmo estadístico para buscar grandes fragmentos sin referencia que cumplen unos criterios determinados (bajo en memoria e IOPs). La recolección normal de elementos no utilizados compacta un contenedor de almacenamiento de fragmentos solo si un porcentaje mínimo de los fragmentos están sin referencia. Este tipo de recolección de elementos no utilizados se ejecuta más rápido y consume menos recursos que la recolección completa de elementos no utilizados. La programación predeterminada del trabajo de recolección normal de elementos no utilizados es ejecutarlo una vez por semana.
- La *recolección completa de elementos no utilizados* hace un trabajo mucho más profundo de búsqueda de fragmentos sin referencia y de liberación de más espacio en disco. La recolección completa de elementos no utilizados compacta cada contenedor, incluso si solo un único fragmento del contenedor no tiene referencia. La recolección de elementos no utilizados completa también liberará espacio que haya estado en uso si se produjo un error de alimentación o un bloqueo durante un trabajo de optimización. Los trabajos de recolección completa de elementos no utilizados recuperarán el 100 por ciento del espacio disponible que se puede recuperar en un volumen desduplicado a costa de requerir más tiempo y recursos del sistema en comparación con un trabajo de recolección normal de elementos no utilizados. El trabajo de recolección completa de elementos no utilizados normalmente encontrará y liberará hasta un 5 por ciento más de datos sin referencia que un trabajo de recolección normal de elementos no utilizados. La programación predeterminada del trabajo de recolección completa de elementos no utilizados consiste en ejecutarse cada cuarta vez que está programada la recolección de elementos no utilizados.

<a id="faq-why-disable-full-gc"></a>**¿Por qué deseo deshabilitar la recolección de elementos no utilizados completa?**
- La recolección de elementos no utilizados podría afectar negativamente a las instantáneas de duración del volumen y al tamaño de la copia de seguridad incremental. Las cargas de trabajo con una E/S intensiva o con una elevada renovación de código pueden ver una degradación del rendimiento en los trabajos de recolección completa de elementos no utilizados.
- Puede ejecutar manualmente un trabajo de recolección completa de elementos no utilizados desde PowerShell para limpiar las fugas si sabe que se bloqueó el sistema.
