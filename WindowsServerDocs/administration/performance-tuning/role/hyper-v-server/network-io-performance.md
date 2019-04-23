---
title: Rendimiento de E/S de red de Hyper-V
description: Consideraciones de rendimiento de e/s en Hyper-V optimización del rendimiento de red
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: d52c4fff6c7e06fb0a9f2b44ea51a0a790e6674d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814366"
---
# <a name="hyper-v-network-io-performance"></a>Rendimiento de E/S de red de Hyper-V

Server 2016 contiene varias mejoras y nuevas funcionalidades para optimizar el rendimiento en Hyper-V de la red.  Documentación sobre cómo optimizar el rendimiento de red se incluirá en una versión futura de este artículo.

## <a name="live-migration"></a>Migración en vivo

Migración en vivo permite mover de manera transparente máquinas virtuales en ejecución de un nodo de un clúster de conmutación por error a otro nodo del mismo clúster sin una conexión de red ni tiempo de inactividad perceptible.

**Tenga en cuenta**    agrupación en clústeres de conmutación por error requiere almacenamiento compartido para los nodos del clúster.

El proceso de mover una máquina virtual se puede dividir en dos fases principales. La primera fase, copia la memoria de la máquina virtual desde el host actual al nuevo host. La segunda fase transfiere el estado de la máquina virtual desde el host actual al nuevo host. Las duraciones de las dos fases en gran medida viene determinada por la velocidad a la que se pueden transferir datos desde el host actual al nuevo host.

Proporcionar una red dedicada tráfico de migración en vivo ayuda a minimizar el tiempo necesario para completar una migración en vivo y garantiza tiempos de migración coherente.

![ejemplo de configuración de migración en vivo de hyper-v](../../media/perftune-guide-live-migration.png)

Además, aumentar el número de envío y recepción en cada red adaptador que está implicado en la migración puede mejorar el rendimiento de la migración.

Windows Server 2012 R2 introdujo una opción para acelerar la migración en vivo mediante la compresión de memoria antes de transferir a través de la red o use remoto memoria acceso directo (RDMA), si el hardware lo admite.

## <a name="see-also"></a>Vea también

-   [Terminología de Hyper-v.](terminology.md)

-   [Arquitectura de Hyper-V](architecture.md)

-   [Configuración del servidor de Hyper-V](configuration.md)

-   [Rendimiento del procesador de Hyper-V](processor-performance.md)

-   [Rendimiento de la memoria de Hyper-V](memory-performance.md)

-   [Rendimiento de E/S de almacenamiento de Hyper-V](storage-io-performance.md)

-   [Detección de cuellos de botella en un entorno virtualizado.](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuales Linux](linux-virtual-machine-considerations.md)