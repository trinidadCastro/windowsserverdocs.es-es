---
title: Prácticas recomendadas para ejecutar Linux en Hyper-V
description: Proporciona recomendaciones para ejecutar Linux en una máquina virtual
ms.topic: article
ms.assetid: a08648eb-eea0-4e2b-87fb-52bfe8953491
ms.author: benarm
author: BenjaminArmstrong
ms.date: 04/15/2020
ms.openlocfilehash: 216bd83eb06cd14b2b2290e3294041b097cfdbd9
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90747170"
---
# <a name="best-practices-for-running-linux-on-hyper-v"></a>Prácticas recomendadas para ejecutar Linux en Hyper-V

>Se aplica a: Windows Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

Este tema contiene una lista de recomendaciones para ejecutar máquinas virtuales Linux en Hyper-V.

## <a name="tuning-linux-file-systems-on-dynamic-vhdx-files"></a>Ajuste de sistemas de archivos de Linux en archivos VHDX dinámicos

Algunos sistemas de archivos de Linux pueden consumir grandes cantidades de espacio en disco real incluso cuando el sistema de archivos está vacío. Para reducir la cantidad de uso real de espacio en disco de los archivos VHDX dinámicos, tenga en cuenta las siguientes recomendaciones:

* Al crear el VHDX, use 1 MB BlockSizeBytes (de los 32 MB predeterminados) en PowerShell, por ejemplo:

```Powershell
PS > New-VHD -Path C:\MyVHDs\test.vhdx -SizeBytes 127GB -Dynamic -BlockSizeBytes 1MB
```

* El formato ext4 es preferible a ext3 porque ext4 es más eficaz que ext3 cuando se usa con archivos VHDX dinámicos.

* Al crear el sistema de archivos, especifique el número de grupos que se van a 4096, por ejemplo:

```bash
# mkfs.ext4 -G 4096 /dev/sdX1

```

## <a name="grub-menu-timeout-on-generation-2-virtual-machines"></a>Tiempo de espera del menú de GRUB en la generación 2 Virtual Machines

