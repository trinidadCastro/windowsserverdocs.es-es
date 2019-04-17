---
title: Historial de rendimiento para volúmenes
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: fea1d3d67ab96d95b1699e8ac0129dba698477fe
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "1589711"
---
# <a name="performance-history-for-volumes"></a>Historial de rendimiento para volúmenes

> Se aplica a: Vista previa de Windows Server información confidencial

En este tema subcaracterística de [historial de rendimiento de almacenamiento espacios directa](performance-history.md) se describe detalladamente el historial de rendimiento recopilado para volúmenes. Historial de rendimiento está disponible para cada clúster compartidos por volumen (CSV) en el clúster. Sin embargo, no está disponible para el sistema operativo de arranque volúmenes ni cualquier otro almacenamiento de información que no sean CSV.

   > [!NOTE]
   > Puede tardar varios minutos para la colección que se va a comenzar para volúmenes recién creados o cuyo nombre ha cambiado.

## <a name="series-names-and-units"></a>Unidades y nombres de las series

Estas series se recopilan para cada volumen optan:

| Serie                    | Unidad             |
|---------------------------|------------------|
| `volume.iops.read`        | por segundo       |
| `volume.iops.write`       | por segundo       |
| `volume.iops.total`       | por segundo       |
| `volume.throughput.read`  | bytes por segundo |
| `volume.throughput.write` | bytes por segundo |
| `volume.throughput.total` | bytes por segundo |
| `volume.latency.read`     | segundos          |
| `volume.latency.write`    | segundos          |
| `volume.latency.average`  | segundos          |
| `volume.size.total`       |  bytes            |
| `volume.size.available`   |  bytes            |

## <a name="how-to-interpret"></a>Cómo interpretar

| Serie                    | Cómo interpretar                                                              |
|---------------------------|-------------------------------------------------------------------------------|
| `volume.iops.read`        | Número de operaciones de lectura por segundo completado por este volumen.                |
| `volume.iops.write`       | Número de operaciones de escritura por segundo completado por este volumen.               |
| `volume.iops.total`       | Número total de lee o escribe operaciones por segundo completado por este volumen. |
| `volume.throughput.read`  | Cantidad de datos que se leen este volumen por segundo.                            |
| `volume.throughput.write` | Cantidad de datos escritos en este volumen por segundo.                           |
| `volume.throughput.total` | Cantidad total de datos leen o escriben en este volumen por segundo.        |
| `volume.latency.read`     | Promedio de latencia de las operaciones de lectura de este volumen.                          |
| `volume.latency.write`    | Promedio de latencia de las operaciones de escritura a este volumen.                           |
| `volume.latency.average`  | Promedio de latencia de todas las operaciones a o desde este volumen.                     |
| `volume.size.total`       | La capacidad de almacenamiento total del volumen.                                     |
| `volume.size.available`   | La capacidad de almacenamiento disponible del volumen.                                 |

## <a name="where-they-come-from"></a>Cuando procedan de

El `iops.*`, `throughput.*`, y `latency.*` serie se recopila desde la `Cluster CSVFS` conjunto de contadores de rendimiento. Cada servidor del clúster tiene una instancia para cada volumen CSV, independientemente de la propiedad. El historial de rendimiento registrados para el volumen `MyVolume` , es la suma de la `MyVolume` instancias en cada servidor del clúster.

| Serie                    | Contador de origen         |
|---------------------------|------------------------|
| `volume.iops.read`        | `Reads/sec`            |
| `volume.iops.write`       | `Writes/sec`           |
| `volume.iops.total`       | *suma de los anteriores*     |
| `volume.throughput.read`  | `Read bytes/sec`       |
| `volume.throughput.write` | `Write bytes/sec`      |
| `volume.throughput.total` | *suma de los anteriores*     |
| `volume.latency.read`     | `Avg. sec/Read`        |
| `volume.latency.write`    | `Avg. sec/Write`       |
| `volume.latency.average`  | *promedio de los anteriores* |

   > [!NOTE]
   > Los contadores miden a través de todo el intervalo, que no se muestra. Por ejemplo, si el volumen está inactivo para 9 segundos completa pero 30 IOs en la segunda 10, su `volume.iops.total` se registrará como 3 IOs por segundo por término medio durante este intervalo de 10 segundos. Esto garantiza su historial de rendimiento de captura todas las actividades y es sólida al ruido.

   > [!TIP]
   > Estos son los mismos contadores usados por el marco del banco de pruebas de [VM flota](https://github.com/Microsoft/diskspd/blob/master/Frameworks/VMFleet/watch-cluster.ps1) popular.

El `size.*` serie se recopila desde la `MSFT_Volume` clase en WMI, una instancia por volumen.

| Serie                    | Propiedad Source |
|---------------------------|-----------------|
| `volume.size.total`       | `Size`          |
| `volume.size.available`   | `SizeRemaining` |

## <a name="usage-in-powershell"></a>Uso de PowerShell

Use el cmdlet [Get-volumen](https://docs.microsoft.com/powershell/module/storage/get-volume) :

```PowerShell
Get-Volume -FriendlyName <FriendlyName> | Get-ClusterPerf
```

## <a name="see-also"></a>Consulta también

- [Historial de rendimiento de almacenamiento espacios directa](performance-history.md)
