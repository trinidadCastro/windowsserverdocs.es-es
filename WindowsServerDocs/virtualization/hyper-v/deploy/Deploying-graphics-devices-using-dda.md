---
title: Implementación de dispositivos de gráficos mediante la asignación discreta de dispositivos
description: Obtenga información acerca de cómo usar DDA para implementar dispositivos gráficos en Windows Server
ms.prod: windows-server
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 67a01889-fa36-4bc6-841d-363d76df6a66
ms.date: 08/21/2019
ms.openlocfilehash: 5466cecf9f11a53dc6e205f36d50d7b27b310ea1
ms.sourcegitcommit: 81198fbf9e46830b7f77dcd345b02abb71ae0ac2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2019
ms.locfileid: "72923877"
---
# <a name="deploy-graphics-devices-using-discrete-device-assignment"></a>Implementación de dispositivos de gráficos mediante la asignación discreta de dispositivos

> Se aplica a: Microsoft Hyper-V Server 2016, Windows Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019  

A partir de Windows Server 2016, puede usar la asignación discreta de dispositivos, o DDA, para pasar un dispositivo PCIe completo en una máquina virtual.  Esto permitirá el acceso de alto rendimiento a dispositivos como el [almacenamiento de NVMe](./Deploying-storage-devices-using-dda.md) o tarjetas de gráficos desde una máquina virtual mientras se pueden aprovechar los controladores nativos de los dispositivos.  Visite el [plan para la implementación de dispositivos mediante la asignación discreta de dispositivos](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) para obtener más detalles sobre qué dispositivos funcionan, cuáles son las posibles implicaciones de seguridad, etc.

Hay tres pasos para usar un dispositivo con asignación discreta de dispositivos:
-   Configuración de la máquina virtual para la asignación discreta de dispositivos
-   Desmontar el dispositivo de la partición de host
-   Asignación del dispositivo a la máquina virtual invitada

El comando All puede ejecutarse en el host de una consola de Windows PowerShell como administrador.

## <a name="configure-the-vm-for-dda"></a>Configuración de la máquina virtual para DDA
La asignación discreta de dispositivos impone algunas restricciones a las máquinas virtuales y es necesario realizar el siguiente paso.

1.  Configurar la "acción de detención automática" de una máquina virtual para que se ejecute

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

### <a name="some-additional-vm-preparation-is-required-for-graphics-devices"></a>Se requiere una preparación adicional de la máquina virtual para los dispositivos gráficos

