---
title: Procedimientos recomendados para ejecutar Linux en Hyper-V
description: Proporciona recomendaciones para la ejecución de Linux en una máquina virtual
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a08648eb-eea0-4e2b-87fb-52bfe8953491
author: shirgall
ms.author: kathydav
ms.date: 3/1/2019
ms.openlocfilehash: a24e2b1a1d79d52c1cc16f9e7c1b253d9b477aae
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284441"
---
# <a name="best-practices-for-running-linux-on-hyper-v"></a>Procedimientos recomendados para ejecutar Linux en Hyper-V

>Se aplica a: Windows Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

Este tema contiene una lista de recomendaciones para la ejecución de máquina virtual Linux en Hyper-V.

## <a name="tuning-linux-file-systems-on-dynamic-vhdx-files"></a>Sistemas de archivos de Linux en los archivos VHDX dinámicos de optimización

Algunos sistemas de archivos de Linux pueden consumir grandes cantidades de espacio en disco real, incluso cuando el sistema de archivos está principalmente vacío. Para reducir la cantidad de uso de espacio en disco real de los archivos VHDX dinámicos, tenga en cuenta las siguientes recomendaciones:

* Al crear el VHDX, utilice BlockSizeBytes de 1 MB (desde el valor predeterminado de 32MB) en PowerShell, por ejemplo:

```Powershell
PS > New-VHD -Path C:\MyVHDs\test.vhdx -SizeBytes 127GB -Dynamic -BlockSizeBytes 1MB
```

* El formato de ext4 es preferible a ext3 porque ext4 es más eficaz de espacio que ext3 cuando se usa con archivos VHDX dinámicos.

* Cuando se especifica el número de grupos con 4096, por ejemplo crear el sistema de archivos:

```bash
# mkfs.ext4 -G 4096 /dev/sdX1

```

## <a name="grub-menu-timeout-on-generation-2-virtual-machines"></a>Tiempo de espera de menú de GRUB en máquinas virtuales de generación 2

