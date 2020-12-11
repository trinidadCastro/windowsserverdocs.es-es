---
description: 'Más información acerca de: historial de rendimiento de las unidades'
title: Historial de rendimiento de las unidades
ms.author: cosdar
manager: eldenc
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
ms.localizationpriority: medium
ms.openlocfilehash: c3bb438f64bcb0ddd42abcc1f8b5fa22b5a16848
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97041903"
---
# <a name="performance-history-for-drives"></a>Historial de rendimiento de las unidades

> Se aplica a: Windows Server 2019

Este subtema del [historial de rendimiento de espacios de almacenamiento directo](performance-history.md) describe en detalle el historial de rendimiento recopilado para las unidades. El historial de rendimiento está disponible para todas las unidades del subsistema de almacenamiento del clúster, independientemente del bus o tipo de medio. Sin embargo, no está disponible para las unidades de arranque del sistema operativo.

   > [!NOTE]
   > No se puede recopilar el historial de rendimiento de las unidades de un servidor que está inactivo. La recopilación se reanudará automáticamente cuando el servidor vuelva a realizar la copia de seguridad.

## <a name="series-names-and-units"></a>Nombres de series y unidades

Estas series se recopilan para cada unidad válida:

| Serie                          | Unidad             |
|---------------------------------|------------------|
| `physicaldisk.iops.read`        | por segundo       |
| `physicaldisk.iops.write`       | por segundo       |
| `physicaldisk.iops.total`       | por segundo       |
| `physicaldisk.throughput.read`  | bytes por segundo |
| `physicaldisk.throughput.write` | bytes por segundo |
| `physicaldisk.throughput.total` | bytes por segundo |
| `physicaldisk.latency.read`     | segundos          |
| `physicaldisk.latency.write`    | segundos          |
| `physicaldisk.latency.average`  | segundos          |
| `physicaldisk.size.total`       | bytes            |
| `physicaldisk.size.used`        | bytes            |

## <a name="how-to-interpret"></a>Cómo interpretar

| Serie                          | Cómo interpretar                                                            |
|---------------------------------|-----------------------------------------------------------------------------|
| `physicaldisk.iops.read`        | Número de operaciones de lectura por segundo completadas por la unidad.                |
| `physicaldisk.iops.write`       | Número de operaciones de escritura por segundo completadas por la unidad.               |
| `physicaldisk.iops.total`       | Número total de operaciones de lectura o escritura por segundo completadas por la unidad. |
| `physicaldisk.throughput.read`  | Cantidad de datos leídos de la unidad por segundo.                            |
| `physicaldisk.throughput.write` | Cantidad de datos escritos en la unidad por segundo.                           |
| `physicaldisk.throughput.total` | Cantidad total de datos leídos o escritos en la unidad por segundo.        |
| `physicaldisk.latency.read`     | Promedio de latencia de las operaciones de lectura de la unidad.                          |
| `physicaldisk.latency.write`    | Promedio de latencia de las operaciones de escritura en la unidad.                           |
| `physicaldisk.latency.average`  | Latencia media de todas las operaciones a o desde la unidad.                     |
| `physicaldisk.size.total`       | La capacidad total de almacenamiento de la unidad.                                    |
| `physicaldisk.size.used`        | La capacidad de almacenamiento usada de la unidad.                                     |

## <a name="where-they-come-from"></a>Desde dónde provienen

Las `iops.*` `throughput.*` series, y `latency.*` se recopilan del `Physical Disk` contador de rendimiento establecido en el servidor al que está conectada la unidad, una instancia por unidad. Estos contadores se miden por `partmgr.sys` y no incluyen gran parte de la pila de software de Windows ni de ningún salto de red. Son representativos del rendimiento del hardware del dispositivo.

| Serie                          | Contador de origen           |
|---------------------------------|--------------------------|
| `physicaldisk.iops.read`        | `Disk Reads/sec`         |
| `physicaldisk.iops.write`       | `Disk Writes/sec`        |
| `physicaldisk.iops.total`       | `Disk Transfers/sec`     |
| `physicaldisk.throughput.read`  | `Disk Read Bytes/sec`    |
| `physicaldisk.throughput.write` | `Disk Write Bytes/sec`   |
| `physicaldisk.throughput.total` | `Disk Bytes/sec`         |
| `physicaldisk.latency.read`     | `Avg. Disk sec/Read`     |
| `physicaldisk.latency.write`    | `Avg. Disk sec/Writes`   |
| `physicaldisk.latency.average`  | `Avg. Disk sec/Transfer` |

   > [!NOTE]
   > Los contadores se miden en todo el intervalo, no muestreado. Por ejemplo, si la unidad está inactiva durante 9 segundos pero completa 30 e/s en el décimo segundo, su `physicaldisk.iops.total` se registrará como 3 e/s por segundo como promedio durante este intervalo de 10 segundos. Esto garantiza que el historial de rendimiento Capture todas las actividades y sea sólido para el ruido.

La `size.*` serie se recopila de la `MSFT_PhysicalDisk` clase en WMI, una instancia por unidad.

| Serie                          | Source, propiedad        |
|---------------------------------|------------------------|
| `physicaldisk.size.total`       | `Size`                 |
| `physicaldisk.size.used`        | `VirtualDiskFootprint` |

## <a name="usage-in-powershell"></a>Uso en PowerShell

Use el cmdlet [Get-PhysicalDisk](/powershell/module/storage/get-physicaldisk) :

```PowerShell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Get-ClusterPerf
```

## <a name="additional-references"></a>Referencias adicionales

- [Historial de rendimiento de Espacios de almacenamiento directo](performance-history.md)
