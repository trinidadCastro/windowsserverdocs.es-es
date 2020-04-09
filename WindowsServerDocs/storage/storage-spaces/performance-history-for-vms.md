---
title: Historial de rendimiento de las máquinas virtuales
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.openlocfilehash: aefc9c3c33cb93be241aae4ef18d815a9f8defef
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856148"
---
# <a name="performance-history-for-virtual-machines"></a>Historial de rendimiento de las máquinas virtuales

> Se aplica a: Windows Server 2019

Este subtema del [historial de rendimiento de espacios de almacenamiento directo](performance-history.md) describe en detalle el historial de rendimiento recopilado para máquinas virtuales (VM). El historial de rendimiento está disponible para todas las máquinas virtuales en clúster en ejecución.

   > [!NOTE]
   > La recopilación puede tardar varios minutos en iniciarse para las máquinas virtuales recién creadas o cuyo nombre ha cambiado.

## <a name="series-names-and-units"></a>Nombres de series y unidades

Estas series se recopilan para cada máquina virtual válida:

| Serie                            | Unidad             |
|-----------------------------------|------------------|
| `vm.cpu.usage`                    | percent          |
| `vm.memory.assigned`              | bytes            |
| `vm.memory.available`             | bytes            |
| `vm.memory.maximum`               | bytes            |
| `vm.memory.minimum`               | bytes            |
| `vm.memory.pressure`              | -                |
| `vm.memory.startup`               | bytes            |
| `vm.memory.total`                 | bytes            |
| `vmnetworkadapter.bandwidth.inbound`  | bits por segundo |
| `vmnetworkadapter.bandwidth.outbound` | bits por segundo |
| `vmnetworkadapter.bandwidth.total`    | bits por segundo |

Además, todas las series de discos duros virtuales (VHD), como `vhd.iops.total`, se agregan para todos los VHD conectados a la máquina virtual.

## <a name="how-to-interpret"></a>Cómo interpretar


| Serie                            | Descripción                                                                                                  |
|-----------------------------------|--------------------------------------------------------------------------------------------------------------|
| `vm.cpu.usage`                    | Porcentaje de uso de la máquina virtual de los procesadores del servidor host.                                   |
| `vm.memory.assigned`              | Cantidad de memoria asignada a la máquina virtual.                                                      |
| `vm.memory.available`             | Cantidad de memoria que permanece disponible, de la cantidad asignada.                                       |
| `vm.memory.maximum`               | Si utiliza memoria dinámica, esta es la cantidad máxima de memoria que se puede asignar a la máquina virtual. |
| `vm.memory.minimum`               | Si utiliza memoria dinámica, esta es la cantidad mínima de memoria que se puede asignar a la máquina virtual. |
| `vm.memory.pressure`              | La proporción de memoria exigida por la máquina virtual a través de la memoria asignada a la máquina virtual.            |
| `vm.memory.startup`               | Cantidad de memoria necesaria para que se inicie la máquina virtual.                                            |
| `vm.memory.total`                 | Memoria total. |
| `vmnetworkadapter.bandwidth.inbound`  | Velocidad de los datos recibidos por la máquina virtual en todos sus adaptadores de red virtuales.                        |
| `vmnetworkadapter.bandwidth.outbound` | Velocidad de datos enviados por la máquina virtual a través de todos sus adaptadores de red virtuales.                            |
| `vmnetworkadapter.bandwidth.total`    | Velocidad total de los datos recibidos o enviados por la máquina virtual en todos sus adaptadores de red virtuales.          |

   > [!NOTE]
   > Los contadores se miden en todo el intervalo, no muestreado. Por ejemplo, si la máquina virtual está inactiva durante 9 segundos pero picos de uso del 50% de la CPU del host en el décimo segundo, su `vm.cpu.usage` se registrará como el 5% del promedio durante este intervalo de 10 segundos. Esto garantiza que el historial de rendimiento Capture todas las actividades y sea sólido para el ruido.

## <a name="usage-in-powershell"></a>Uso en PowerShell

Use el cmdlet [Get-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm) :

```PowerShell
Get-VM <Name> | Get-ClusterPerf
```

   > [!NOTE]
   > El cmdlet Get-VM solo devuelve las máquinas virtuales en el servidor local (o especificado), no en el clúster.

## <a name="see-also"></a>Vea también

- [Historial de rendimiento de Espacios de almacenamiento directo](performance-history.md)
