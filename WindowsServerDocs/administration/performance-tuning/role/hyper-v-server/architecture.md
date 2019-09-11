---
title: Arquitectura de Hyper-V
description: Arquitectura de Hyper-v condsiderations para el ajuste del rendimiento
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 7ed8522ec34e9e262f835530a248567ebbd0ddc9
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866669"
---
# <a name="hyper-v-architecture"></a>Arquitectura de Hyper-V

Hyper-V incluye una arquitectura basada en hipervisor de tipo 1. El hipervisor Virtualiza los procesadores y la memoria, y proporciona mecanismos para la pila de virtualización en la partición raíz para administrar las particiones secundarias (máquinas virtuales) y exponer servicios como dispositivos de e/s a las máquinas virtuales.

La partición raíz posee y tiene acceso directo a los dispositivos de e/s físicos. La pila de virtualización en la partición raíz proporciona un administrador de memoria para las máquinas virtuales, las API de administración y los dispositivos de e/s virtualizados. También implementa dispositivos emulados, como el controlador de disco IDE (electrónica integrada de dispositivos) y el puerto de dispositivo de entrada PS/2, y admite dispositivos sintéticos específicos de Hyper-V para aumentar el rendimiento y reducir la sobrecarga.

![arquitectura basada en hipervisor de Hyper-v](../../media/perftune-guide-hyperv-arch.png)

La arquitectura de e/s específica de Hyper-V consta de proveedores de servicios de virtualización (VSPs) en la partición raíz y los clientes del servicio de virtualización (VSCs) en la partición secundaria. Cada servicio se expone como un dispositivo a través de VMBus, que actúa como un bus de e/s y permite la comunicación de alto rendimiento entre las máquinas virtuales que usan mecanismos como la memoria compartida. El administrador de Plug and Play del sistema operativo invitado enumera estos dispositivos, incluido VMBus, y carga los controladores de dispositivo adecuados (clientes de servicio virtual). Los servicios que no son de e/s también se exponen a través de esta arquitectura.

A partir de Windows Server 2008, el sistema operativo incluye habilitaciones para optimizar su comportamiento cuando se ejecuta en máquinas virtuales. Entre las ventajas se incluyen la reducción del costo de la virtualización de memoria, la mejora de la escalabilidad de varios núcleos y la disminución del uso de CPU en segundo plano del sistema operativo invitado.

En las secciones siguientes se sugieren procedimientos recomendados que ofrecen un mayor rendimiento en los servidores que ejecutan el rol Hyper-V.

## <a name="see-also"></a>Vea también

-   [Terminología de Hyper-V](terminology.md)

-   [Configuración del servidor de Hyper-V](configuration.md)

-   [Rendimiento del procesador de Hyper-V](processor-performance.md)

-   [Rendimiento del procesador de Hyper-V](memory-performance.md)

-   [Rendimiento de E/S de almacenamiento de Hyper-V](storage-io-performance.md)

-   [Rendimiento de E/S de red de Hyper-V](network-io-performance.md)

-   [Detección de cuellos de botella en un entorno virtualizado.](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuales Linux](linux-virtual-machine-considerations.md)
