---
title: Detección de cuellos de botella en un entorno virtualizado.
description: Cómo detectar y resolver posibles cuellos de botella de rendimiento de Hyper-v
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: cdad5f0cc3b0e49ae46e975e3acc2c48a18e5f70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867536"
---
# <a name="detecting-bottlenecks-in-a-virtualized-environment"></a>Detección de cuellos de botella en un entorno virtualizado.

En esta sección debería entregarle algunas sugerencias sobre qué supervisar con el Monitor de rendimiento y cómo identificar donde el problema podría ser cuando el host o algunas de las máquinas virtuales no funcionen como se esperaba.

## <a name="processor-bottlenecks"></a>Cuellos de botella de procesador

Estos son algunos escenarios comunes que podrían provocar cuellos de botella de procesador:

-   Uno o más procesadores lógicos se cargan

-   Uno o más procesadores virtuales se cargan

Puede usar los siguientes contadores de rendimiento desde el host:

-   Utilización del procesador lógico - \\procesadores lógicos del hipervisor de Hyper-V (\*)\\% Total de tiempo de ejecución

-   Utilización del procesador virtual - \\procesador Virtual del hipervisor de Hyper-V (\*)\\% Total de tiempo de ejecución

-   Raíz de la utilización del procesador Virtual - \\procesador Virtual raíz de hipervisor de Hyper-V (\*)\\% Total de tiempo de ejecución

Si el **procesadores lógicos del hipervisor de Hyper-V (\_Total)\\% tiempo de ejecución Total** contador es superior al 90%, el host está sobrecargado. Debe agregar más capacidad de procesamiento o mover algunas máquinas virtuales a un host diferente.

Si el **Hyper-V hipervisor Virtual Processor(VM Name:VP x)\\% tiempo de ejecución Total** contador es superior al 90% de todos los procesadores virtuales, debe hacer lo siguiente:

-   Compruebe que el host no está sobrecargado

-   Averigüe si la carga de trabajo puede aprovechar más procesadores virtuales

-   Asignar más procesadores virtuales a la máquina virtual

Si **Hyper-V hipervisor Virtual Processor(VM Name:VP x)\\% tiempo de ejecución Total** contador es superior al 90% para algunos, pero no todos, de los procesadores virtuales, debe hacer lo siguiente:

-   Si la carga de trabajo es recibir red intensivo, considere la posibilidad de usar vRSS.

-   Si las máquinas virtuales no se está ejecutando Windows Server 2012 R2, debe agregar más adaptadores de red.

-   Si su carga de trabajo intensivas de almacenamiento, debe habilitar NUMA virtual y agregar más discos virtuales.

Si el **procesador Virtual de raíz de hipervisor de Hyper-V (VP raíz x)\\% tiempo de ejecución Total** contador es más de un 90% durante algunos, pero no para todos los procesadores virtuales y la **(x) del procesador\\% de tiempo de interrupción y Procesador (x)\\% de tiempo de DPC** contador aproximadamente agrega el valor para el **raíz Virtual Processor(Root VP x)\\% tiempo de ejecución Total** contador, debe asegurarse de habilitar VMQ en los adaptadores de red.

## <a name="memory-bottlenecks"></a>Cuellos de botella de memoria

Estos son algunos escenarios comunes que podrían provocar cuellos de botella de memoria:

-   El host no está respondiendo.

-   No se puede iniciar las máquinas virtuales.

-   Las máquinas virtuales quedarse sin memoria.

Puede usar los siguientes contadores de rendimiento desde el host:

-   Memoria\\Mbytes disponibles

-   Equilibrador de memoria dinámica de Hyper-V (\*)\\memoria disponible

Puede usar los siguientes contadores de rendimiento de la máquina virtual:

-   Memoria\\Mbytes disponibles

Si el **memoria\\Mbytes disponibles** y **equilibrador de memoria dinámica de Hyper-V (\*)\\memoria disponible** contadores son bajos en el host, debe detener no esenciales de servicios y migración una o más máquinas virtuales a otro host.

Si el **memoria\\Mbytes disponibles** contador es bajo en la máquina virtual, debe asignar más memoria a la máquina virtual. Si usas la memoria dinámica, debe aumentar el valor máximo de memoria.

## <a name="network-bottlenecks"></a>Cuellos de botella de red

Estos son algunos escenarios comunes que podrían provocar cuellos de botella de red:

-   El host es el límite de la red.

-   La máquina virtual es el límite de la red.

Puede usar los siguientes contadores de rendimiento desde el host:

-   Interfaz de red (*el nombre del adaptador de red*)\\Bytes/seg.

Puede usar los siguientes contadores de rendimiento de la máquina virtual:

-   El adaptador de red Virtual de Hyper-V (*nombre de máquina virtual&lt;GUID&gt;*)\\Bytes/seg.

Si el **NIC física Bytes/seg.** contador es mayor o igual que el 90% de la capacidad, debe agregar adaptadores de red adicionales, migre las máquinas virtuales a otro host y configurar la QoS de red.

Si el **Bytes por segundo del adaptador de red Virtual de Hyper-V** contador es mayor o igual a 250 MBps, debe agregar adaptadores de red agrupados adicionales en la máquina virtual, habilite vRSS y usar SR-IOV.

Si las cargas de trabajo no pueden cumplir la latencia de red, habilite SR-IOV presentar los recursos de adaptador de red físico a la máquina virtual.

## <a name="storage-bottlenecks"></a>Cuellos de botella de almacenamiento

Estos son algunos escenarios comunes que podrían provocar cuellos de botella de almacenamiento:

-   El host y máquina virtual de las operaciones son lentas o tiempo de espera.

-   La máquina virtual es lenta.

Puede usar los siguientes contadores de rendimiento desde el host:

-   Disco físico (*letra de disco*)\\promedio de segundos de disco/lectura

-   Disco físico (*letra de disco*)\\promedio de segundos de disco/escritura

-   Disco físico (*letra de disco*)\\longitud de cola de lectura de disco de promedio

-   Disco físico (*letra de disco*)\\longitud de cola de escritura en disco promedio

Si las latencias son constantemente superior a 50 ms, haga lo siguiente:

-   Distribuir las máquinas virtuales en el almacenamiento adicional

-   Considere la posibilidad de compra de almacenamiento más rápido

-   Considere la posibilidad de espacios de almacenamiento en capas, que se introdujo en Windows Server 2012 R2

-   Considere el uso de QoS de almacenamiento, que se introdujo en Windows Server 2012 R2

-   Use VHDX

## <a name="see-also"></a>Vea también

-   [Terminología de Hyper-v.](terminology.md)

-   [Arquitectura de Hyper-V](architecture.md)

-   [Configuración del servidor de Hyper-V](configuration.md)

-   [Rendimiento del procesador de Hyper-V](processor-performance.md)

-   [Rendimiento de la memoria de Hyper-V](memory-performance.md)

-   [Rendimiento de E/S de almacenamiento de Hyper-V](storage-io-performance.md)

-   [Rendimiento de E/S de red de Hyper-V](network-io-performance.md)

-   [Máquinas virtuales Linux](linux-virtual-machine-considerations.md)