Cierto hardware funciona mejor si la máquina virtual está configurada de una manera determinada.  Para obtener más información sobre si necesita o no las siguientes configuraciones para el hardware, póngase en contacto con el proveedor de hardware. Encontrará más detalles en el [plan para la implementación de dispositivos mediante la asignación discreta de dispositivos](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) y en esta [entrada de blog.](https://techcommunity.microsoft.com/t5/Virtualization/Discrete-Device-Assignment-GPUs/ba-p/382266)

1. Habilitar la combinación de escritura en la CPU
   ```
   Set-VM -GuestControlledCacheTypes $true -VMName VMName
   ```
2. Configurar el espacio MMIO de 32 bits
   ```
   Set-VM -LowMemoryMappedIoSpace 3Gb -VMName VMName
   ```
3. Configurar un espacio de enmmio de más de 32 bits
   ```
   Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName VMName
   ```
   > [!TIP] 
   > Los valores de espacio MMIO anteriores son valores razonables que se establecen para experimentar con una sola GPU.  Si después de iniciar la máquina virtual, el dispositivo informa de un error relacionado con no hay suficientes recursos, es probable que tenga que modificar estos valores. Consulte [planeación de la implementación de dispositivos mediante la asignación discreta de dispositivos](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) para obtener información sobre cómo calcular con precisión los requisitos de MMIO.

## <a name="dismount-the-device-from-the-host-partition"></a>Desmontar el dispositivo de la partición de host
### <a name="optional---install-the-partitioning-driver"></a>Opcional: instalar el controlador de particionamiento
La asignación discreta de dispositivos proporciona a los expendedores de hardware la capacidad de proporcionar un controlador de mitigación de seguridad con sus dispositivos.  Tenga en cuenta que este controlador no es el mismo que el controlador de dispositivo que se instalará en la máquina virtual invitada.  Es responsabilidad del proveedor de hardware proporcionar este controlador; sin embargo, si lo proporcionan, instálelo antes de desmontar el dispositivo de la partición del host...  Póngase en contacto con el proveedor de hardware para obtener más información sobre si tienen un controlador de mitigación.
> Si no se proporciona ningún controlador de particionamiento, durante el desmontaje debe utilizar la opción `-force` para omitir la advertencia de seguridad. Lea más sobre las implicaciones de seguridad de hacerlo en [planear la implementación de dispositivos mediante la asignación discreta de dispositivos](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md).

### <a name="locating-the-devices-location-path"></a>Buscar la ruta de acceso de la ubicación del dispositivo
La ruta de acceso de la ubicación de PCI es necesaria para desmontar y montar el dispositivo del host.  Una ruta de acceso de ubicación de ejemplo tiene un aspecto similar al siguiente: `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`.  Aquí encontrará más detalles sobre cómo encontrar la ruta de acceso de Ubicación: [planear la implementación de dispositivos mediante la asignación discreta de dispositivos](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md).

### <a name="disable-the-device"></a>Deshabilitar el dispositivo
Con Device Manager o PowerShell, asegúrese de que el dispositivo está "deshabilitado".  

### <a name="dismount-the-device"></a>Desmontar el dispositivo
Dependiendo de si el proveedor proporcionó un controlador de mitigación, deberá usar la opción "-Force" o no.
- Si se instaló un controlador de mitigación
  ```
  Dismount-VMHostAssignableDevice -LocationPath $locationPath
  ```
- Si no se instaló un controlador de mitigación
  ```
  Dismount-VMHostAssignableDevice -force -LocationPath $locationPath
  ```

## <a name="assigning-the-device-to-the-guest-vm"></a>Asignación del dispositivo a la máquina virtual invitada
El paso final consiste en indicar a Hyper-V que una máquina virtual debe tener acceso al dispositivo.  Además de la ruta de acceso de ubicación que se encuentra arriba, deberá conocer el nombre de la máquina virtual.

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>Pasos siguientes
Una vez que un dispositivo se monta correctamente en una máquina virtual, ahora puede iniciar la máquina virtual e interactuar con el dispositivo como lo haría normalmente si estuviera ejecutando en un sistema sin sistema operativo.  Esto significa que ahora puede instalar los controladores del proveedor de hardware en la máquina virtual y las aplicaciones podrán ver que el hardware está presente.  Para comprobarlo, abra el administrador de dispositivos en la máquina virtual invitada y vea que el hardware se muestra ahora.

## <a name="removing-a-device-and-returning-it-to-the-host"></a>Quitar un dispositivo y devolverlo al host
Si desea devolver el dispositivo a su estado original, tendrá que detener la máquina virtual y emitir lo siguiente:
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
Después, puede volver a habilitar el dispositivo en el administrador de dispositivos y el sistema operativo host podrá interactuar con el dispositivo de nuevo.

## <a name="example"></a>Ejemplo

### <a name="mounting-a-gpu-to-a-vm"></a>Montaje de una GPU en una máquina virtual
En este ejemplo, se usa PowerShell para configurar una máquina virtual denominada "ddatest1" para que la primera GPU esté disponible por el fabricante NVIDIA y asignarla a la máquina virtual.  
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

## <a name="troubleshooting"></a>de solución de problemas

Si ha pasado una GPU a una máquina virtual pero Escritorio remoto o una aplicación no reconoce la GPU, busque los siguientes problemas comunes:

- Asegúrese de que ha instalado la versión más reciente del controlador compatible con el proveedor de GPU y que el controlador no informa de los errores comprobando el estado del dispositivo en Device Manager.
- Asegúrese de que el dispositivo tiene suficiente espacio MMIO asignado dentro de la máquina virtual. Para obtener más información, vea [espacio de MMIO](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md#mmio-space).
- Asegúrese de que está usando una GPU que el proveedor admite en esta configuración. Por ejemplo, algunos proveedores impiden que sus tarjetas de consumidor funcionen cuando pasan a una máquina virtual.
- Asegúrese de que la aplicación que se ejecuta admite la ejecución dentro de una máquina virtual y que la aplicación admite tanto la GPU como sus controladores asociados. Algunas aplicaciones tienen listas de permitidos de GPU y entornos.
- Si usa el rol host de sesión de Escritorio remoto o Windows MultiPoint Services en el invitado, deberá asegurarse de que una entrada de directiva de grupo específica está establecida para permitir el uso de la GPU predeterminada. Con un objeto de directiva de grupo aplicado al invitado (o al Editor de directivas de grupo local en el invitado), navegue hasta el siguiente elemento de directiva de grupo: **configuración del equipo** > plantillas de **Administrador** > componentes de **Windows** > **Servicios de Escritorio remoto** > **escritorio remoto Host de sesión** > **entorno de sesión remota** > **usar el adaptador de gráficos predeterminado de hardware para todas las sesiones de servicios de escritorio remoto**. Establezca este valor en habilitado y, a continuación, reinicie la máquina virtual una vez que se haya aplicado la Directiva.
