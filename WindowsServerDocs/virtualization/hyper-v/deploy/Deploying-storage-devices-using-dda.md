---
title: Implementación de dispositivos de almacenamiento de NVMe mediante la asignación discreta de dispositivos
description: Obtenga información sobre cómo usar DDA para implementar los dispositivos de almacenamiento
ms.prod: windows-server-threshold
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 1c36107e-78c9-4ec0-a313-6ed557ac0ffc
ms.openlocfilehash: d6fe54789d37386d5dc782ef8a2ca26b47adc69e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841366"
---
# <a name="deploy-nvme-storage-devices-using-discrete-device-assignment"></a>Implementación de dispositivos de almacenamiento de NVMe mediante la asignación discreta de dispositivos

>Se aplica a: Microsoft Hyper-V Server 2016, Windows Server 2016

A partir de Windows Server 2016, puede usar la asignación de dispositivo discretos o DDA, pasar toda una matriz de dispositivos de PCIe en una máquina virtual.  Esto permitirá el acceso de alto rendimiento con dispositivos como almacenamiento NVMe o tarjetas gráficas desde dentro de una máquina virtual mientras se pueden aprovechar los controladores de dispositivos nativos.  Visite el [planear la implementación de dispositivos mediante la asignación discreta de dispositivos](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) para obtener más detalles sobre qué dispositivos de trabajo, ¿cuáles son las posibles implicaciones de seguridad, etcetera. Hay tres pasos para usar un dispositivo con DDA:
-   Configurar la máquina virtual para DDA
-   Desmonte el dispositivo de la partición del Host
-   Asignar el dispositivo a la máquina virtual invitada

Todos los comandos se pueden ejecutar en el Host en una consola de Windows PowerShell como administrador.

## <a name="configure-the-vm-for-dda"></a>Configurar la máquina virtual para DDA
Asignación discreta de dispositivos impone algunas restricciones a las máquinas virtuales y es necesario realizar el paso siguiente.

1.  Configure el "Detener acción automática" de una máquina virtual para la desactivación mediante la ejecución

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

## <a name="dismount-the-device-from-the-host-partition"></a>Desmonte el dispositivo de la partición del Host

### <a name="locating-the-devices-location-path"></a>Buscar la ruta de acceso de ubicación del dispositivo
Se requiere la ruta de acceso de ubicación de PCI para desmontar y montar el dispositivo desde el Host.  Una ruta de acceso de ubicación de ejemplo es similar al siguiente: `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`.   Obtener más detalles sobre encuentra la ruta de acceso de ubicación puede encontrarse aquí: [Planear la implementación de dispositivos mediante la asignación discreta de dispositivos](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md).

### <a name="disable-the-device"></a>Deshabilitar el dispositivo
Mediante el Administrador de dispositivos o PowerShell, asegúrese del dispositivo es "deshabilitado".  

### <a name="dismount-the-device"></a>Desmonte el dispositivo
```
Dismount-VMHostAssignableDevice -LocationPath $locationPath
```

## <a name="assigning-the-device-to-the-guest-vm"></a>Asignar el dispositivo a la máquina virtual invitada
El último paso es indicar a Hyper-V que una máquina virtual debe tener acceso al dispositivo.  Además de la ruta de acceso de ubicación que se encuentra por encima, necesita conocer el nombre de la máquina virtual.

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>Próxima Novedades
Después de un dispositivo se monta correctamente en una máquina virtual, ahora podemos iniciar esa máquina virtual e interactuar con el dispositivo como normalmente haría si estuviera ejecutando en un sistema sin sistema operativo.  Puede comprobarlo abriendo el Administrador de dispositivos en la máquina virtual invitada y ver que el hardware se muestra ahora.

## <a name="removing-a-device-and-returning-it-to-the-host"></a>Eliminación de un dispositivo y devolverlo al Host
Si desea devuelva dispositivo a su estado original, deberá detener la máquina virtual y emitir la siguiente:
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
A continuación, puede volver a habilitar el dispositivo en el Administrador de dispositivos y el sistema operativo host podrá interactuar con el dispositivo nuevo.
