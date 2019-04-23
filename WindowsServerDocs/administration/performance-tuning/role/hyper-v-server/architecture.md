---
title: Arquitectura de Hyper-V
description: Arquitectura de Hyper-v condsiderations para optimizar el rendimiento
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fcc87b04698a44e115c8f49150fe33443f8e6a88
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890286"
---
# <a name="hyper-v-architecture"></a>Arquitectura de Hyper-V

Hyper-V ofrece una arquitectura basada en hipervisor de tipo 1. El hipervisor virtualiza procesadores y memoria y proporciona mecanismos para la pila de virtualización en la partición raíz para administrar las particiones secundarias (máquinas virtuales) y exponer los servicios, como dispositivos de E/S para las máquinas virtuales.

La partición raíz posee y tiene acceso directo a los dispositivos de E/S físicos. La pila de virtualización en la partición raíz proporciona un administrador de memoria para las máquinas virtuales, las API de administración y los dispositivos de E/S virtualizados. También implementa dispositivos emulados, como el controlador de disco electronics (IDE) de dispositivos integrados y puerto de dispositivo de entrada de PS/2 y es compatible con los dispositivos sintéticos específicos de Hyper-V para aumentar el rendimiento y reducción de la sobrecarga.

![arquitectura basada en hipervisor de Hyper-v](../../media/perftune-guide-hyperv-arch.png)

La arquitectura de E/S específico de Hyper-V consta de los proveedores de servicios de virtualización (vsp) en la raíz virtualización y la partición de servicio a clientes (VSC) en la partición secundaria. Cada servicio se exponen como un dispositivo a través de VMBus, que actúa como un bus de E/S y permite la comunicación de alto rendimiento entre las máquinas virtuales que usan mecanismos como la memoria compartida. Administrador de Plug and Play del sistema operativo invitado enumera estos dispositivos, incluidos VMBus y carga los controladores de dispositivo adecuado (clientes de servicio virtual). Servicios que no sean de E/S también se exponen a través de esta arquitectura.

A partir de Windows Server 2008, el giraría características del sistema operativo para optimizar su comportamiento cuando se ejecuta en máquinas virtuales. Las ventajas incluyen la reducción del costo de virtualización de la memoria, mejorar la escalabilidad de varios núcleos y reduce el uso de CPU del sistema operativo invitado de fondo.

Las secciones siguientes sugieren prácticas recomendadas que producen un mayor rendimiento en los servidores que ejecutan el rol Hyper-V.

## <a name="see-also"></a>Vea también

-   [Terminología de Hyper-v.](terminology.md)

-   [Configuración del servidor de Hyper-V](configuration.md)

-   [Rendimiento del procesador de Hyper-V](processor-performance.md)

-   [Rendimiento de la memoria de Hyper-V](memory-performance.md)

-   [Rendimiento de E/S de almacenamiento de Hyper-V](storage-io-performance.md)

-   [Rendimiento de E/S de red de Hyper-V](network-io-performance.md)

-   [Detección de cuellos de botella en un entorno virtualizado.](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuales Linux](linux-virtual-machine-considerations.md)
