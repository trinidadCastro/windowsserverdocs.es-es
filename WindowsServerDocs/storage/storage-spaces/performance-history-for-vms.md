---
title: Historial de rendimiento para las máquinas virtuales
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: Espacios de almacenamiento directo
ms.localizationpriority: medium
ms.openlocfilehash: f8072ab5fc853248f2eedd26019956ec864a891d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890876"
---
# <a name="performance-history-for-virtual-machines"></a>Historial de rendimiento para las máquinas virtuales

> Se aplica a: Windows Server Insider Preview

Este subtema de [historial de rendimiento de espacios de almacenamiento directo](performance-history.md) describe detalladamente el historial de rendimiento recopilado para las máquinas virtuales (VM). Historial de rendimiento está disponible para cada ejecución, máquina virtual en clúster.

   > [!NOTE]
   > Puede tardar varios minutos de colección empezar a para las máquinas virtuales recién creadas o cuyo nombre ha cambiado.

## <a name="series-names-and-units"></a>Las unidades y los nombres de las series

Estas series se recopilan para cada máquina virtual apto:

| serie                            | Unidad             |
|-----------------------------------|------------------|
| `vm.cpu.usage`                    | Por ciento          |
| `vm.memory.assigned`              | bytes            |
| `vm.memory.available`             | bytes            |
| `vm.memory.maximum`               | bytes            |
| `vm.memory.minimum`               | bytes            |
| `vm.memory.pressure`              | -                |
| `vm.memory.startup`               | bytes            |
| `vm.memory.total`                 | bytes            |
| `vmnetworkadapter.bandwidth.inbound`  | Bits por segundo |
| `vmnetworkadapter.bandwidth.outbound` | Bits por segundo |
| `vmnetworkadapter.bandwidth.total`    | Bits por segundo |

Además, todas las series de disco duro virtual (VHD), como `vhd.iops.total`, se agregan para cada VHD conectado a la máquina virtual.

## <a name="how-to-interpret"></a>Cómo interpretar


| serie                            | Descripción                                                                                                  |
|-----------------------------------|--------------------------------------------------------------------------------------------------------------|
| `vm.cpu.usage`                    | Porcentaje de la máquina virtual está usando de procesadores del servidor host.                                   |
| `vm.memory.assigned`              | La cantidad de memoria asignada a la máquina virtual.                                                      |
| `vm.memory.available`             | La cantidad de memoria que sigue estando disponible, la cantidad asignada.                                       |
| `vm.memory.maximum`               | Si utiliza la memoria dinámica, esta es la cantidad máxima de memoria que se puede asignar a la máquina virtual. |
| `vm.memory.minimum`               | Si utiliza la memoria dinámica, esta es la cantidad mínima de memoria que se puede asignar a la máquina virtual. |
| `vm.memory.pressure`              | La proporción de memoria exigido por la máquina virtual a través de la memoria asignada a la máquina virtual.            |
| `vm.memory.startup`               | La cantidad de memoria necesaria para que iniciar la máquina virtual.                                            |
| `vm.memory.total`                 | Total de memoria. |
| `vmnetworkadapter.bandwidth.inbound`  | Velocidad de los datos recibidos por la máquina virtual a través de todos sus adaptadores de red virtual.                        |
| `vmnetworkadapter.bandwidth.outbound` | Velocidad de datos enviados por la máquina virtual a través de todos sus adaptadores de red virtual.                            |
| `vmnetworkadapter.bandwidth.total`    | Tasa total de los datos recibidos o enviados por la máquina virtual a través de todos sus adaptadores de red virtual.          |

   > [!NOTE]
   > Los contadores se miden en el intervalo de todo, que no se muestrean. Por ejemplo, si la máquina virtual está inactiva durante los 9 segundos pero picos para usar el 50% de CPU del host en la segunda de 10, su `vm.cpu.usage` se registrarán como 5% por término medio durante este intervalo de 10 segundos. Esto garantiza su historial de rendimiento captura toda la actividad y sólido al ruido.

## <a name="usage-in-powershell"></a>Uso en PowerShell

Use la [Get-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm) cmdlet:

```PowerShell
Get-VM <Name> | Get-ClusterPerf
```

   > [!NOTE]
   > El cmdlet Get-VM devuelve solo las máquinas virtuales en el servidor local (o especificado), no en el clúster.

## <a name="see-also"></a>Vea también

- [Historial de rendimiento de espacios de almacenamiento directo](performance-history.md)
