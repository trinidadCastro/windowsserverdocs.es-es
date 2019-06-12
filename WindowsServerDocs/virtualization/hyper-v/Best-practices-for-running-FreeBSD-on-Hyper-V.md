---
title: Procedimientos recomendados para ejecutar FreeBSD en Hyper-V
description: Proporciona recomendaciones para la ejecución de FreeBSD en máquinas virtuales
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c66f1c8-2606-43a3-b4cc-166acaaf2d2a
author: shirgall
ms.author: kathydav
ms.date: 01/09/2017
ms.openlocfilehash: 6320ceb86093146592a54ab34b013f334f43ddb4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447776"
---
# <a name="best-practices-for-running-freebsd-on-hyper-v"></a>Procedimientos recomendados para ejecutar FreeBSD en Hyper-V

>Se aplica a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

Este tema contiene una lista de recomendaciones para ejecutar FreeBSD como un sistema operativo invitado en una máquina virtual de Hyper-V.

## <a name="enable-carp-in-freebsd-102-on-hyper-v"></a>Habilitar CARP en FreeBSD 10.2 en Hyper-V

El protocolo de redundancia de dirección común (CARP) permite que varios hosts que comparten la misma dirección IP y el Id. de Host Virtual (VHID) para ayudar a proporcionar alta disponibilidad para uno o varios servicios. Si se producirá un error en uno o más hosts, los demás hosts se ocupará transparente para que usuarios no notarán un error del servicio. Para usar CARP en FreeBSD 10.2, siga las instrucciones de la [manual de FreeBSD](https://www.freebsd.org/doc/en/books/handbook/carp.html) y realice lo siguiente en el Administrador de Hyper-V.

* Compruebe la máquina virtual tiene un adaptador de red y se ha asignado un conmutador virtual. Seleccione la máquina virtual y seleccione **acciones** > **configuración**.

![Captura de pantalla de configuración de máquina virtual con el adaptador de red seleccionado](media/Hyper-V_Settings_NetworkAdapter.png)

* Habilitar la suplantación de direcciones MAC. Para ello,

   1. Seleccione la máquina virtual y seleccione **acciones** > **configuración**.

   2. Expanda **adaptador de red** y seleccione **características avanzadas**.

   3. Seleccione **la suplantación de direcciones de MAC habilitar**.

## <a name="create-labels-for-disk-devices"></a>Crear etiquetas para los dispositivos de disco

Durante el inicio, se crean nodos de dispositivo tal como se detectan nuevos dispositivos. Esto puede significar que los nombres de dispositivo pueden cambiar cuando se agregan nuevos dispositivos. Si recibe un ERROR de montaje de raíz durante el inicio, debe crear etiquetas para cada partición IDE evitar conflictos y cambios. Para obtener información sobre cómo hacerlo, consulte [etiquetado de dispositivos de disco](https://www.freebsd.org/doc/handbook/geom-glabel.html). A continuación se muestran algunos ejemplos. 

> [!IMPORTANT]
> Realizar una copia de seguridad de su fstab antes de realizar cambios.

1. Reinicie el sistema en modo de usuario único. Esto puede realizarse mediante la selección de la opción de menú de arranque 2 para FreeBSD 10.3 y versiones posteriores (opción 4 para FreeBSD 8.x), o la realización de un '-s de arranque' desde el símbolo del sistema de arranque.

2. En modo de usuario único, crear etiquetas GEOM para cada una de las particiones de disco IDE aparece en su fstab (raíz e intercambio). A continuación es un ejemplo de FreeBSD 10.3.

   ```bash
   # cat  /etc/fstab
   # Device           Mountpoint      FStype  Options   Dump   Pass#
   /dev/da0p2         /               ufs     rw        1       1
   /dev/da0p3         none            swap    sw        0       0

   # glabel  label rootfs  /dev/da0p2
   # glabel  label swap   /dev/da0p3
   # exit
   ```

   Obtener más información sobre las etiquetas GEOM puede encontrarse en: [Etiquetado de dispositivos de disco](https://www.freebsd.org/doc/handbook/geom-glabel.html).

3. El sistema continuará con el arranque de varios usuario. Una vez completado el arranque, editar/etc/fstab y reemplace los nombres de dispositivo convencionales, por sus respectivas etiquetas. El último/de etc/fstab tendrá este aspecto:

   ```
   # Device                Mountpoint      FStype  Options         Dump    Pass#
   /dev/label/rootfs       /               ufs     rw              1       1
   /dev/label/swap         none            swap    sw              0       0
   ```

4. Ahora se puede reiniciar el sistema. Si todo ha ido bien, se iniciará con normalidad y mostrará el montaje:

   ```
   # mount
   /dev/label/rootfs on / (ufs, local, journaled soft-updates)
   devfs on /dev (devfs, local, mutilabel)
   ```

## <a name="use-a-wireless-network-adapter-as-the-virtual-switch"></a>Usar un adaptador de red inalámbrica como el conmutador virtual

Si el conmutador virtual en el host se basa en el adaptador de red inalámbrica, reducir el tiempo de expiración de ARP en 60 segundos mediante el comando siguiente. En caso contrario, las funciones de red de la máquina virtual pueden dejar de funcionar después de unos minutos.


```
   # sysctl net.link.ether.inet.max_age=60
```


Vea también

* [Máquinas de virtuales de FreeBSD compatibles en Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)
