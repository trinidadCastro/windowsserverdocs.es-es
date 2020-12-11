---
description: Más información acerca de la ejecución de la desduplicación de datos
ms.assetid: f15c02d7-1cbd-4eba-a571-0ea34ab93ef4
title: Ejecución de desduplicación de datos
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/15/2016
ms.openlocfilehash: 55d5d3ee542ed0c16f66d9e882ae5d6dafd1af99
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97040353"
---
# <a name="running-data-deduplication"></a>Ejecución de desduplicación de datos

> Se aplica a: Windows Server (canal semianual), Windows Server 2016

## <a name="running-data-deduplication-jobs-manually"></a><a id="running-dedup-jobs-manually"></a>Ejecución de trabajos de Desduplicación de datos manualmente

Es posible ejecutar cada trabajo de Desduplicación de datos programado manualmente con los siguientes cmdlets de PowerShell:
* [`Start-DedupJob`](/previous-versions/system-center/system-center-2012-R2/hh758173(v=sc.12)): Inicia un nuevo trabajo de desduplicación de datos.
* [`Stop-DedupJob`](/previous-versions/system-center/system-center-2012-R2/hh758173(v=sc.12)): Detiene un trabajo de desduplicación de datos ya en curso (o lo quita de la cola)
* [`Get-DedupJob`](/previous-versions/system-center/system-center-2012-R2/hh758173(v=sc.12)): Muestra todos los trabajos de desduplicación de datos activos y en cola.

Todas las [opciones que están disponibles al programar un trabajo de Desduplicación de datos](advanced-settings.md#modifying-job-schedules-available-settings) también están disponibles cuando se inicia un trabajo manualmente, excepto los valores específicos de la programación. Por ejemplo, para iniciar un trabajo de [Optimización](understand.md#job-info-optimization) manualmente con alta prioridad y uso máximo de CPU y memoria, ejecute el siguiente comando de PowerShell con privilegios de administrador:

```PowerShell
Start-DedupJob -Type Optimization -Volume <Your-Volume-Here> -Memory 100 -Cores 100 -Priority High
```

## <a name="monitoring-data-deduplication"></a><a id="monitoring-dedup"></a>Supervisión de la desduplicación de datos

### <a name="job-successes"></a><a id="monitoring-dedup-job-successes"></a>Trabajos ejecutados correctamente

Como la desduplicación de datos emplea un modelo de posprocesamiento, es importante que los trabajos de [desduplicación de datos](understand.md#job-info) se realicen correctamente. Una manera fácil de comprobar el estado del trabajo más reciente es usar el cmdlet de [`Get-DedupStatus`](/previous-versions/system-center/system-center-2012-R2/hh758173(v=sc.12)) PowerShell. Compruebe periódicamente los campos siguientes:

* Para el [trabajo de optimización](understand.md#job-info-optimization), examine `LastOptimizationResult` (0 = correcto), `LastOptimizationResultMessage`, y `LastOptimizationTime` (debe ser reciente).
* Para el [trabajo de recolección de elementos no utilizados](understand.md#job-info-gc), examine `LastGarbageCollectionResult` (0 = correcto), `LastGarbageCollectionResultMessage`, y `LastGarbageCollectionTime` (debe ser reciente).
* Para el [trabajo de limpieza de integridad](understand.md#job-info-scrubbing), mire `LastScrubbingResult` (0 = correcto), `LastScrubbingResultMessage`, y `LastScrubbingTime` (debe ser reciente).

> [!Note]
> Encontrará información más detallada acerca de los aciertos y errores de los trabajos en el Visor de eventos de Windows en `\Applications and Services Logs\Windows\Deduplication\Operational`.

### <a name="optimization-rates"></a><a id="monitoring-dedup-optimization-rates"></a>Tasas de Optimización

Un indicador de error del [trabajo de Optimización](understand.md#job-info-optimization) es una tasa de optimización con tendencia a la baja que puede indicar que los trabajos de Optimización no se mantienen actualizados con la tasa de cambios, o renovación. Puede comprobar la tasa de optimización mediante el [`Get-DedupStatus`](/previous-versions/system-center/system-center-2012-R2/hh758173(v=sc.12)) cmdlet de PowerShell.

> [!Important]
> `Get-DedupStatus` tiene dos campos que son relevantes para la tasa de optimización: `OptimizedFilesSavingsRate` y `SavingsRate` . Ambos son valores importantes que hay que supervisar, pero cada uno tiene un significado único.
> - `OptimizedFilesSavingsRate` solo se aplica a los archivos que están en la Directiva para la optimización ( `space used by optimized files after optimization / logical size of optimized files` ).
> - `SavingsRate` se aplica a todo el volumen ( `space used by optimized files after optimization / total logical size of the optimization` ).

## <a name="disabling-data-deduplication"></a><a id="disabling-dedup"></a>Deshabilitación de la desduplicación de datos
Para desactivar la Desduplicación de datos, ejecute el [trabajo de Desoptimización](understand.md#job-info-unoptimization). Para deshacer la optimización del volumen, ejecute el siguiente comando:

```PowerShell
Start-DedupJob -Type Unoptimization -Volume <Desired-Volume>
```

> [!Important]
> El trabajo de Desoptimización dará error si el volumen no tiene espacio suficiente para contener los datos desoptimizados.

## <a name="frequently-asked-questions"></a><a id="faq"></a>Preguntas más frecuentes
**¿Existe un paquete de administración de System Center Operations Manager disponible para supervisar la desduplicación de datos?**
Sí. La Desduplicación de datos se puede supervisar mediante el paquete de administración de System Center para el servidor de archivos. Para obtener más información, consulte el documento [Guide for System Center Management Pack for File Server 2012 R2](https://download.microsoft.com/download/6/F/7/6F7A33B9-9383-48ED-9252-23C2C8AD1BDA/MPGuide_FileServer2012R2.doc) (Guía para el paquete de administración de System Center para el servidor de archivos 2012 R2).
