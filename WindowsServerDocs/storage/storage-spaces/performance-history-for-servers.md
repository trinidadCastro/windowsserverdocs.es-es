---
title: Historial de rendimiento para servidores
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/05/2018
Keywords: Espacios de almacenamiento directo
ms.localizationpriority: medium
ms.openlocfilehash: bbfc92f7926b93f5f6716514e64672f4aa304c0f
ms.sourcegitcommit: e817a130c2ed9caaddd1def1b2edac0c798a6aa2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2019
ms.locfileid: "74945246"
---
# <a name="performance-history-for-servers"></a>Historial de rendimiento para servidores

> Se aplica a: Windows Server 2019

Este subtema del [historial de rendimiento de espacios de almacenamiento directo](performance-history.md) describe en detalle el historial de rendimiento recopilado para los servidores. El historial de rendimiento está disponible para todos los servidores del clúster.

   > [!NOTE]
   > No se puede recopilar el historial de rendimiento de un servidor que está inactivo. La recopilación se reanudará automáticamente cuando el servidor vuelva a realizar la copia de seguridad.

## <a name="series-names-and-units"></a>Nombres de series y unidades

Estas series se recopilan para cada servidor válido:

| Series                           | Unidad    |
|----------------------------------|---------|
| `clusternode.cpu.usage`          | percent |
| `clusternode.cpu.usage.guest`    | percent |
| `clusternode.cpu.usage.host`     | percent |
| `clusternode.memory.total`       | bytes   |
| `clusternode.memory.available`   | bytes   |
| `clusternode.memory.usage`       | bytes   |
| `clusternode.memory.usage.guest` | bytes   |
| `clusternode.memory.usage.host`  | bytes   |

Además, las series de unidades como `physicaldisk.size.total` se agregan a todas las unidades válidas conectadas al servidor, y las series de adaptadores de red como `networkadapter.bytes.total` se agregan a todos los adaptadores de red válidos conectados al servidor.

## <a name="how-to-interpret"></a>Cómo interpretar

| Series                           | Cómo interpretar                                                      |
|----------------------------------|-----------------------------------------------------------------------|
| `clusternode.cpu.usage`          | Porcentaje de tiempo de procesador que no está inactivo.                        |
| `clusternode.cpu.usage.guest`    | Porcentaje de tiempo de procesador usado para la demanda de invitado (máquina virtual). |
| `clusternode.cpu.usage.host`     | Porcentaje de tiempo de procesador usado para la demanda del host.                    |
| `clusternode.memory.total`       | La memoria física total del servidor.                              |
| `clusternode.memory.available`   | La memoria disponible del servidor.                                   |
| `clusternode.memory.usage`       | Memoria asignada (no disponible) del servidor.                   |
| `clusternode.memory.usage.guest` | La memoria asignada a la demanda de invitado (máquina virtual).               |
| `clusternode.memory.usage.host`  | Memoria asignada a la demanda del host.                                  |

## <a name="where-they-come-from"></a>Desde dónde provienen

La serie `cpu.*` se recopilan de diferentes contadores de rendimiento en función de si Hyper-V está habilitado o no.

Si Hyper-V está habilitado:

| Series                           | Contador de origen |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Hyper-V Hypervisor Logical Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.guest`    | `Hyper-V Hypervisor Virtual Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.host`     | `Hyper-V Hypervisor Root Virtual Processor` > `_Total` > `% Total Run Time` |

El uso de los contadores de `% Total Run Time` garantiza que el historial de rendimiento tiene todos los atributos.

Si Hyper-V no está habilitado:

| Series                           | Contador de origen |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Processor` > `_Total` > `% Processor Time` |
| `clusternode.cpu.usage.guest`    | *zero* |
| `clusternode.cpu.usage.host`     | *igual que el uso total* |

Sin perjuicio de la sincronización imperfecta, `clusternode.cpu.usage` siempre se `clusternode.cpu.usage.host` más `clusternode.cpu.usage.guest`.

Con la misma salvedad, `clusternode.cpu.usage.guest` siempre es la suma de `vm.cpu.usage` para todas las máquinas virtuales del servidor host.

La serie `memory.*` es (PRÓXIMAmente).

  > [!NOTE]
  > Los contadores se miden en todo el intervalo, no muestreado. Por ejemplo, si el servidor está inactivo durante 9 segundos pero picos al 100% de la CPU en el décimo segundo, su `clusternode.cpu.usage` se registrará como el 10% del promedio durante este intervalo de 10 segundos. Esto garantiza que el historial de rendimiento Capture todas las actividades y sea sólido para el ruido.

## <a name="usage-in-powershell"></a>Uso en PowerShell

Use el cmdlet [Get-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode) :

```PowerShell
Get-ClusterNode <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>Consulta también

- [Historial de rendimiento de Espacios de almacenamiento directo](performance-history.md)
