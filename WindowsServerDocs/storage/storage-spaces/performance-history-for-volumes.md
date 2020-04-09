---
title: Historial de rendimiento de los volúmenes
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 5f6acf062d2dba7c2a1a04d8a3f7cb4d7bd51a4d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856138"
---
# <a name="performance-history-for-volumes"></a>Historial de rendimiento de los volúmenes

> Se aplica a: Windows Server 2019

Este subtema del [historial de rendimiento de espacios de almacenamiento directo](performance-history.md) describe en detalle el historial de rendimiento recopilado para los volúmenes. El historial de rendimiento está disponible para cada Volumen compartido de clúster (CSV) del clúster. Sin embargo, no está disponible para los volúmenes de arranque del sistema operativo ni para ningún otro almacenamiento no CSV.

   > [!NOTE]
   > La recopilación puede tardar varios minutos en iniciarse para los volúmenes recién creados o cuyo nombre se ha cambiado.

## <a name="series-names-and-units"></a>Nombres de series y unidades

Estas series se recopilan para cada volumen válido:

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
| `volume.size.total`       | bytes            |
| `volume.size.available`   | bytes            |

## <a name="how-to-interpret"></a>Cómo interpretar

| Serie                    | Cómo interpretar                                                              |
|---------------------------|-------------------------------------------------------------------------------|
| `volume.iops.read`        | Número de operaciones de lectura por segundo completadas por este volumen.                |
| `volume.iops.write`       | Número de operaciones de escritura por segundo completadas por este volumen.               |
| `volume.iops.total`       | Número total de operaciones de lectura o escritura por segundo completadas por este volumen. |
| `volume.throughput.read`  | Cantidad de datos leídos de este volumen por segundo.                            |
| `volume.throughput.write` | Cantidad de datos escritos en este volumen por segundo.                           |
| `volume.throughput.total` | Cantidad total de datos leídos o escritos en este volumen por segundo.        |
| `volume.latency.read`     | Promedio de latencia de las operaciones de lectura de este volumen.                          |
| `volume.latency.write`    | Promedio de latencia de las operaciones de escritura en este volumen.                           |
| `volume.latency.average`  | Latencia media de todas las operaciones a o desde este volumen.                     |
| `volume.size.total`       | La capacidad total de almacenamiento del volumen.                                     |
| `volume.size.available`   | La capacidad de almacenamiento disponible del volumen.                                 |

## <a name="where-they-come-from"></a>Desde dónde provienen

Las series `iops.*`, `throughput.*`y `latency.*` se recopilan del conjunto de contadores de rendimiento `Cluster CSVFS`. Cada servidor del clúster tiene una instancia para cada volumen CSV, independientemente de la propiedad. El historial de rendimiento registrado para el volumen `MyVolume` es el agregado de las instancias de `MyVolume` en cada servidor del clúster.

| Serie                    | Contador de origen         |
|---------------------------|------------------------|
| `volume.iops.read`        | `Reads/sec`            |
| `volume.iops.write`       | `Writes/sec`           |
| `volume.iops.total`       | *suma de lo anterior*     |
| `volume.throughput.read`  | `Read bytes/sec`       |
| `volume.throughput.write` | `Write bytes/sec`      |
| `volume.throughput.total` | *suma de lo anterior*     |
| `volume.latency.read`     | `Avg. sec/Read`        |
| `volume.latency.write`    | `Avg. sec/Write`       |
| `volume.latency.average`  | *promedio de lo anterior* |

   > [!NOTE]
   > Los contadores se miden en todo el intervalo, no muestreado. Por ejemplo, si el volumen está inactivo durante 9 segundos pero completa 30 e/s en el décimo, su `volume.iops.total` se registrará como 3 e/s por segundo en promedio durante este intervalo de 10 segundos. Esto garantiza que el historial de rendimiento Capture todas las actividades y sea sólido para el ruido.

   > [!TIP]
   > Estos son los mismos contadores utilizados por el popular marco de pruebas comparativas de la [máquina virtual](https://github.com/Microsoft/diskspd/blob/master/Frameworks/VMFleet/watch-cluster.ps1) .

La serie `size.*` se recopilan de la clase `MSFT_Volume` en WMI, una instancia por volumen.

| Serie                    | Source (propiedad) |
|---------------------------|-----------------|
| `volume.size.total`       | `Size`          |
| `volume.size.available`   | `SizeRemaining` |

## <a name="usage-in-powershell"></a>Uso en PowerShell

Use el cmdlet [Get-Volume](https://docs.microsoft.com/powershell/module/storage/get-volume) :

```PowerShell
Get-Volume -FriendlyName <FriendlyName> | Get-ClusterPerf
```

## <a name="see-also"></a>Vea también

- [Historial de rendimiento de Espacios de almacenamiento directo](performance-history.md)
