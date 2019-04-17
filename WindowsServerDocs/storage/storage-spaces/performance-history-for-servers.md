---
title: Historial de rendimiento para servidores
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/0s/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 33fd62376e9769c23fc6b00eefde9a9b95eb4650
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "1894254"
---
# <a name="performance-history-for-servers"></a>Historial de rendimiento para servidores

> Se aplica a: Vista previa de Windows Server información confidencial

En este tema subcaracterística de [historial de rendimiento de almacenamiento espacios directa](performance-history.md) se describe detalladamente el historial de rendimiento recopilado para los servidores. Historial de rendimiento está disponible para todos los servidores del clúster.

   > [!NOTE]
   > No se pueden recopilar el historial de rendimiento para un servidor que está inactivo. Colección reanudará automáticamente cuando se obtienen el servidor de copia de seguridad.

## <a name="series-names-and-units"></a>Unidades y nombres de las series

Estas series se recopilan para cada servidor de aplicaciones:

| Serie                           | Unidad    |
|----------------------------------|---------|
| `clusternode.cpu.usage`          | por ciento |
| `clusternode.cpu.usage.guest`    | por ciento |
| `clusternode.cpu.usage.host`     | por ciento |
| `clusternode.memory.total`       |  bytes   |
| `clusternode.memory.available`   |  bytes   |
| `clusternode.memory.usage`       |  bytes   |
| `clusternode.memory.usage.guest` |  bytes   |
| `clusternode.memory.usage.host`  |  bytes   |

Además, como la unidad serie `physicaldisk.size.total` se agregan para todas las unidades optan conectadas con el servidor y la serie de adaptador de red como `networkadapter.bytes.total` se agregan para todos los adaptadores de red optan conectados al servidor.

## <a name="how-to-interpret"></a>Cómo interpretar

| Serie                           | Cómo interpretar                                                      |
|----------------------------------|-----------------------------------------------------------------------|
| `clusternode.cpu.usage`          | Porcentaje de tiempo de procesador que no está inactivo.                        |
| `clusternode.cpu.usage.guest`    | Porcentaje de tiempo de procesador utilizado para propuestas de invitado (máquina virtual). |
| `clusternode.cpu.usage.host`     | Porcentaje de tiempo de procesador utilizado para propuestas de host.                    |
| `clusternode.memory.total`       | La memoria física total del servidor.                              |
| `clusternode.memory.available`   | La memoria disponible del servidor.                                   |
| `clusternode.memory.usage`       | La memoria asignada (no disponible) del servidor.                   |
| `clusternode.memory.usage.guest` | La memoria asignada a la demanda de invitado (máquina virtual).               |
| `clusternode.memory.usage.host`  | La memoria asignada a la demanda de host.                                  |

## <a name="where-they-come-from"></a>Cuando procedan de

El `cpu.*` serie se recopila de los contadores de rendimiento diferente dependiendo de si está habilitado Hyper-V.

Si se habilita Hyper-V:

| Serie                           | Contador de origen |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Hyper-V Hypervisor Logical Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.guest`    | `Hyper-V Hypervisor Virtual Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.host`     | `Hyper-V Hypervisor Root Virtual Processor` > `_Total` > `% Total Run Time` |

Uso de la `% Total Run Time` contadores se asegura de que todo el uso de los atributos del historial de rendimiento.

Si no está habilitado Hyper-V:

| Serie                           | Contador de origen |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Processor` > `_Total` > `% Processor Time` |
| `clusternode.cpu.usage.guest`    | *cero* |
| `clusternode.cpu.usage.host`     | *igual que el uso total* |

A pesar de sincronización deficiente, `clusternode.cpu.usage` es siempre `clusternode.cpu.usage.host` plus `clusternode.cpu.usage.guest`.

Con la misma advertencia, `clusternode.cpu.usage.guest` siempre es la suma de `vm.cpu.usage` para todas las máquinas virtuales en el servidor host.

El `memory.*` serie es (próximamente).

  > [!NOTE]
  > Los contadores miden a través de todo el intervalo, que no se muestra. Por ejemplo, si el servidor está inactivo para 9 segundos pero los picos en la CPU del 100% en la segunda 10, su `clusternode.cpu.usage` se registrará como un 10% por término medio durante este intervalo de 10 segundos. Esto garantiza su historial de rendimiento de captura todas las actividades y es sólida al ruido.

## <a name="usage-in-powershell"></a>Uso de PowerShell

Use el cmdlet [Get-nodoClúster](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode) :

```PowerShell
Get-ClusterNode <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>Consulta también

- [Historial de rendimiento de almacenamiento espacios directa](performance-history.md)
