---
title: Rendimiento de e/s de red de Hyper-V
description: Consideraciones de rendimiento de e/s de red en el ajuste del rendimiento de Hyper-V
ms.topic: article
ms.author: asmahi; sandysp; jopoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: b21ed45b97b1bc657b8a77ac7731dd32f5090c3d
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896110"
---
# <a name="hyper-v-network-io-performance"></a>Rendimiento de e/s de red de Hyper-V

El servidor 2016 contiene varias mejoras y nuevas funciones para optimizar el rendimiento de la red en Hyper-V.  La documentación sobre cómo optimizar el rendimiento de la red se incluirá en una versión futura de este artículo.

## <a name="live-migration"></a>Migración en vivo

Migración en vivo le permite trasladar de forma transparente máquinas virtuales en ejecución desde un nodo de un clúster de conmutación por error a otro nodo del mismo clúster sin que se interrumpa la conexión de red o se perciba tiempo de inactividad.

> [!NOTE]
> Los clústeres de conmutación por error requieren almacenamiento compartido para los nodos del clúster.

El proceso de mover una máquina virtual en ejecución puede dividirse en dos fases principales. La primera fase copia la memoria de la máquina virtual del host actual al nuevo host. La segunda fase transfiere el estado de la máquina virtual del host actual al nuevo host. La duración de ambas fases viene determinada en gran medida por la velocidad a la que se pueden transferir los datos desde el host actual al nuevo host.

Proporcionar una red dedicada para el tráfico de migración en vivo ayuda a minimizar el tiempo necesario para completar una migración en vivo y garantiza tiempos de migración coherentes.

![ejemplo de configuración de la migración en vivo de Hyper-v](../../media/perftune-guide-live-migration.png)

Además, el aumento del número de búferes de envío y recepción en cada adaptador de red implicado en la migración puede mejorar el rendimiento de la migración.

Windows Server 2012 R2 presentó una opción para acelerar Migración en vivo al comprimir la memoria antes de transferirla a través de la red o usar el acceso directo a memoria remota (RDMA), si el hardware lo admite.

## <a name="additional-references"></a>Referencias adicionales

-   [Terminología de Hyper-V](terminology.md)

-   [Arquitectura de Hyper-V](architecture.md)

-   [Configuración del servidor de Hyper-V](configuration.md)

-   [Rendimiento del procesador de Hyper-V](processor-performance.md)

-   [Rendimiento del procesador de Hyper-V](memory-performance.md)

-   [Rendimiento de E/S de almacenamiento de Hyper-V](storage-io-performance.md)

-   [Detección de cuellos de botella en un entorno virtualizado.](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuales Linux](linux-virtual-machine-considerations.md)
