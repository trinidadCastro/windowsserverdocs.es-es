---
title: Consideraciones sobre máquinas virtuales Linux
description: Máquina virtual Linux y BSD
ms.topic: article
ms.author: asmahi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: be9d2f0d758f6f59282c770ef1a48d71a947ba00
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078292"
---
# <a name="linux-virtual-machine-considerations"></a>Consideraciones sobre máquinas virtuales Linux

Las máquinas virtuales Linux y BSD tienen consideraciones adicionales en comparación con las máquinas virtuales de Windows en Hyper-V.

La primera consideración es si Integration Services están presentes o si la máquina virtual se está ejecutando simplemente en hardware emulado sin ninguna habilitación. Hay disponible una tabla de las versiones de Linux y BSD que tienen Integration Services integradas o descargables en [máquinas virtuales Linux y FreeBSD compatibles con Hyper-V en Windows](../../../../virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md). Estas páginas tienen cuadrículas de las características de Hyper-V disponibles para las versiones de distribución de Linux y notas sobre esas características, si procede.

Incluso cuando el invitado se ejecuta Integration Services, se puede configurar con hardware heredado que no presenta el mejor rendimiento. Por ejemplo, configure y use un adaptador Ethernet virtual para el invitado en lugar de usar un adaptador de red heredado. Con Windows Server 2016, también están disponibles redes avanzadas como SR-IOV.

## <a name="linux-network-performance"></a>Rendimiento de red de Linux

De forma predeterminada, Linux habilita las descargas y la aceleración de hardware de forma predeterminada. Si vRSS está habilitado en las propiedades de una NIC en el host y el invitado de Linux tiene la capacidad de usar vRSS, se habilitará la funcionalidad. En PowerShell se puede cambiar este mismo parámetro con el `EnableNetAdapterRSS` comando.

Del mismo modo, la característica VMMQ (RSS de conmutador virtual) se puede habilitar en la NIC física que usa la configuración de **propiedades**de invitado  >  **...**  >  Pestaña **avanzadas** > establecer el **RSS del conmutador virtual** en **habilitado** o habilitar VMMQ en PowerShell mediante lo siguiente:

```PowerShell
 Set-VMNetworkAdapter -VMName **$VMName** -VmmqEnabled $True
 ```

En, el ajuste TCP adicional del invitado puede realizarse aumentando los límites. Para obtener el mejor rendimiento de la distribución de la carga de trabajo a través de varias CPU y las cargas de trabajo profundas producen el mejor rendimiento, ya que las cargas de trabajo virtualizadas tendrán una latencia mayor que las "sin sistema operativo".

Algunos parámetros de optimización de ejemplo que han sido útiles en las pruebas comparativas de red son:

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

Una herramienta útil para microbenchmarks de red es ntttcp, que está disponible en Linux y Windows. La versión de Linux es de código abierto y está disponible en [ntttcp-for-Linux en github.com](https://github.com/Microsoft/ntttcp-for-linux). La versión de Windows se puede encontrar en el [centro de descarga](https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769). Cuando se optimizan las cargas de trabajo, es mejor usar tantas secuencias como sea necesario para obtener el mejor rendimiento. Con ntttcp para modelar el tráfico, el `-P` parámetro establece el número de conexiones paralelas usadas.

## <a name="linux-storage-performance"></a>Rendimiento de almacenamiento de Linux

Algunos procedimientos recomendados, como los siguientes, se enumeran en [procedimientos recomendados para ejecutar Linux en Hyper-V](../../../../virtualization/hyper-v/best-practices-for-running-linux-on-hyper-v.md). El kernel de Linux tiene diferentes programadores de e/s para reordenar las solicitudes con diferentes algoritmos. NOOP es una cola primero en salir que pasa la decisión de programación que va a realizar el hipervisor. Se recomienda usar NOOP como programador al ejecutar la máquina virtual Linux en Hyper-V. Para cambiar el programador de un dispositivo específico, en la configuración del cargador de arranque (por ejemplo,/etc/grub.conf), agregue `elevator=noop` los parámetros de kernel y, a continuación, reinicie.

De forma similar a las redes, el rendimiento de invitados de Linux con almacenamiento aprovecha al máximo varias colas con suficiente profundidad para mantener el host ocupado. La herramienta de pruebas comparativas de FIO con el motor de libaio es probablemente la mejor opción de rendimiento del almacenamiento.

## <a name="additional-references"></a>Referencias adicionales

-   [Terminología de Hyper-V](terminology.md)

-   [Arquitectura de Hyper-V](architecture.md)

-   [Configuración del servidor de Hyper-V](configuration.md)

-   [Rendimiento del procesador de Hyper-V](processor-performance.md)

-   [Rendimiento del procesador de Hyper-V](memory-performance.md)

-   [Rendimiento de E/S de almacenamiento de Hyper-V](storage-io-performance.md)

-   [Rendimiento de E/S de red de Hyper-V](network-io-performance.md)

-   [Detección de cuellos de botella en un entorno virtualizado.](detecting-virtualized-environment-bottlenecks.md)