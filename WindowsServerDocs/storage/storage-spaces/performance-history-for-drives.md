---
title: Historial de rendimiento para las unidades
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: d162275a885dac79e7efe749328ebdca471fcad1
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "1589762"
---
# <a name="performance-history-for-drives"></a>Historial de rendimiento para las unidades

> Se aplica a: Vista previa de Windows Server información confidencial

En este tema subcaracterística de [historial de rendimiento de almacenamiento espacios directa](performance-history.md) se describe detalladamente el historial de rendimiento recopilado para las unidades. Historial de rendimiento está disponible para cada unidad en el subsistema de almacenamiento de clúster, independientemente de bus o tipo de medio. Sin embargo, no está disponible para las unidades de arranque del sistema operativo.

   > [!NOTE]
   > No se pueden recopilar el historial de rendimiento para las unidades en un servidor que está inactivo. Colección reanudará automáticamente cuando se obtienen el servidor de copia de seguridad.

## <a name="series-names-and-units"></a>Unidades y nombres de las series

Estas series se recopilan para cada unidad optan:

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
| `physicaldisk.size.total`       |  bytes            |
| `physicaldisk.size.used`        |  bytes            |

## <a name="how-to-interpret"></a>Cómo interpretar

| Serie                          | Cómo interpretar                                                            |
|---------------------------------|-----------------------------------------------------------------------------|
| `physicaldisk.iops.read`        | Número de operaciones de lectura por segundo que se haya completado a la unidad.                |
| `physicaldisk.iops.write`       | Número de operaciones de escritura por segundo que se haya completado a la unidad.               |
| `physicaldisk.iops.total`       | Número total de lee o escribe operaciones por segundo que se haya completado a la unidad. |
| `physicaldisk.throughput.read`  | Cantidad de datos que se leen de la unidad por segundo.                            |
| `physicaldisk.throughput.write` | Cantidad de datos escritos en la unidad por segundo.                           |
| `physicaldisk.throughput.total` | Cantidad total de datos leen o escriben en el disco por segundo.        |
| `physicaldisk.latency.read`     | Promedio de latencia de las operaciones de lectura de la unidad.                          |
| `physicaldisk.latency.write`    | Promedio de latencia de operaciones de escritura en la unidad.                           |
| `physicaldisk.latency.average`  | Promedio de latencia de todas las operaciones a o desde la unidad.                     |
| `physicaldisk.size.total`       | La capacidad de almacenamiento total de la unidad.                                    |
| `physicaldisk.size.used`        | La capacidad de espacio de almacenamiento utilizado de la unidad.                                     |

## <a name="where-they-come-from"></a>Cuando procedan de

El `iops.*`, `throughput.*`, y `latency.*` serie se recopila desde la `Physical Disk` establecido en el servidor donde está conectada la unidad de contador de rendimiento, una instancia por cada unidad. Estos contadores miden por `partmgr.sys` y no se incluye parte de la pila de software de Windows ni los saltos de red. Son representante del rendimiento del hardware del dispositivo.

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
   > Los contadores miden a través de todo el intervalo, que no se muestra. Por ejemplo, si la unidad está inactiva para 9 segundos completa pero 30 IOs en la segunda 10, su `physicaldisk.iops.total` se registrará como 3 IOs por segundo por término medio durante este intervalo de 10 segundos. Esto garantiza su historial de rendimiento de captura todas las actividades y es sólida al ruido.

El `size.*` serie se recopila desde la `MSFT_PhysicalDisk` clase en WMI, una instancia por cada unidad.

| Serie                          | Propiedad Source        |
|---------------------------------|------------------------|
| `physicaldisk.size.total`       | `Size`                 |
| `physicaldisk.size.used`        | `VirtualDiskFootprint` |

## <a name="usage-in-powershell"></a>Uso de PowerShell

Use el cmdlet [Get-DiscoFísico](https://docs.microsoft.com/powershell/module/storage/get-physicaldisk) :

```PowerShell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Get-ClusterPerf
```

## <a name="see-also"></a>Consulta también

- [Historial de rendimiento de almacenamiento espacios directa](performance-history.md)
