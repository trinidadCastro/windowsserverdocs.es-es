---
title: Historial de rendimiento de las unidades
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Espacios de almacenamiento directo
ms.localizationpriority: medium
ms.openlocfilehash: d162275a885dac79e7efe749328ebdca471fcad1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879196"
---
# <a name="performance-history-for-drives"></a>Historial de rendimiento de las unidades

> Se aplica a: Windows Server Insider Preview

Este subtema de [historial de rendimiento de espacios de almacenamiento directo](performance-history.md) describe detalladamente el historial de rendimiento recopilado para las unidades. Historial de rendimiento está disponible para cada unidad en el subsistema de almacenamiento de clúster, independientemente de bus o tipo de medio. Sin embargo, no está disponible para las unidades de arranque del sistema operativo.

   > [!NOTE]
   > Historial de rendimiento no se pueden recopilar para las unidades en un servidor que está inactivo. Colección reanudará automáticamente cuando el servidor vuelve a activarse.

## <a name="series-names-and-units"></a>Las unidades y los nombres de las series

Estas series se recopilan para cada unidad apto:

| serie                          | Unidad             |
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

| serie                          | Cómo interpretar                                                            |
|---------------------------------|-----------------------------------------------------------------------------|
| `physicaldisk.iops.read`        | Número de operaciones de lectura por segundo que se completan mediante la unidad.                |
| `physicaldisk.iops.write`       | Número de operaciones de escritura por segundo que se completan mediante la unidad.               |
| `physicaldisk.iops.total`       | Número total de lee o escribe operaciones por segundo que se completan mediante la unidad. |
| `physicaldisk.throughput.read`  | Cantidad de datos leídos de la unidad por segundo.                            |
| `physicaldisk.throughput.write` | Cantidad de datos escritos en el disco por segundo.                           |
| `physicaldisk.throughput.total` | Cantidad total de datos de lectura o escritura en el disco por segundo.        |
| `physicaldisk.latency.read`     | Latencia media de las operaciones de lectura de la unidad.                          |
| `physicaldisk.latency.write`    | Promedio de latencia de operaciones de escritura en la unidad.                           |
| `physicaldisk.latency.average`  | Latencia media de todas las operaciones a o desde la unidad.                     |
| `physicaldisk.size.total`       | La capacidad de almacenamiento total de la unidad.                                    |
| `physicaldisk.size.used`        | La capacidad de almacenamiento usado de la unidad.                                     |

## <a name="where-they-come-from"></a>Procedencia

El `iops.*`, `throughput.*`, y `latency.*` serie se recopila desde el `Physical Disk` establecido en el servidor donde se conecta la unidad de contador de rendimiento, una instancia por unidad. Estos contadores se miden por `partmgr.sys` e incluyen gran parte de la pila de software de Windows ni los saltos de red. Son representativos de rendimiento del hardware del dispositivo.

| serie                          | Contador de origen           |
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
   > Los contadores se miden en el intervalo de todo, que no se muestrean. Por ejemplo, si la unidad está inactiva por 9 segundos, pero se completa 30 IOs en la segunda de 10, su `physicaldisk.iops.total` se registrarán como 3 IOs por segundo promedio durante este intervalo de 10 segundos. Esto garantiza su historial de rendimiento captura toda la actividad y sólido al ruido.

El `size.*` serie se recopila desde el `MSFT_PhysicalDisk` clase en WMI, una instancia por unidad.

| serie                          | Propiedad Source        |
|---------------------------------|------------------------|
| `physicaldisk.size.total`       | `Size`                 |
| `physicaldisk.size.used`        | `VirtualDiskFootprint` |

## <a name="usage-in-powershell"></a>Uso en PowerShell

Use la [Get-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/get-physicaldisk) cmdlet:

```PowerShell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Get-ClusterPerf
```

## <a name="see-also"></a>Vea también

- [Historial de rendimiento de espacios de almacenamiento directo](performance-history.md)
