---
title: Rendimiento de la memoria de Hyper-V
description: Consideraciones de memoria de Hyper-V de optimización del rendimiento
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 63a1b654b8ac52725cc5dd87c8b245f9dfaf40f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848076"
---
# <a name="hyper-v-memory-performance"></a>Rendimiento de la memoria de Hyper-V


El hipervisor virtualiza la memoria física del invitado para aislar las máquinas virtuales entre sí y para proporcionar un espacio de memoria contiguo y basado en cero para cada sistema operativo invitado, como en los sistemas no virtualizados.

## <a name="correct-memory-sizing-for-child-partitions"></a>Ajuste de tamaño de memoria correcta para las particiones secundarias

Debe cambiar el tamaño de memoria de máquina virtual como lo haría normalmente para aplicaciones de servidor en un equipo físico. Debe cambiar su tamaño para controlar razonablemente la carga esperada en períodos normales y las horas pico, porque no hay memoria suficiente puede aumentar significativamente los tiempos de respuesta y el uso de CPU o E/S.

Puede habilitar la memoria dinámica permitir que Windows cambiar dinámicamente el tamaño de memoria de máquina virtual. Con la memoria dinámica, si las aplicaciones en la máquina virtual teniendo problemas para realizar asignaciones de memoria repentino grande, puede aumentar el tamaño del archivo de paginación para la máquina virtual para asegurarse de respaldo temporal mientras la memoria dinámica responde a la presión de memoria.

Para obtener más información sobre la memoria dinámica, consulte [Hyper-V Dynamic Memory Overview]( https://go.microsoft.com/fwlink/?linkid=834434) y [Guía de configuración de memoria dinámica de Hyper-V](https://go.microsoft.com/fwlink/?linkid=834435).

Cuando se ejecuta Windows en la partición secundaria, puede usar los siguientes contadores de rendimiento dentro de una partición secundaria para identificar si la partición secundaria está experimentando la presión de memoria y es probable que funcionan mejor con un tamaño superior de memoria de máquina virtual.

| Contador de rendimiento                                                         | Valor de umbral sugerido                                                                                                                                                           |
|-----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Memoria: Bytes de reserva de caché en espera                                        | Suma de Bytes de reserva de caché en espera y libre y cero Bytes de lista de página debe ser 200 MB o más en sistemas con 1 GB y 300 MB o más en los sistemas con 2 GB de RAM o más visible. |
| Memoria: libre y cero Bytes de la lista de páginas                                        | Suma de Bytes de reserva de caché en espera y libre y cero Bytes de lista de página debe ser 200 MB o más en sistemas con 1 GB y 300 MB o más en los sistemas con 2 GB de RAM o más visible. |
| Memoria: entrada de páginas/seg.                                                    | Promedio durante un período de 1 hora es inferior a 10.                                                                                                                                       | 

## <a name="correct-memory-sizing-for-root-partition"></a>Ajuste de tamaño de memoria correcto para la partición raíz

La partición raíz debe tener suficiente memoria para prestar servicios como virtualización de E/S, instantánea de máquina virtual y administración para admitir las particiones secundarias.

Hyper-V en Windows Server 2016 supervisa el estado de tiempo de ejecución del sistema operativo de administración de la partición raíz para determinar cuánta memoria segura se puede asignar a las particiones secundarias, sin comprometer el alto rendimiento y confiabilidad de la partición raíz.

## <a name="see-also"></a>Vea también

-   [Terminología de Hyper-v.](terminology.md)

-   [Arquitectura de Hyper-V](architecture.md)

-   [Configuración del servidor de Hyper-V](configuration.md)

-   [Rendimiento del procesador de Hyper-V](processor-performance.md)

-   [Rendimiento de E/S de almacenamiento de Hyper-V](storage-io-performance.md)

-   [Rendimiento de E/S de red de Hyper-V](network-io-performance.md)

-   [Detección de cuellos de botella en un entorno virtualizado.](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuales Linux](linux-virtual-machine-considerations.md)