Debido a que el hardware heredado se quita de la emulación en máquinas virtuales de generación 2, el temporizador de la cuenta atrás del menú de GRUB se recuenta demasiado rápido para que se muestre el menú de GRUB, cargando inmediatamente la entrada predeterminada. Hasta que se corrija GRUB para usar el temporizador compatible con EFI, modifique **/boot/grub/grub.conf**,/**etc/default/GRUB**o equivalente a tener "timeout = 100.000" en lugar del valor predeterminado "timeout = 5".

## <a name="pxe-boot-on-generation-2-virtual-machines"></a>Arranque PxE en la generación 2 Virtual Machines

Dado que el temporizador de PIT no está presente en la generación 2 Virtual Machines, las conexiones de red al servidor TFTP de PxE pueden finalizar prematuramente y evitar que el cargador de arranque Lea la configuración de GRUB y cargue un kernel desde el servidor.

En RHEL 6. x, se puede usar el cargador de arranque de EFI heredado de GRUB v 0.97 en lugar de grub2, como se describe aquí: [https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html)

En las distribuciones de Linux que no sean RHEL 6. x, se pueden seguir pasos similares para configurar GRUB v 0.97 para cargar kernels de Linux desde un servidor PxE.

Además, en la entrada de mouse y teclado de RHEL/versión 6,6, no funcionará con el kernel de preinstalación, lo que evita que se especifiquen opciones de instalación en el menú. Una consola serie debe estar configurada para permitir la elección de opciones de instalación.

* En el archivo **efidefault** en el servidor PXE, agregue el siguiente parámetro de kernel **"Console = ttyS1"** .

* En la máquina virtual de Hyper-V, configure un puerto COM mediante este cmdlet de PowerShell:

```Powershell
Set-VMComPort -VMName <Name> -Number 2 -Path \\.\pipe\dbg1

```

Si se especifica un archivo Kickstart en el kernel de preinstalación, también se evita la necesidad de la entrada del teclado y del mouse durante la instalación.

## <a name="use-static-mac-addresses-with-failover-clustering"></a>Usar direcciones MAC estáticas con clústeres de conmutación por error

Las máquinas virtuales Linux que se implementarán mediante clústeres de conmutación por error deben configurarse con una dirección Media Access Control (MAC) estática para cada adaptador de red virtual. En algunas versiones de Linux, la configuración de red se puede perder después de la conmutación por error porque se asigna una nueva dirección MAC al adaptador de red virtual. Para evitar la pérdida de la configuración de red, asegúrese de que cada adaptador de red virtual tiene una dirección MAC estática. Puede configurar la dirección MAC editando la configuración de la máquina virtual en el administrador de Hyper-V o Administrador de clústeres de conmutación por error.

## <a name="use-hyper-v-specific-network-adapters-not-the-legacy-network-adapter"></a>Usar adaptadores de red específicos de Hyper-V, no el adaptador de red heredado

Configure y use el adaptador Ethernet virtual, que es una tarjeta de red específica de Hyper-V con un rendimiento mejorado. Si los adaptadores de red heredados y específicos de Hyper-V están conectados a una máquina virtual, los nombres de red de la salida de **ifconfig-a** pueden mostrar valores aleatorios como **_tmp12000801310**. Para evitar este problema, quite todos los adaptadores de red heredados al usar adaptadores de red específicos de Hyper-V en una máquina virtual Linux.

## <a name="use-io-scheduler-noopnone-for-better-disk-io-performance"></a>Usar el programador de e/s en NOOP/ninguno para mejorar el rendimiento de e/s de disco

El kernel de Linux ofrece dos conjuntos de programadores de e/s de disco para reordenar las solicitudes.  Un conjunto es para el subsistema "BLK" anterior y un conjunto es para el subsistema "BLK-MQ" más reciente. En cualquier caso, con los discos de estado sólido de hoy en día, se recomienda usar un programador que pase las decisiones de programación al hipervisor de Hyper-V subyacente. En el caso de los kernels de Linux que usan el subsistema "BLK", este es el programador "noop". En el caso de los kernels de Linux que usan el subsistema "BLK-MQ", este es el programador "ninguno".

En el caso de un disco determinado, los programadores disponibles pueden verse en esta ubicación del sistema de archivos:/sys/class/Block/ `<diskname>` /Queue/Scheduler, con el programador seleccionado actualmente entre corchetes. Puede cambiar el programador escribiendo en esta ubicación del sistema de archivos. El cambio se debe agregar a un script de inicialización para que pueda conservarse entre los reinicios. Consulte la documentación de distribución de Linux para obtener más información.

## <a name="numa"></a>NUMA

Las versiones de kernel de Linux inferiores a la versión 2.6.37 no admiten NUMA en Hyper-V con tamaños de VM más grandes. Este problema afecta principalmente a las distribuciones anteriores que usan el kernel Red Hat 2.6.32 de canal de subida, y se ha corregido en Red Hat Enterprise Linux (RHEL) 6.6 (kernel-2.6.32-504). Los sistemas que ejecutan kernels personalizados cuyas versiones son anteriores a la versión 2.6.37, o bien kernels basados en RHEL cuyas versiones son anteriores a la versión 2.6.32-504, deben establecer el parámetro de inicio `numa=off` en la línea de comandos de kernel en grub.conf. Para más información, consulte [Red Hat KB 436883](https://access.redhat.com/solutions/436883).

## <a name="reserve-more-memory-for-kdump"></a>Reserve más memoria para kdump

En caso de que el kernel de captura de volcado acabe con un pánico en el arranque, Reserve más memoria para el kernel. Por ejemplo, cambie el parámetro **crashkernel = 384M-: 128M** a **crashkernel = 384M-: 256M** en el archivo de configuración de GRUB de Ubuntu.

## <a name="shrinking-vhdx-or-expanding-vhd-and-vhdx-files-can-result-in-erroneous-gpt-partition-tables"></a>La reducción del VHDX o la expansión de archivos VHD y VHDX puede dar lugar a tablas de particiones GPT erróneas

Hyper-V permite la reducción de archivos de disco virtual (VHDX) sin tener en cuenta las estructuras de datos de particiones, volúmenes o sistemas de archivos que puedan existir en el disco. Si el VHDX se reduce a donde se encuentra el final del VHDX antes del final de una partición, se pueden perder los datos, se puede dañar la partición o se pueden devolver datos no válidos cuando se lee la partición.

Después de cambiar el tamaño de un VHD o VHDX, los administradores deben usar una utilidad como fdisk o parte de para actualizar las estructuras de partición, volumen y sistema de archivos para reflejar el cambio en el tamaño del disco. Al reducir o expandir el tamaño de un VHD o VHDX que tenga una tabla de particiones GUID (GPT) se producirá una advertencia cuando se use una herramienta de administración de particiones para comprobar el diseño de las particiones, y se avisará al administrador para que corrija el primer y el secundario. Este paso manual es seguro para realizar sin pérdida de datos.

## <a name="additional-references"></a>Referencias adicionales

* [Máquinas virtuales Linux y FreeBSD compatibles con Hyper-V en Windows](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

* [Prácticas recomendadas para ejecutar FreeBSD en Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)

* [Implementar un clúster de Hyper-V](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj863389(v=ws.11))

* [Creación de imágenes de Linux para Azure](/azure/virtual-machines/linux/create-upload-generic)

* [Optimización de la máquina virtual Linux en Azure](/azure/virtual-machines/linux/optimization)