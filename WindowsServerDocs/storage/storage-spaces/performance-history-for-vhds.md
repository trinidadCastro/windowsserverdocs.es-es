---
title: Historial de rendimiento para discos duros virtuales
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: Espacios de almacenamiento directo
ms.localizationpriority: medium
ms.openlocfilehash: 7a0d8d132b6a5ff42cbe78a22c67dd9fec397184
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880406"
---
# <a name="performance-history-for-virtual-hard-disks"></a>Historial de rendimiento para discos duros virtuales

> Se aplica a: Windows Server Insider Preview

Este subtema de [historial de rendimiento de espacios de almacenamiento directo](performance-history.md) describe detalladamente el historial de rendimiento recopilado para los archivos de disco duro virtual (VHD). Historial de rendimiento está disponible para cada disco duro virtual conectado a una máquina virtual en clúster, ejecución. Historial de rendimiento está disponible para los formatos VHD y VHDX, sin embargo, no está disponible para los archivos VHDX compartido.

   > [!NOTE]
   > Puede tardar varios minutos de colección para empezar a archivos de disco duro virtual recién creados o mover.

## <a name="series-names-and-units"></a>Las unidades y los nombres de las series

Estas series se recopilan para cada disco duro apto:

| serie                    | Unidad             |
|---------------------------|------------------|
| `vhd.iops.read`           | por segundo       |
| `vhd.iops.write`          | por segundo       |
| `vhd.iops.total`          | por segundo       |
| `vhd.throughput.read`     | bytes por segundo |
| `vhd.throughput.write`    | bytes por segundo |
| `vhd.throughput.total`    | bytes por segundo |
| `vhd.latency.average`     | segundos          |
| `vhd.size.current`        | bytes            |
| `vhd.size.maximum`        | bytes            |

## <a name="how-to-interpret"></a>Cómo interpretar

| serie                    | Cómo interpretar                                                                                                 |
|---------------------------|------------------------------------------------------------------------------------------------------------------|
| `vhd.iops.read`           | Número de operaciones de lectura por segundo que se completan mediante el disco duro virtual.                                         |
| `vhd.iops.write`          | Número de operaciones de escritura por segundo que se completan mediante el disco duro virtual.                                        |
| `vhd.iops.total`          | Número total de lee o escribe operaciones por segundo que se completan mediante el disco duro virtual.                          |
| `vhd.throughput.read`     | Cantidad de datos leídos desde el disco duro virtual por segundo.                                                     |
| `vhd.throughput.write`    | Cantidad de datos escritos en el disco duro virtual por segundo.                                                    |
| `vhd.throughput.total`    | Cantidad total de datos de lectura o escritura en el disco duro virtual por segundo.                                 |
| `vhd.latency.average`     | Latencia media de todas las operaciones a o desde el disco duro virtual.                                              |
| `vhd.size.current`        | El tamaño de archivo actual del disco duro virtual, si de expansión dinámica. Si se ha corregido, no se recopila la serie. |
| `vhd.size.maximum`        | El tamaño máximo del disco duro virtual, si de expansión dinámica. Si se ha corregido, la es el tamaño.                  |

## <a name="where-they-come-from"></a>Procedencia

El `iops.*`, `throughput.*`, y `latency.*` serie se recopila desde el `Hyper-V Virtual Storage Device` conjunto de contadores de rendimiento en el servidor donde la máquina virtual está ejecutando, una instancia por VHD o VHDX.

| serie                    | Contador de origen         |
|---------------------------|------------------------|
| `vhd.iops.read`           | `Read Operations/Sec`  |
| `vhd.iops.write`          | `Write Operations/Sec` |
| `vhd.iops.total`          | *suma de los anteriores*     |
| `vhd.throughput.read`     | `Read Bytes/sec`       |
| `vhd.throughput.write`    | `Write Bytes/sec`      |
| `vhd.throughput.total`    | *suma de los anteriores*     |
| `vhd.latency.average`     | `Latency`              |

   > [!NOTE]
   > Los contadores se miden en el intervalo de todo, que no se muestrean. Por ejemplo, si está inactivo para el disco duro virtual 9 segundos, pero se completa 30 IOs en la segunda de 10, su `vhd.iops.total` se registrarán como 3 IOs por segundo promedio durante este intervalo de 10 segundos. Esto garantiza su historial de rendimiento captura toda la actividad y sólido al ruido.

## <a name="usage-in-powershell"></a>Uso en PowerShell

Use la [Get-VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd) cmdlet:

```PowerShell
Get-VHD <Path> | Get-ClusterPerf
```

Para obtener la ruta de acceso de cada disco duro virtual de la máquina virtual:

```PowerShell
(Get-VM <Name>).HardDrives | Select Path
```

   > [!NOTE]
   > El cmdlet Get-VHD requiere una ruta de acceso del archivo que se proporcione. No admite la enumeración.

## <a name="see-also"></a>Vea también

- [Historial de rendimiento de espacios de almacenamiento directo](performance-history.md)
