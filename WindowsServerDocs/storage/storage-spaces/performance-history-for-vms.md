---
title: Historial de rendimiento para las máquinas virtuales
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: f8072ab5fc853248f2eedd26019956ec864a891d
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239222"
---
# Historial de rendimiento para las máquinas virtuales

> Se aplica a: Windows Server Insider Preview

Este tema secundarias de [historial de rendimiento de espacios de almacenamiento directo](performance-history.md) se describe con detalle el historial de rendimiento recopilado para máquinas virtuales (VM). Historial de rendimiento está disponible para cada ejecutando, máquina virtual en clúster.

   > [!NOTE]
   > Puede tardar varios minutos de colección empezar a para las máquinas virtuales recién creadas o ha cambiado de nombre.

## Unidades y nombres de las series

Estas series se reciben para cada máquina virtual apto:

| Serie                            | Unidad             |
|-----------------------------------|------------------|
| `vm.cpu.usage`                    | por ciento          |
| `vm.memory.assigned`              |  bytes            |
| `vm.memory.available`             |  bytes            |
| `vm.memory.maximum`               |  bytes            |
| `vm.memory.minimum`               |  bytes            |
| `vm.memory.pressure`              | -                |
| `vm.memory.startup`               |  bytes            |
| `vm.memory.total`                 |  bytes            |
| `vmnetworkadapter.bandwidth.inbound`  | bits por segundo |
| `vmnetworkadapter.bandwidth.outbound` | bits por segundo |
| `vmnetworkadapter.bandwidth.total`    | bits por segundo |

Además, todas las series de disco duro virtual (VHD), como `vhd.iops.total`, se agregan para cada VHD conectado a la máquina virtual.

## Cómo interpretar


| Serie                            | Descripción                                                                                                  |
|-----------------------------------|--------------------------------------------------------------------------------------------------------------|
| `vm.cpu.usage`                    | Porcentaje de la máquina virtual está usando de procesadores del servidor de su host.                                   |
| `vm.memory.assigned`              | La cantidad de memoria asignada a la máquina virtual.                                                      |
| `vm.memory.available`             | La cantidad de memoria que sigue estando disponible, el importe asignado.                                       |
| `vm.memory.maximum`               | Si usas la memoria dinámica, esto es la cantidad máxima de memoria que se puede asignar a la máquina virtual. |
| `vm.memory.minimum`               | Si el uso de memoria dinámica, se trata la cantidad mínima de memoria que se puede asignar a la máquina virtual. |
| `vm.memory.pressure`              | La relación de memoria exigida por la máquina virtual a través de la memoria asignada a la máquina virtual.            |
| `vm.memory.startup`               | La cantidad de memoria necesaria para que la máquina virtual iniciar.                                            |
| `vm.memory.total`                 | Memoria total. |
| `vmnetworkadapter.bandwidth.inbound`  | Velocidad de datos recibidos por la máquina virtual en sus todos los adaptadores de red virtual.                        |
| `vmnetworkadapter.bandwidth.outbound` | Velocidad de datos enviados a través de la máquina virtual en sus todos los adaptadores de red virtual.                            |
| `vmnetworkadapter.bandwidth.total`    | Velocidad total de datos recibidos o enviados por la máquina virtual en sus todos los adaptadores de red virtual.          |

   > [!NOTE]
   > Contadores se miden a través de todo el intervalo, que no se muestrea. Por ejemplo, si la máquina virtual esté inactiva 9 segundos, pero los picos usar 50% del host CPU en el segundo 10, su `vm.cpu.usage` se registrará como 5% por término medio durante este intervalo de 10 segundos. Esto garantiza su historia de rendimiento captura toda la actividad y es eficaz al ruido.

## Uso de PowerShell

Usa el cmdlet [Get-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm) :

```PowerShell
Get-VM <Name> | Get-ClusterPerf
```

   > [!NOTE]
   > El cmdlet Get-VM devuelve solo las máquinas virtuales en el servidor local (o especificado), no a través del clúster.

## Ver también

- [Historial de rendimiento de espacios de almacenamiento directo](performance-history.md)
