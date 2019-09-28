---
title: Prácticas recomendadas para ejecutar FreeBSD en Hyper-V
description: Proporciona recomendaciones para ejecutar FreeBSD en máquinas virtuales
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c66f1c8-2606-43a3-b4cc-166acaaf2d2a
author: shirgall
ms.author: kathydav
ms.date: 01/09/2017
ms.openlocfilehash: 1d284b38e1bdb642aa40ecbb8e82caa7712f7aad
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365632"
---
# <a name="best-practices-for-running-freebsd-on-hyper-v"></a>Prácticas recomendadas para ejecutar FreeBSD en Hyper-V

>Se aplica a: Windows Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

Este tema contiene una lista de recomendaciones para ejecutar FreeBSD como sistema operativo invitado en una máquina virtual de Hyper-V.

## <a name="enable-carp-in-freebsd-102-on-hyper-v"></a>Habilitación de CARP en FreeBSD 10,2 en Hyper-V

El protocolo de redundancia de direcciones comunes (CARP) permite que varios hosts compartan la misma dirección IP y el identificador de host virtual (VHID) para ayudar a proporcionar alta disponibilidad para uno o más servicios. Si se produce un error en uno o más hosts, los demás hosts se toman de forma transparente para que los usuarios no perciban un error del servicio. Para usar CARP en FreeBSD 10,2, siga las instrucciones del [manual de FreeBSD](https://www.freebsd.org/doc/en/books/handbook/carp.html) y haga lo siguiente en el administrador de Hyper-V.

* Compruebe que la máquina virtual tiene un adaptador de red y que tiene asignado un conmutador virtual. Seleccione la máquina virtual y seleccione **acciones** > **configuración**.

![Captura de pantalla de la configuración de la máquina virtual con el adaptador de red seleccionado](media/Hyper-V_Settings_NetworkAdapter.png)

* Habilite la suplantación de direcciones MAC. Para ello,

   1. Seleccione la máquina virtual y seleccione **acciones** > **configuración**.

   2. Expanda **adaptador de red** y seleccione **características avanzadas**.

   3. Seleccione **Habilitar la suplantación de direcciones MAC**.

## <a name="create-labels-for-disk-devices"></a>Crear etiquetas para dispositivos de disco

Durante el inicio, los nodos de dispositivo se crean a medida que se detectan nuevos dispositivos. Esto puede significar que los nombres de los dispositivos pueden cambiar cuando se agregan nuevos dispositivos. Si obtiene un ERROR de montaje raíz durante el inicio, debe crear etiquetas para cada partición IDE para evitar conflictos y cambios. Para obtener información sobre cómo hacerlo, consulte [etiquetado de dispositivos de disco](https://www.freebsd.org/doc/handbook/geom-glabel.html). A continuación se muestran ejemplos. 

> [!IMPORTANT]
> Realice una copia de seguridad de su fstab antes de realizar cualquier cambio.

1. Reinicie el sistema en modo de usuario único. Esto puede realizarse seleccionando la opción 2 del menú de arranque de FreeBSD 10.3 + (opción 4 para FreeBSD 8. x) o realizando un "arranque" desde el símbolo del sistema de arranque.

2. En el modo de usuario único, cree etiquetas GEOM para cada una de las particiones de disco IDE enumeradas en el fstab (tanto raíz como de intercambio). A continuación se muestra un ejemplo de FreeBSD 10,3.

   ```bash
   # cat  /etc/fstab
   # Device           Mountpoint      FStype  Options   Dump   Pass#
   /dev/da0p2         /               ufs     rw        1       1
   /dev/da0p3         none            swap    sw        0       0

   # glabel  label rootfs  /dev/da0p2
   # glabel  label swap   /dev/da0p3
   # exit
   ```

   Puede encontrar información adicional sobre las etiquetas GEOM en: [Etiquetar dispositivos de disco](https://www.freebsd.org/doc/handbook/geom-glabel.html).

3. El sistema continuará con el arranque de varios usuarios. Una vez completado el arranque, edite/etc/fstab y reemplace los nombres de dispositivo convencionales con sus etiquetas correspondientes. El último/etc/fstab tendrá el siguiente aspecto:

   ```
   # Device                Mountpoint      FStype  Options         Dump    Pass#
   /dev/label/rootfs       /               ufs     rw              1       1
   /dev/label/swap         none            swap    sw              0       0
   ```

4. Ahora se puede reiniciar el sistema. Si todo ha ido bien, se mostrará normalmente y el montaje mostrará:

   ```
   # mount
   /dev/label/rootfs on / (ufs, local, journaled soft-updates)
   devfs on /dev (devfs, local, mutilabel)
   ```

## <a name="use-a-wireless-network-adapter-as-the-virtual-switch"></a>Usar un adaptador de red inalámbrica como conmutador virtual

Si el conmutador virtual del host se basa en el adaptador de red inalámbrica, reduzca el tiempo de expiración de ARP a 60 segundos con el siguiente comando. De lo contrario, las redes de la máquina virtual pueden dejar de funcionar después de un tiempo.


```
   # sysctl net.link.ether.inet.max_age=60
```


Vea también

* [Máquinas virtuales de FreeBSD compatibles en Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)
