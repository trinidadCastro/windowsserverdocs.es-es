---
title: Historial de rendimiento de discos duros virtuales
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 7a0d8d132b6a5ff42cbe78a22c67dd9fec397184
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "1589728"
---
# <a name="performance-history-for-virtual-hard-disks"></a>Historial de rendimiento de discos duros virtuales

> Se aplica a: Vista previa de Windows Server información confidencial

En este tema subcaracterística de [historial de rendimiento de almacenamiento espacios directa](performance-history.md) se describe detalladamente el historial de rendimiento recopilado para los archivos de disco duro virtual (VHD). Historial de rendimiento está disponible para todos los VHD conectado a una máquina virtual que se está ejecutando, agrupada. Historial de rendimiento está disponible para los formatos VHD y VHDX, sin embargo, no está disponible para los archivos compartidos VHDX.

   > [!NOTE]
   > Puede tardar varios minutos para la colección que se va a comenzar para archivos VHD recién creados o que se ha movido.

## <a name="series-names-and-units"></a>Unidades y nombres de las series

Estas series se recopilan para cada disco duro virtual de optan:

| Serie                    | Unidad             |
|---------------------------|------------------|
| `vhd.iops.read`           | por segundo       |
| `vhd.iops.write`          | por segundo       |
| `vhd.iops.total`          | por segundo       |
| `vhd.throughput.read`     | bytes por segundo |
| `vhd.throughput.write`    | bytes por segundo |
| `vhd.throughput.total`    | bytes por segundo |
| `vhd.latency.average`     | segundos          |
| `vhd.size.current`        |  bytes            |
| `vhd.size.maximum`        |  bytes            |

## <a name="how-to-interpret"></a>Cómo interpretar

| Serie                    | Cómo interpretar                                                                                                 |
|---------------------------|------------------------------------------------------------------------------------------------------------------|
| `vhd.iops.read`           | Número de operaciones de lectura por segundo completado por el disco duro virtual.                                         |
| `vhd.iops.write`          | Número de operaciones de escritura por segundo completado por el disco duro virtual.                                        |
| `vhd.iops.total`          | Número total de lee o escribe operaciones por segundo completado por el disco duro virtual.                          |
| `vhd.throughput.read`     | Cantidad de datos leídos desde el disco duro virtual por segundo.                                                     |
| `vhd.throughput.write`    | Cantidad de datos escritos en el disco duro virtual por segundo.                                                    |
| `vhd.throughput.total`    | Cantidad total de datos leen o escriben en el disco duro virtual por segundo.                                 |
| `vhd.latency.average`     | Promedio de latencia de todas las operaciones a o desde el disco duro virtual.                                              |
| `vhd.size.current`        | El tamaño de archivo actual del disco duro virtual, si se está expandiendo dinámicamente. Si fija, no se recopilarán la serie. |
| `vhd.size.maximum`        | El tamaño máximo de disco duro virtual, si se está expandiendo dinámicamente. Si se fija, el es el tamaño.                  |

## <a name="where-they-come-from"></a>Cuando procedan de

El `iops.*`, `throughput.*`, y `latency.*` serie se recopila desde la `Hyper-V Virtual Storage Device` establece de contador de rendimiento en el servidor donde la máquina virtual es una instancia por VHD o VHDX, de ejecución.

| Serie                    | Contador de origen         |
|---------------------------|------------------------|
| `vhd.iops.read`           | `Read Operations/Sec`  |
| `vhd.iops.write`          | `Write Operations/Sec` |
| `vhd.iops.total`          | *suma de los anteriores*     |
| `vhd.throughput.read`     | `Read Bytes/sec`       |
| `vhd.throughput.write`    | `Write Bytes/sec`      |
| `vhd.throughput.total`    | *suma de los anteriores*     |
| `vhd.latency.average`     | `Latency`              |

   > [!NOTE]
   > Los contadores miden a través de todo el intervalo, que no se muestra. Por ejemplo, si el disco duro virtual está inactivo para 9 segundos completa pero 30 IOs en la segunda 10, su `vhd.iops.total` se registrará como 3 IOs por segundo por término medio durante este intervalo de 10 segundos. Esto garantiza su historial de rendimiento de captura todas las actividades y es sólida al ruido.

## <a name="usage-in-powershell"></a>Uso de PowerShell

Use el cmdlet [Get-VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd) :

```PowerShell
Get-VHD <Path> | Get-ClusterPerf
```

Para obtener la ruta de acceso de cada VHD desde la máquina virtual:

```PowerShell
(Get-VM <Name>).HardDrives | Select Path
```

   > [!NOTE]
   > El cmdlet Get-VHD requiere una ruta de acceso del archivo que se debe proporcionar. No se admite (enumeración).

## <a name="see-also"></a>Consulta también

- [Historial de rendimiento de almacenamiento espacios directa](performance-history.md)
