---
title: Implementación de dispositivos de gráficos mediante la asignación discreta de dispositivos
description: Obtenga información sobre cómo usar DDA para implementar dispositivos gráficos en Windows Server
ms.prod: windows-server-threshold
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 67a01889-fa36-4bc6-841d-363d76df6a66
ms.openlocfilehash: 9e9a36df39c7bd7a96cc8c5681e83bf263ee5f8e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833876"
---
# <a name="deploy-graphics-devices-using-discrete-device-assignment"></a>Implementación de dispositivos de gráficos mediante la asignación discreta de dispositivos

>Se aplica a: Microsoft Hyper-V Server 2016, Windows Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019  

A partir de Windows Server 2016, puede usar la asignación de dispositivo discretos o DDA, pasar toda una matriz de dispositivos de PCIe en una máquina virtual.  Esto permitirá el acceso de alto rendimiento con dispositivos como [NVMe almacenamiento](./Deploying-storage-devices-using-dda.md) o tarjetas gráficas desde dentro de una máquina virtual mientras se pueden aprovechar los controladores de dispositivos nativos.  Visite el [planear la implementación de dispositivos mediante la asignación discreta de dispositivos](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) para obtener más detalles sobre qué dispositivos de trabajo, ¿cuáles son las posibles implicaciones de seguridad, etcetera.

Hay tres pasos para usar un dispositivo con la asignación discreta de dispositivos:
-   Configurar la máquina virtual para la asignación discreta de dispositivos
-   Desmonte el dispositivo de la partición del Host
-   Asignar el dispositivo a la máquina virtual invitada

Todos los comandos se pueden ejecutar en el Host en una consola de Windows PowerShell como administrador.

## <a name="configure-the-vm-for-dda"></a>Configurar la máquina virtual para DDA
Asignación discreta de dispositivos impone algunas restricciones a las máquinas virtuales y es necesario realizar el paso siguiente.

1.  Configure el "Detener acción automática" de una máquina virtual para la desactivación mediante la ejecución

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

### <a name="some-additional-vm-preparation-is-required-for-graphics-devices"></a>Algunos preparativos de la máquina virtual adicional es necesaria para los dispositivos de gráficos

