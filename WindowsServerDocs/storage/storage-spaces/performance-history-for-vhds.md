---
description: 'Más información acerca de: historial de rendimiento de los discos duros virtuales'
title: Historial de rendimiento de los discos duros virtuales
ms.author: cosdar
manager: eldenc
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: ff9990ce720330e63679b89ed63cbcc01fc2c57f
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97040063"
---
# <a name="performance-history-for-virtual-hard-disks"></a>Historial de rendimiento de los discos duros virtuales

> Se aplica a: Windows Server 2019

Este subtema del [historial de rendimiento de espacios de almacenamiento directo](performance-history.md) describe en detalle el historial de rendimiento recopilado para los archivos de disco duro virtual (VHD). El historial de rendimiento está disponible para todos los VHD conectados a una máquina virtual en clúster en ejecución. El historial de rendimiento está disponible para los formatos VHD y VHDX, pero no está disponible para los archivos VHDX compartidos.

   > [!NOTE]
   > La recopilación puede tardar varios minutos en iniciarse para los archivos VHD recién creados o migrados.

## <a name="series-names-and-units"></a>Nombres de series y unidades

Estas series se recopilan para todos los discos duros virtuales válidos:

| Serie                    | Unidad             |
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

| Serie                    | Cómo interpretar                                                                                                 |
|---------------------------|------------------------------------------------------------------------------------------------------------------|
| `vhd.iops.read`           | Número de operaciones de lectura por segundo completadas por el disco duro virtual.                                         |
| `vhd.iops.write`          | Número de operaciones de escritura por segundo completadas por el disco duro virtual.                                        |
| `vhd.iops.total`          | Número total de operaciones de lectura o escritura por segundo completadas por el disco duro virtual.                          |
| `vhd.throughput.read`     | Cantidad de datos leídos del disco duro virtual por segundo.                                                     |
| `vhd.throughput.write`    | Cantidad de datos escritos en el disco duro virtual por segundo.                                                    |
| `vhd.throughput.total`    | Cantidad total de datos leídos o escritos en el disco duro virtual por segundo.                                 |
| `vhd.latency.average`     | Latencia media de todas las operaciones hacia o desde el disco duro virtual.                                              |
| `vhd.size.current`        | Tamaño actual del archivo del disco duro virtual, si se expande dinámicamente. Si es Fixed, no se recopila la serie. |
| `vhd.size.maximum`        | El tamaño máximo del disco duro virtual, si se expande dinámicamente. Si es fijo, es el tamaño.                  |

## <a name="where-they-come-from"></a>Desde dónde provienen

Las `iops.*` `throughput.*` series, y `latency.*` se recopilan del `Hyper-V Virtual Storage Device` contador de rendimiento establecido en el servidor donde se ejecuta la máquina virtual, una instancia por VHD o VHDX.

| Serie                    | Contador de origen         |
|---------------------------|------------------------|
| `vhd.iops.read`           | `Read Operations/Sec`  |
| `vhd.iops.write`          | `Write Operations/Sec` |
| `vhd.iops.total`          | *suma de lo anterior*     |
| `vhd.throughput.read`     | `Read Bytes/sec`       |
| `vhd.throughput.write`    | `Write Bytes/sec`      |
| `vhd.throughput.total`    | *suma de lo anterior*     |
| `vhd.latency.average`     | `Latency`              |

   > [!NOTE]
   > Los contadores se miden en todo el intervalo, no muestreado. Por ejemplo, si el disco duro virtual está inactivo durante 9 segundos pero completa 30 IOs en el décimo segundo, se `vhd.iops.total` registrará como un promedio de 3 e/s por segundo durante este intervalo de 10 segundos. Esto garantiza que el historial de rendimiento Capture todas las actividades y sea sólido para el ruido.

## <a name="usage-in-powershell"></a>Uso en PowerShell

Use el cmdlet [Get-VHD](/powershell/module/hyper-v/get-vhd) :

```PowerShell
Get-VHD <Path> | Get-ClusterPerf
```

Para obtener la ruta de acceso de todos los VHD de la máquina virtual:

```PowerShell
(Get-VM <Name>).HardDrives | Select Path
```

   > [!NOTE]
   > El cmdlet Get-VHD requiere que se proporcione una ruta de acceso al archivo. No es compatible con la enumeración.

## <a name="additional-references"></a>Referencias adicionales

- [Historial de rendimiento de Espacios de almacenamiento directo](performance-history.md)
