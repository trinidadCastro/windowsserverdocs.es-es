---
title: Historial de rendimiento para servidores
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/0s/2018
Keywords: Espacios de almacenamiento directo
ms.localizationpriority: medium
ms.openlocfilehash: 33fd62376e9769c23fc6b00eefde9a9b95eb4650
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820596"
---
# <a name="performance-history-for-servers"></a>Historial de rendimiento para servidores

> Se aplica a: Windows Server Insider Preview

Este subtema de [historial de rendimiento de espacios de almacenamiento directo](performance-history.md) describe detalladamente el historial de rendimiento recopilado para los servidores. Historial de rendimiento está disponible para todos los servidores del clúster.

   > [!NOTE]
   > Historial de rendimiento no se pueden recopilar para un servidor que está inactivo. Colección reanudará automáticamente cuando el servidor vuelve a activarse.

## <a name="series-names-and-units"></a>Las unidades y los nombres de las series

Estas series se recopilan para cada servidor aptos:

| serie                           | Unidad    |
|----------------------------------|---------|
| `clusternode.cpu.usage`          | Por ciento |
| `clusternode.cpu.usage.guest`    | Por ciento |
| `clusternode.cpu.usage.host`     | Por ciento |
| `clusternode.memory.total`       | bytes   |
| `clusternode.memory.available`   | bytes   |
| `clusternode.memory.usage`       | bytes   |
| `clusternode.memory.usage.guest` | bytes   |
| `clusternode.memory.usage.host`  | bytes   |

Además, como la unidad serie `physicaldisk.size.total` se agregan para todas las unidades válidas que se adjunta al servidor y la serie de adaptador de red, como `networkadapter.bytes.total` se agregan para todos los adaptadores de red autorizados conectados al servidor.

## <a name="how-to-interpret"></a>Cómo interpretar

| serie                           | Cómo interpretar                                                      |
|----------------------------------|-----------------------------------------------------------------------|
| `clusternode.cpu.usage`          | Porcentaje de tiempo de procesador no inactivo.                        |
| `clusternode.cpu.usage.guest`    | Porcentaje de tiempo de procesador usado para la demanda de invitado (máquina virtual). |
| `clusternode.cpu.usage.host`     | Porcentaje de tiempo de procesador usado para la demanda de host.                    |
| `clusternode.memory.total`       | La memoria física total del servidor.                              |
| `clusternode.memory.available`   | La memoria disponible del servidor.                                   |
| `clusternode.memory.usage`       | La memoria asignada (no disponible) del servidor.                   |
| `clusternode.memory.usage.guest` | La memoria asignada a la demanda de invitado (máquina virtual).               |
| `clusternode.memory.usage.host`  | La memoria asignada a la demanda de host.                                  |

## <a name="where-they-come-from"></a>Procedencia

El `cpu.*` serie se recopila de los contadores de rendimiento diferente dependiendo de si Hyper-V está habilitada.

Si Hyper-V está habilitado:

| serie                           | Contador de origen |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Hyper-V Hypervisor Logical Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.guest`    | `Hyper-V Hypervisor Virtual Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.host`     | `Hyper-V Hypervisor Root Virtual Processor` > `_Total` > `% Total Run Time` |

Mediante el `% Total Run Time` contadores garantiza que todo el uso de los atributos del historial de rendimiento.

Si Hyper-V no está habilitado:

| serie                           | Contador de origen |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Processor` > `_Total` > `% Processor Time` |
| `clusternode.cpu.usage.guest`    | *zero* |
| `clusternode.cpu.usage.host`     | *igual que el uso total* |

Sin perjuicio de sincronización imperfecta, `clusternode.cpu.usage` siempre `clusternode.cpu.usage.host` plus `clusternode.cpu.usage.guest`.

Con la misma salvedad, `clusternode.cpu.usage.guest` siempre es la suma de `vm.cpu.usage` para todas las máquinas virtuales en el servidor host.

El `memory.*` series son (próximamente).

  > [!NOTE]
  > Los contadores se miden en el intervalo de todo, que no se muestrean. Por ejemplo, si el servidor está inactivo durante 9 segundos, pero los picos de CPU del 100% en la segunda de 10, su `clusternode.cpu.usage` se registrarán como 10% en promedio durante este intervalo de 10 segundos. Esto garantiza su historial de rendimiento captura toda la actividad y sólido al ruido.

## <a name="usage-in-powershell"></a>Uso en PowerShell

Use la [Get-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode) cmdlet:

```PowerShell
Get-ClusterNode <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>Vea también

- [Historial de rendimiento de espacios de almacenamiento directo](performance-history.md)