Algunos tipos de hardware funciona mejor si la máquina virtual en configurado de una manera determinada.  Para obtener más información sobre si necesita o no las siguientes configuraciones para el hardware, póngase en contacto con el fabricante del hardware. Pueden encontrar detalles adicionales en [planear la implementación de dispositivos mediante la asignación discreta de dispositivos](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) y sobre esta [entrada de blog.](https://blogs.technet.microsoft.com/virtualization/2015/11/23/discrete-device-assignment-gpus/)

1.  Habilitar la combinación de escritura en la CPU
```
Set-VM -GuestControlledCacheTypes $true -VMName VMName
```
2.  Configure el espacio MMIO de 32 bits
```
Set-VM -LowMemoryMappedIoSpace 3Gb -VMName VMName
```
3.  Configurar el mayor espacio MMIO de 32 bits
```
Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName VMName
```
Tenga en cuenta que los valores de espacio MMIO indicados anteriormente son valores razonables para establecer para experimentar con una sola GPU.  Si después de iniciar la máquina virtual, el dispositivo notifica un error relacionado con recursos insuficientes, es probable que deba modificar estos valores.  Además, si va a asignar varias GPU, necesita aumentar estos valores también.

## <a name="dismount-the-device-from-the-host-partition"></a>Desmonte el dispositivo de la partición del Host
### <a name="optional---install-the-partitioning-driver"></a>Opcional: instalar el controlador de partición
Asignación discreta de dispositivos proporcionan venders hardware la capacidad para proporcionar un controlador de mitigación de seguridad con sus dispositivos.  Tenga en cuenta que este controlador no es el mismo que el controlador de dispositivo que se instalará en la máquina virtual invitada.  Ha hasta discreción del fabricante del hardware para proporcionar este controlador, sin embargo, si se proporcionan, instálelo antes para desmontar el dispositivo desde la partición del host.  Póngase en contacto con el proveedor de hardware para obtener más información sobre si tienen un controlador de mitigación
> Si no se proporciona ningún controlador de creación de particiones, durante el desmontaje debe usar el `-force` opción para omitir la advertencia de seguridad. Lea más acerca de las implicaciones de seguridad de hacerlo en [planear la implementación de dispositivos mediante la asignación discreta de dispositivos](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md).

### <a name="locating-the-devices-location-path"></a>Buscar la ruta de acceso de ubicación del dispositivo
Se requiere la ruta de acceso de ubicación de PCI para desmontar y montar el dispositivo desde el Host.  Una ruta de acceso de ubicación de ejemplo es similar al siguiente: `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`.  Obtener más detalles sobre encuentra la ruta de acceso de ubicación puede encontrarse aquí: [Planear la implementación de dispositivos mediante la asignación discreta de dispositivos](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md).

### <a name="disable-the-device"></a>Deshabilitar el dispositivo
Mediante el Administrador de dispositivos o PowerShell, asegúrese del dispositivo es "deshabilitado".  

### <a name="dismount-the-device"></a>Desmonte el dispositivo
Dependiendo de si el proveedor proporciona un controlador de mitigación, o bien deberá usar el "-forzar" opción o no.
-   Si se instaló un controlador de mitigación
```
Dismount-VMHostAssignableDevice -LocationPath $locationPath
```
-   Si no se instaló un controlador de mitigación
```
Dismount-VMHostAssignableDevice -force -LocationPath $locationPath
```

## <a name="assigning-the-device-to-the-guest-vm"></a>Asignar el dispositivo a la máquina virtual invitada
El último paso es indicar a Hyper-V que una máquina virtual debe tener acceso al dispositivo.  Además de la ruta de acceso de ubicación que se encuentra por encima, necesita conocer el nombre de la máquina virtual.

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>Próxima Novedades
Después de un dispositivo se monta correctamente en una máquina virtual, ahora podemos iniciar esa máquina virtual e interactuar con el dispositivo como normalmente haría si estuviera ejecutando en un sistema sin sistema operativo.  Esto significa que ahora ya puede instalar los controladores del fabricante del Hardware en la máquina virtual y las aplicaciones podrán ver que presente el hardware.  Puede comprobarlo abriendo el Administrador de dispositivos en la máquina virtual invitada y ver que el hardware se muestra ahora.

## <a name="removing-a-device-and-returning-it-to-the-host"></a>Eliminación de un dispositivo y devolverlo al Host
Si desea devuelva dispositivo a su estado original, deberá detener la máquina virtual y emitir la siguiente:
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
A continuación, puede volver a habilitar el dispositivo en el Administrador de dispositivos y el sistema operativo host podrá interactuar con el dispositivo nuevo.

## <a name="examples"></a>Ejemplos

### <a name="mounting-a-gpu-to-a-vm"></a>Montaje de una GPU a una máquina virtual
En este ejemplo se usa PowerShell para configurar una máquina virtual denominada "ddatest1" para aprovechar la GPU primera por el fabricante de NVIDIA y asignarlo a la máquina virtual.  
```
#Configure the VM for a Discrete Device Assignment
$vm =   "ddatest1"
#Set automatic stop action to TurnOff
Set-VM -Name $vm -AutomaticStopAction TurnOff
#Enable Write-Combining on the CPU
Set-VM -GuestControlledCacheTypes $true -VMName $vm
#Configure 32 bit MMIO space
Set-VM -LowMemoryMappedIoSpace 3Gb -VMName $vm
#Configure Greater than 32 bit MMIO space
Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName $vm

#Find the Location Path and disable the Device
#Enumerate all PNP Devices on the system
$pnpdevs = Get-PnpDevice -presentOnly
#Select only those devices that are Display devices manufactured by NVIDIA
$gpudevs = $pnpdevs |where-object {$_.Class -like "Display" -and $_.Manufacturer -like "NVIDIA"}
#Select the location path of the first device that's available to be dismounted by the host.
$locationPath = ($gpudevs | Get-PnpDeviceProperty DEVPKEY_Device_LocationPaths).data[0]
#Disable the PNP Device
Disable-PnpDevice  -InstanceId $gpudevs[0].InstanceId

#Dismount the Device from the Host
Dismount-VMHostAssignableDevice -force -LocationPath $locationPath

#Assign the device to the guest VM.
Add-VMAssignableDevice -LocationPath $locationPath -VMName $vm
```