Debido a hardware heredado que se va a quitar de la emulación en máquinas virtuales de generación 2, el temporizador del menú de grub cuenta hacia atrás demasiado rápido para que el menú de grub para mostrarse, cargar inmediatamente la entrada predeterminada. Modificar hasta que se corrija grub para usar el temporizador compatible con EFI, **/boot/grub/grub.conf**, /**predeterminada/etcetera/grub**, o equivalente a tener "tiempo de espera = 100000" en lugar del predeterminado "timeout = 5".

## <a name="pxe-boot-on-generation-2-virtual-machines"></a>Arranque de PxE en máquinas virtuales de generación 2

Dado que el temporizador PIT no está presente en Generation 2 Virtual Machines, las conexiones de red en el servidor PxE TFTP se pueden finalizar prematuramente y evitar que el cargador de arranque de leer la configuración de Grub y cargar un kernel desde el servidor.

En RHEL 6.x, el cargador de arranque de grub heredado v0.97 EFI puede usarse en lugar de grub2 como se describe aquí: [https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html)

En las distribuciones de Linux que no sea de RHEL 6.x, se pueden seguir los mismos pasos para configurar v0.97 grub para cargar los kernels de Linux desde un servidor PxE.

Además, en RHEL/CentOS 6.6 teclado y mouse entrada no funcionará con el kernel previo a la instalación, lo que impide que especifica las opciones de instalación en el menú. Una consola de serie debe configurarse para permitir elegir opciones de instalación.

* En el **efidefault** en el servidor PxE, agregue el siguiente parámetro de kernel **"consola = ttyS1"**

* En la máquina virtual de Hyper-V, configurar un puerto COM mediante este cmdlet de PowerShell:

```Powershell
Set-VMComPort -VMName <Name> -Number 2 -Path \\.\pipe\dbg1

```

Especificar un archivo kickstart al núcleo previo a la instalación también evitaría la necesidad de teclado y mouse de entrada durante la instalación.

## <a name="use-static-mac-addresses-with-failover-clustering"></a>Usar direcciones MAC estáticas con agrupación en clústeres de conmutación por error

Máquinas virtuales Linux que se implementan mediante la agrupación en clústeres de conmutación por error debe configurarse con una dirección de estático de media access control (MAC) para cada adaptador de red virtual. En algunas versiones de Linux, la configuración de red puede perderse después de la conmutación por error porque se le asigna una nueva dirección MAC para el adaptador de red virtual. Para evitar perder la configuración de red, asegúrese de que cada adaptador de red virtual tiene una dirección MAC estática. Puede configurar la dirección MAC, edite la configuración de la máquina virtual en el Administrador de Hyper-V o el Administrador de clústeres de conmutación por error.

## <a name="use-hyper-v-specific-network-adapters-not-the-legacy-network-adapter"></a>Usar adaptadores de red específico de Hyper-V, no el adaptador de red heredado

Configurar y usar el adaptador de Ethernet virtual, que es una tarjeta de red específico de Hyper-V con un rendimiento mejorado. Si heredado y adaptadores de red específico de Hyper-V están conectados a una máquina virtual, los nombres de la red en la salida de **ifconfig - a** podría mostrar los valores aleatorios como **_tmp12000801310**. Para evitar este problema, quite todos los adaptadores de red heredados al usar adaptadores de red específico de Hyper-V en una máquina virtual Linux.

## <a name="use-io-scheduler-noop-for-better-disk-io-performance"></a>Usar al programador NOOP de E/S de disco de un mejor rendimiento de E/S

El kernel de Linux tiene cuatro distintos programadores de E/S para reordenar las solicitudes con algoritmos diferentes. NOOP es una cola primero en entrar es el primero que pasa la decisión de programación se realiza por el hipervisor. Se recomienda usar NOOP como programador cuando se ejecuta la máquina virtual Linux en Hyper-V. Cambiar el programador de un dispositivo específico, en la configuración del cargador de arranque (/ etc/grub.conf, por ejemplo), agregue **ascensor = noop** para los parámetros de kernel y, a continuación, reinicie.

## <a name="numa"></a>NUMA

Las versiones de kernel de Linux anteriores a 2.6.37 no admiten NUMA en Hyper-V con tamaños mayores de máquina virtual. Este problema afecta principalmente a las distribuciones anteriores que usan el kernel Red Hat 2.6.32 y se ha corregido en Red Hat Enterprise Linux (RHEL) 6.6 (kernel-2.6.32-504). Sistemas que ejecutan kernels personalizados anteriores a la versión 2.6.37, o basados en RHEL kernels anteriores que 2.6.32-504 debe establecer el parámetro de arranque `numa=off` en la línea de comandos de kernel en grub.conf. Para obtener más información, consulte [Red Hat KB 436883](https://access.redhat.com/solutions/436883).

## <a name="reserve-more-memory-for-kdump"></a>Reservar más memoria para kdump

En caso de que el kernel de captura de volcado de memoria termina con un fallo en el arranque, reservar más memoria para el kernel. Por ejemplo, cambie el parámetro **crashkernel = 384M-:128M** a **crashkernel = 384M-:256M** en el archivo de configuración de grub de Ubuntu.

## <a name="shrinking-vhdx-or-expanding-vhd-and-vhdx-files-can-result-in-erroneous-gpt-partition-tables"></a>Puede dar lugar a VHDX la reducción o expansión de archivos VHD y VHDX en tablas de partición GPT erróneas

Hyper-V permite reducir los archivos de disco virtual (VHDX) sin tener en cuenta cualquier partición, volumen o estructuras de datos de sistema de archivos que puedan existir en el disco. Si el VHDX se reduce hasta donde entra el final de VHDX antes del final de una partición, se pueden perder datos, se puede devolver que partición puede convertirse en datos dañados o no es válidos cuando se lee la partición.

Después de cambiar el tamaño de un VHD o VHDX, los administradores deben usar una utilidad como fdisk o divididas para actualizar la partición, el volumen y las estructuras de sistema de archivos para reflejar el cambio en el tamaño del disco. Reducir o expandir el tamaño de un VHD o VHDX que tiene una tabla de particiones GUID (GPT) provocará una advertencia cuando se usa una herramienta de administración de particiones para comprobar el diseño de partición y el administrador se le avisará para corregir los encabezados GPT primera y la secundarios. Este paso manual es seguro realizar sin pérdida de datos.

## <a name="see-also"></a>Vea también

* [Linux y FreeBSD las máquinas virtuales compatibles para Hyper-V en Windows](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

* [Procedimientos recomendados para ejecutar FreeBSD en Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)

* [Implementar un clúster de Hyper-V](https://technet.microsoft.com/library/jj863389.aspx)

* [Creación de imágenes de Linux para Azure](https://docs.microsoft.com/azure/virtual-machines/linux/create-upload-generic)

* [Optimizar la máquina virtual Linux en Azure](https://docs.microsoft.com/azure/virtual-machines/linux/optimization)
