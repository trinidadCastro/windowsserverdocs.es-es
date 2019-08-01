---
title: Detección de cuellos de botella en un entorno virtualizado
description: Detección y resolución de posibles cuellos de botella de rendimiento de Hyper-v
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: cdad5f0cc3b0e49ae46e975e3acc2c48a18e5f70
ms.sourcegitcommit: af80963a1d16c0b836da31efd9c5caaaf6708133
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "63722882"
---
# <a name="detecting-bottlenecks-in-a-virtualized-environment"></a>Detección de cuellos de botella en un entorno virtualizado

En esta sección se ofrecen algunas sugerencias sobre lo que se debe supervisar mediante el monitor de rendimiento y cómo identificar dónde podría ser el problema cuando el host o algunas de las máquinas virtuales no funcionan tal y como cabría esperar.

## <a name="processor-bottlenecks"></a>Cuellos de botella del procesador

Estos son algunos escenarios comunes que pueden provocar cuellos de botella en el procesador:

-   Se cargan uno o varios procesadores lógicos

-   Se cargan uno o varios procesadores virtuales

Puede usar los siguientes contadores de rendimiento del host:

-   Uso del procesador lógico \\: procesador lógico de hipervisor de Hyper\*-\\V ()% de tiempo total de ejecución

-   Uso de procesador virtual \\: procesador virtual de hipervisor de Hyper\*-\\V ()% de tiempo total de ejecución

-   Utilización del procesador virtual raíz \\: procesador virtual raíz del hipervisor de Hyper\*-\\V ()% total de tiempo de ejecución

Si el contador de **tiempo de ejecución total del\_procesador lógico del hipervisor de Hyper-V (total)\\** es superior al 90%, el host está sobrecargado. Debe agregar más capacidad de procesamiento o trasladar algunas máquinas virtuales a otro host.

Si el contador de **tiempo de ejecución del procesador virtual del hipervisor de Hyper-\\V (nombre de VM: VP x)** es superior al 90% para todos los procesadores virtuales, debe hacer lo siguiente:

-   Comprobar que el host no está sobrecargado

-   Averiguar si la carga de trabajo puede aprovechar más procesadores virtuales

-   Asignar más procesadores virtuales a la máquina virtual

Si el **procesador virtual del hipervisor de Hyper-V (nombre de VM\\: VP x)% total del contador en tiempo de ejecución** es superior al 90% para algunos, pero no todos, de los procesadores virtuales, debe hacer lo siguiente:

-   Si la carga de trabajo recibe un uso intensivo de la red, considere la posibilidad de usar vRSS.

-   Si las máquinas virtuales no ejecutan Windows Server 2012 R2, debe agregar más adaptadores de red.

-   Si la carga de trabajo requiere un uso intensivo del almacenamiento, debe habilitar NUMA virtual y agregar más discos virtuales.

Si el contador de **tiempo de ejecución del hipervisor de Hyper-V (\\raíz VP x)** es superior al 90% en algunos, pero no todos, los procesadores virtuales y el **procesador (\\x)% de tiempo de interrupción y\\procesador (x)% de tiempo de DPC** el contador aproximadamente suma el valor del contador de **tiempo de ejecución de la raíz del procesador\\virtual (raíz VP x) al total** . debe asegurarse de habilitar VMQ en los adaptadores de red.

## <a name="memory-bottlenecks"></a>Cuellos de botella de memoria

Estos son algunos escenarios comunes que pueden provocar cuellos de botella de memoria:

-   El host no responde.

-   No se pueden iniciar las máquinas virtuales.

-   Las máquinas virtuales se están quedando sin memoria.

Puede usar los siguientes contadores de rendimiento del host:

-   Mbytes de memoria\\disponibles

-   Memoria disponible del equilibrador de memoria dinámica\*de\\Hyper-V ()

Puede usar los siguientes contadores de rendimiento de la máquina virtual:

-   Mbytes de memoria\\disponibles

Si los **\\** contadores de memoria disponibles en Mbytes e **memoria dinámica de Hyper\*-\\V ()** están bajo en el host, debe detener los servicios no esenciales y migrar uno o más equipos a otro host.

Si el **contador\\MB de memoria disponible** está bajo en la máquina virtual, debe asignar más memoria a la máquina virtual. Si usa Memoria dinámica, debe aumentar la configuración de memoria máxima.

## <a name="network-bottlenecks"></a>Cuellos de botella de red

Estos son algunos escenarios comunes que pueden provocar cuellos de botella en la red:

-   El host está enlazado a la red.

-   La máquina virtual está enlazada a la red.

Puede usar los siguientes contadores de rendimiento del host:

-   Interfaz de red (nombre de adaptador\\de*red*) bytes por segundo

Puede usar los siguientes contadores de rendimiento de la máquina virtual:

-   Adaptador de Virtual Network de Hyper-V (*GUID&lt;&gt;de nombre de máquina virtual*)\\bytes por segundo

Si el contador de bytes de **NIC/s físicos** es mayor o igual que el 90% de la capacidad, debe agregar adaptadores de red adicionales, migrar máquinas virtuales a otro host y configurar QoS de red.

Si el contador de bytes de **adaptador de Virtual Network de Hyper-V** es mayor o igual que 250 Mbps, debe agregar adaptadores de red agrupados adicionales en la máquina virtual, habilitar vRSS y usar SR-IOV.

Si las cargas de trabajo no pueden cumplir su latencia de red, habilite SR-IOV para presentar recursos de adaptador de red físico a la máquina virtual.

## <a name="storage-bottlenecks"></a>Cuellos de botella de almacenamiento

Estos son algunos escenarios comunes que pueden provocar cuellos de botella de almacenamiento:

-   Las operaciones del host y de la máquina virtual son lentas o se agota el tiempo de espera.

-   La máquina virtual es lenta.

Puede usar los siguientes contadores de rendimiento del host:

-   Disco físico (*letra*de disco\\) promedio de segundos de disco/lectura

-   Disco físico (*letra*de disco\\) promedio de segundos de disco/escritura

-   Disco físico (*letra*de disco\\) Longitud promedio de cola de lectura de disco

-   Disco físico (*letra*de disco\\) Longitud promedio de cola de escritura de disco

Si las latencias son constantemente mayores que 50 ms, debe hacer lo siguiente:

-   Distribución de máquinas virtuales en almacenamiento adicional

-   Considere la posibilidad de adquirir un almacenamiento más rápido

-   Considere la posibilidad de Tiered Storage espacios, que se introdujeron en Windows Server 2012 R2.

-   Considere la posibilidad de usar QoS de almacenamiento, que se presentó en Windows Server 2012 R2.

-   Usar VHDX

## <a name="see-also"></a>Vea también

-   [Terminología de Hyper-V](terminology.md)

-   [Arquitectura de Hyper-V](architecture.md)

-   [Configuración del servidor de Hyper-V](configuration.md)

-   [Rendimiento del procesador de Hyper-V](processor-performance.md)

-   [Rendimiento del procesador de Hyper-V](memory-performance.md)

-   [Rendimiento de E/S de almacenamiento de Hyper-V](storage-io-performance.md)

-   [Rendimiento de E/S de red de Hyper-V](network-io-performance.md)

-   [Máquinas virtuales Linux](linux-virtual-machine-considerations.md)
