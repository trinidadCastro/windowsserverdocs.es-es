---
title: Historial de rendimiento para servidores
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7ed910aba376ba7f78c628d7f47bdd21b366459d
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474742"
---
# <a name="performance-history-for-servers"></a>Historial de rendimiento para servidores

> Se aplica a: Windows Server 2019

Este subtema del [historial de rendimiento de espacios de almacenamiento directo](performance-history.md) describe en detalle el historial de rendimiento recopilado para los servidores. El historial de rendimiento está disponible para todos los servidores del clúster.

   > [!NOTE]
   > No se puede recopilar el historial de rendimiento de un servidor que está inactivo. La recopilación se reanudará automáticamente cuando el servidor vuelva a realizar la copia de seguridad.

## <a name="series-names-and-units"></a>Nombres de series y unidades

Estas series se recopilan para cada servidor válido:

| Serie                           | Unidad    |
|----------------------------------|---------|
| `clusternode.cpu.usage`          | percent |
| `clusternode.cpu.usage.guest`    | percent |
| `clusternode.cpu.usage.host`     | percent |
| `clusternode.memory.total`       | bytes   |
| `clusternode.memory.available`   | bytes   |
| `clusternode.memory.usage`       | bytes   |
| `clusternode.memory.usage.guest` | bytes   |
| `clusternode.memory.usage.host`  | bytes   |

Además, las series de unidades como `physicaldisk.size.total` se agregan a todas las unidades válidas conectadas al servidor, y las series de adaptadores de red como `networkadapter.bytes.total` se agregan para todos los adaptadores de red válidos conectados al servidor.

## <a name="how-to-interpret"></a>Cómo interpretar

| Serie                           | Cómo interpretar                                                      |
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

Las `cpu.*` series se recopilan de diferentes contadores de rendimiento en función de si Hyper-V está habilitado o no.

Si Hyper-V está habilitado:

| Serie                           | Contador de origen |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Hyper-V Hypervisor Logical Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.guest`    | `Hyper-V Hypervisor Virtual Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.host`     | `Hyper-V Hypervisor Root Virtual Processor` > `_Total` > `% Total Run Time` |

El uso de los `% Total Run Time` contadores garantiza que todo el uso de los atributos del historial de rendimiento.

Si Hyper-V no está habilitado:

| Serie                           | Contador de origen |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Processor` > `_Total` > `% Processor Time` |
| `clusternode.cpu.usage.guest`    | *nulo* |
| `clusternode.cpu.usage.host`     | *igual que el uso total* |

Sin perjuicio de la sincronización imperfecta, `clusternode.cpu.usage` siempre es `clusternode.cpu.usage.host` más `clusternode.cpu.usage.guest` .

Con la misma salvedad, `clusternode.cpu.usage.guest` siempre es la suma de `vm.cpu.usage` para todas las máquinas virtuales del servidor host.

La `memory.*` serie está disponible (próximamente).

  > [!NOTE]
  > Los contadores se miden en todo el intervalo, no muestreado. Por ejemplo, si el servidor está inactivo durante 9 segundos pero picos al 100% de CPU en el décimo segundo, se `clusternode.cpu.usage` registrará como un 10% de media durante este intervalo de 10 segundos. Esto garantiza que el historial de rendimiento Capture todas las actividades y sea sólido para el ruido.

## <a name="usage-in-powershell"></a>Uso en PowerShell

Use el cmdlet [Get-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode) :

```PowerShell
Get-ClusterNode <Name> | Get-ClusterPerf
```

## <a name="additional-references"></a>Referencias adicionales

- [Historial de rendimiento de Espacios de almacenamiento directo](performance-history.md)
