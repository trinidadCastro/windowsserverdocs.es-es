---
title: Historial de rendimiento para los volúmenes
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: Espacios de almacenamiento directo
ms.localizationpriority: medium
ms.openlocfilehash: fea1d3d67ab96d95b1699e8ac0129dba698477fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882956"
---
# <a name="performance-history-for-volumes"></a>Historial de rendimiento para los volúmenes

> Se aplica a: Windows Server Insider Preview

Este subtema de [historial de rendimiento de espacios de almacenamiento directo](performance-history.md) describe detalladamente el historial de rendimiento recopilado para los volúmenes. Historial de rendimiento está disponible para cada clúster de volumen compartido (CSV) en el clúster. Sin embargo, no está disponible para el sistema operativo de arranque volúmenes ni cualquier otro tipo de almacenamiento distinto de CSV.

   > [!NOTE]
   > Puede tardar varios minutos de colección empezar a volúmenes recién creado o cuyo nombre ha cambiado.

## <a name="series-names-and-units"></a>Las unidades y los nombres de las series

Estas series se recopilan para cada volumen apto:

| serie                    | Unidad             |
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

| serie                    | Cómo interpretar                                                              |
|---------------------------|-------------------------------------------------------------------------------|
| `volume.iops.read`        | Número de operaciones de lectura por segundo realiza este volumen.                |
| `volume.iops.write`       | Número de operaciones de escritura por segundo realiza este volumen.               |
| `volume.iops.total`       | Número total de lee o escribe operaciones por segundo realiza este volumen. |
| `volume.throughput.read`  | Cantidad de datos leídos de este volumen por segundo.                            |
| `volume.throughput.write` | Cantidad de datos escritos en este volumen por segundo.                           |
| `volume.throughput.total` | Cantidad total de datos de lectura o escritura a este volumen por segundo.        |
| `volume.latency.read`     | Latencia media de las operaciones de lectura de este volumen.                          |
| `volume.latency.write`    | Latencia media de las operaciones de escritura a este volumen.                           |
| `volume.latency.average`  | Latencia media de todas las operaciones a o desde este volumen.                     |
| `volume.size.total`       | La capacidad de almacenamiento total del volumen.                                     |
| `volume.size.available`   | La capacidad de almacenamiento disponible del volumen.                                 |

## <a name="where-they-come-from"></a>Procedencia

El `iops.*`, `throughput.*`, y `latency.*` serie se recopila desde el `Cluster CSVFS` conjunto de contadores de rendimiento. Todos los servidores del clúster tiene una instancia para cada volumen CSV, independientemente de la propiedad. Registra el historial de rendimiento para el volumen `MyVolume` es el agregado de la `MyVolume` instancias en todos los servidores del clúster.

| serie                    | Contador de origen         |
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
   > Los contadores se miden en el intervalo de todo, que no se muestrean. Por ejemplo, si el volumen está inactivo por 9 segundos, pero se completa 30 IOs en la segunda de 10, su `volume.iops.total` se registrarán como 3 IOs por segundo promedio durante este intervalo de 10 segundos. Esto garantiza su historial de rendimiento captura toda la actividad y sólido al ruido.

   > [!TIP]
   > Estos son los mismos contadores usados por el popular [VM flota](https://github.com/Microsoft/diskspd/blob/master/Frameworks/VMFleet/watch-cluster.ps1) marco de pruebas comparativas.

El `size.*` serie se recopila desde el `MSFT_Volume` clase en WMI, una instancia por volumen.

| serie                    | Propiedad Source |
|---------------------------|-----------------|
| `volume.size.total`       | `Size`          |
| `volume.size.available`   | `SizeRemaining` |

## <a name="usage-in-powershell"></a>Uso en PowerShell

Use la [Get-Volume](https://docs.microsoft.com/powershell/module/storage/get-volume) cmdlet:

```PowerShell
Get-Volume -FriendlyName <FriendlyName> | Get-ClusterPerf
```

## <a name="see-also"></a>Vea también

- [Historial de rendimiento de espacios de almacenamiento directo](performance-history.md)
