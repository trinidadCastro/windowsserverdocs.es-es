---
title: Consideraciones de la máquina Virtual Linux
description: Máquina virtual Linux y BSD
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: cc6aab7825754579269eb05e591ca2a3cf5a561b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869686"
---
# <a name="linux-virtual-machine-considerations"></a>Consideraciones de la máquina Virtual Linux

Linux y máquinas virtuales BSD tienen consideraciones adicionales en comparación con las máquinas virtuales de Windows en Hyper-V.

La primera consideración es si los servicios de integración están presentes o si la máquina virtual se ejecuta solamente en hardware emulado con ninguna iluminación. Una tabla de versiones de Linux y BSD que tienen integrado o descargable Integration Services está disponible en [Linux y FreeBSD máquinas virtuales de Hyper-V en Windows](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows). Estas páginas tienen cuadrículas de características disponibles de Hyper-V disponibles para las versiones de distribución de Linux y notas sobre esas características, si procede.

Incluso cuando el invitado ejecuta Servicios de integración, se puede configurar con hardware heredado que no presenta el mejor rendimiento. Por ejemplo, configurar y usar un adaptador de ethernet virtual para el invitado en lugar de usar un adaptador de red heredado. Con Windows Server 2016, avanzados de red, como SR-IOV también están disponibles.

## <a name="linux-network-performance"></a>Rendimiento de la red de Linux

Linux de forma predeterminada permite la aceleración de hardware y descarga de forma predeterminada. Si vRSS se habilita en las propiedades de una NIC en el host y el invitado Linux tiene la capacidad para usar vRSS se habilitará la funcionalidad. En Powershell se puede cambiar este mismo parámetro con el `EnableNetAdapterRSS` comando.

De forma similar, se puede habilitar la característica VMMQ (RSS de conmutador Virtual) en la NIC física usada por el invitado **propiedades** > **configurar...**   >  **Avanzadas** ficha > establecer **RSS de conmutador Virtual** a **habilitado** o habilitar VMMQ en Powershell mediante los siguientes:

```PowerShell
 Set-VMNetworkAdapter -VMName **$VMName** -VmmqEnabled $True
 ```

En el invitado optimización adicional de TCP se puede realizar mediante el aumento de los límites. Para optimizar el rendimiento repartir la carga de trabajo a través de varias CPU y tener profundos cargas de trabajo genera el mejor rendimiento, como cargas de trabajo virtualizadas tendrán una latencia mayor "sin sistema operativo" aquellos.

Algunos parámetros de ajuste de ejemplo que hayan sido útiles en las pruebas comparativas de red incluyen:

```PowerShell
net.core.netdev_max_backlog = 30000
net.core.rmem_max = 67108864
net.core.wmem_max = 67108864
net.ipv4.tcp_wmem = 4096 12582912 33554432
net.ipv4.tcp_rmem = 4096 12582912 33554432
net.ipv4.tcp_max_syn_backlog = 80960
net.ipv4.tcp_slow_start_after_idle = 0
net.ipv4.tcp_tw_reuse = 1
net.ipv4.ip_local_port_range = 10240 65535
net.ipv4.tcp_abort_on_overflow = 1
```

Una herramienta útil para la red microbenchmarks es ntttcp, que está disponible en Linux y Windows. La versión de Linux es código abierto y está disponible en [ntttcp-for-linux en github.com](https://github.com/Microsoft/ntttcp-for-linux). La versión de Windows puede encontrarse en el [centro de descarga](https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769). Al optimizar las cargas de trabajo es mejor usar secuencias tantos como sea necesario para obtener el mejor rendimiento. Mediante ntttcp al tráfico del modelo, el `-P` parámetro establece el número de conexiones en paralelo utiliza.

## <a name="linux-storage-performance"></a>Rendimiento del almacenamiento de Linux

Algunas prácticas recomendadas, similar al siguiente, se muestran en [prácticas recomendadas para el que ejecuta Linux en Hyper-V](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/best-practices-for-running-linux-on-hyper-v). El kernel de Linux tiene distintos programadores de E/S para reordenar las solicitudes con algoritmos diferentes. NOOP es una cola primero en entrar es el primero que pasa la decisión de programación se realiza por el hipervisor. Se recomienda usar NOOP como programador cuando se ejecuta la máquina virtual Linux en Hyper-V. Cambiar el programador de un dispositivo específico, en la configuración del cargador de arranque (/ etc/grub.conf, por ejemplo), agregue `elevator=noop` para los parámetros de kernel y, a continuación, reinicie.

Similar a la red, el rendimiento del invitado Linux con almacenamiento beneficia el máximo provecho de varias colas con es lo suficientemente profunda para mantener ocupado el host. Es mejor con la herramienta de pruebas comparativas fio con el motor libaio Microbenchmarking el rendimiento del almacenamiento.

## <a name="see-also"></a>Vea también

-   [Terminología de Hyper-v.](terminology.md)

-   [Arquitectura de Hyper-V](architecture.md)

-   [Configuración del servidor de Hyper-V](configuration.md)

-   [Rendimiento del procesador de Hyper-V](processor-performance.md)

-   [Rendimiento de la memoria de Hyper-V](memory-performance.md)

-   [Rendimiento de E/S de almacenamiento de Hyper-V](storage-io-performance.md)

-   [Rendimiento de E/S de red de Hyper-V](network-io-performance.md)

-   [Detección de cuellos de botella en un entorno virtualizado.](detecting-virtualized-environment-bottlenecks.md)
