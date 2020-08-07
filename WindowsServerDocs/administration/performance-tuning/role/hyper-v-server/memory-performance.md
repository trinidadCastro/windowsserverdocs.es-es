---
title: Rendimiento de la memoria de Hyper-V
description: Consideraciones de memoria en el ajuste del rendimiento en Hyper-V
ms.topic: article
ms.author: asmahi; sandysp; jopoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 08ccc5c8a6b7300f1fa476c01838080b0b01f67a
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896097"
---
# <a name="hyper-v-memory-performance"></a>Rendimiento de la memoria de Hyper-V


El hipervisor virtualiza la memoria física invitada para aislar las máquinas virtuales entre sí y proporcionar un espacio de memoria de base cero contiguo para cada sistema operativo invitado, al igual que en los sistemas no virtualizados.

## <a name="correct-memory-sizing-for-child-partitions"></a>Corregir el tamaño de la memoria para las particiones secundarias

Debe cambiar el tamaño de la memoria de la máquina virtual como lo haría normalmente para las aplicaciones de servidor en un equipo físico. Debe cambiar su tamaño para controlar razonablemente la carga esperada en horas normales y máximas, ya que la memoria insuficiente puede aumentar significativamente los tiempos de respuesta y el uso de la CPU o de e/s.

Puede habilitar Memoria dinámica para permitir que Windows cambie la memoria de la máquina virtual de forma dinámica. Con Memoria dinámica, si las aplicaciones de la máquina virtual experimentan problemas al realizar grandes asignaciones de memoria repentinas, puede aumentar el tamaño del archivo de paginación de la máquina virtual para garantizar la copia de seguridad temporal mientras Memoria dinámica responde a la presión de memoria.

Para obtener más información sobre Memoria dinámica, consulte [la introducción a Hyper-v memoria dinámica]( https://go.microsoft.com/fwlink/?linkid=834434) y [la guía de configuración de hyper-v memoria dinámica](https://go.microsoft.com/fwlink/?linkid=834435).

Al ejecutar Windows en la partición secundaria, puede usar los siguientes contadores de rendimiento en una partición secundaria para identificar si la partición secundaria está experimentando presión de memoria y es probable que funcione mejor con un mayor tamaño de memoria de la máquina virtual.

| Contador de rendimiento                                                         | Valor de umbral sugerido                                                                                                                                                           |
|-----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Memoria: bytes de reserva de caché en espera                                        | La suma de los bytes de reserva de caché en espera y los bytes de lista de páginas libres y cero deben ser de 200 MB o más en sistemas con 1 GB y 300 MB o más en sistemas con 2 GB o más de RAM visible. |
| Memoria: bytes de lista de & de páginas cero libres                                        | La suma de los bytes de reserva de caché en espera y los bytes de lista de páginas libres y cero deben ser de 200 MB o más en sistemas con 1 GB y 300 MB o más en sistemas con 2 GB o más de RAM visible. |
| Memoria: entrada de páginas/seg.                                                    | El promedio de un período de 1 hora es inferior a 10.                                                                                                                                       | 

## <a name="correct-memory-sizing-for-root-partition"></a>Corregir el tamaño de la memoria para la partición raíz

La partición raíz debe tener suficiente memoria para proporcionar servicios como la virtualización de e/s, la instantánea de máquina virtual y la administración para admitir las particiones secundarias.

Hyper-V en Windows Server 2016 supervisa el estado de tiempo de ejecución del sistema operativo de administración de la partición raíz para determinar cuánta memoria se puede asignar con seguridad a las particiones secundarias, al tiempo que se garantiza un alto rendimiento y confiabilidad de la partición raíz.

## <a name="additional-references"></a>Referencias adicionales

-   [Terminología de Hyper-V](terminology.md)

-   [Arquitectura de Hyper-V](architecture.md)

-   [Configuración del servidor de Hyper-V](configuration.md)

-   [Rendimiento del procesador de Hyper-V](processor-performance.md)

-   [Rendimiento de E/S de almacenamiento de Hyper-V](storage-io-performance.md)

-   [Rendimiento de E/S de red de Hyper-V](network-io-performance.md)

-   [Detección de cuellos de botella en un entorno virtualizado.](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuales Linux](linux-virtual-machine-considerations.md)
