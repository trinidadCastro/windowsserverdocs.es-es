---
title: Implementación de dispositivos de almacenamiento de NVMe mediante la asignación discreta de dispositivos
description: Obtenga información sobre cómo usar DDA para implementar dispositivos de almacenamiento
ms.prod: windows-server
ms.technology: hyper-v
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 1c36107e-78c9-4ec0-a313-6ed557ac0ffc
ms.openlocfilehash: 2b92b175a6e914b62b069f76f92255cb99d55d74
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860908"
---
# <a name="deploy-nvme-storage-devices-using-discrete-device-assignment"></a>Implementación de dispositivos de almacenamiento de NVMe mediante la asignación discreta de dispositivos

>Se aplica a: Microsoft Hyper-V Server 2016, Windows Server 2016

A partir de Windows Server 2016, puede usar la asignación discreta de dispositivos, o DDA, para pasar un dispositivo PCIe completo en una máquina virtual.  Esto permitirá el acceso de alto rendimiento a dispositivos como el almacenamiento de NVMe o tarjetas de gráficos desde una máquina virtual mientras se pueden aprovechar los controladores nativos de los dispositivos.  Visite el [plan para la implementación de dispositivos mediante la asignación discreta de dispositivos](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) para obtener más detalles sobre qué dispositivos funcionan, cuáles son las posibles implicaciones de seguridad, etc. Hay tres pasos para usar un dispositivo con DDA:
-   Configuración de la máquina virtual para DDA
-   Desmontar el dispositivo de la partición de host
-   Asignación del dispositivo a la máquina virtual invitada

El comando All puede ejecutarse en el host de una consola de Windows PowerShell como administrador.

## <a name="configure-the-vm-for-dda"></a>Configuración de la máquina virtual para DDA
La asignación discreta de dispositivos impone algunas restricciones a las máquinas virtuales y es necesario realizar el siguiente paso.

1.  Configurar la "acción de detención automática" de una máquina virtual para que se ejecute

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

## <a name="dismount-the-device-from-the-host-partition"></a>Desmontar el dispositivo de la partición de host

### <a name="locating-the-devices-location-path"></a>Buscar la ruta de acceso de la ubicación del dispositivo
La ruta de acceso de la ubicación de PCI es necesaria para desmontar y montar el dispositivo del host.  Una ruta de acceso de ubicación de ejemplo tiene un aspecto similar al siguiente: `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`.   Aquí encontrará más detalles sobre cómo encontrar la ruta de acceso de Ubicación: [planear la implementación de dispositivos mediante la asignación discreta de dispositivos](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md).

### <a name="disable-the-device"></a>Deshabilitar el dispositivo
Con Device Manager o PowerShell, asegúrese de que el dispositivo está "deshabilitado".  

### <a name="dismount-the-device"></a>Desmontar el dispositivo
```
Dismount-VMHostAssignableDevice -LocationPath $locationPath
```

## <a name="assigning-the-device-to-the-guest-vm"></a>Asignación del dispositivo a la máquina virtual invitada
El paso final consiste en indicar a Hyper-V que una máquina virtual debe tener acceso al dispositivo.  Además de la ruta de acceso de ubicación que se encuentra arriba, deberá conocer el nombre de la máquina virtual.

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>¿Qué debe hacer a continuación?
Una vez que un dispositivo se monta correctamente en una máquina virtual, ahora puede iniciar la máquina virtual e interactuar con el dispositivo como lo haría normalmente si estuviera ejecutando en un sistema sin sistema operativo.  Para comprobarlo, abra el administrador de dispositivos en la máquina virtual invitada y vea que el hardware se muestra ahora.

## <a name="removing-a-device-and-returning-it-to-the-host"></a>Quitar un dispositivo y devolverlo al host
Si desea devolver el dispositivo a su estado original, tendrá que detener la máquina virtual y emitir lo siguiente:
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
Después, puede volver a habilitar el dispositivo en el administrador de dispositivos y el sistema operativo host podrá interactuar con el dispositivo de nuevo.
