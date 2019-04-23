---
title: Planear la implementación de dispositivos mediante la asignación discreta de dispositivos
description: Obtenga información sobre cómo funciona la DDA en Windows Server
ms.prod: windows-server-threshold
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.date: 02/06/2018
ms.openlocfilehash: c64c2b75c00f97622278c3e590db46995e108218
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840206"
---
# <a name="plan-for-deploying-devices-using-discrete-device-assignment"></a>Planear la implementación de dispositivos mediante la asignación discreta de dispositivos
>Se aplica a: Microsoft Hyper-V Server 2016, Windows Server 2016, Microsoft Hyper-V Server, Windows Server 2019 de 2019

Asignación discreta de dispositivos permite hardware físico de PCIe ser accesible directamente desde dentro de una máquina virtual.  Esta guía describirá el tipo de dispositivos que puede utilizar la asignación de dispositivo discretos, requisitos del sistema host, han impuesto limitaciones en las máquinas virtuales, así como las implicaciones de seguridad de la asignación discreta de dispositivos.

Versión inicial de la asignación dispositivo discretos, nos hemos centrado en dos clases de dispositivos que formalmente sea compatible con Microsoft: Adaptadores de gráficos y el almacenamiento de NVMe dispositivos.  Otros dispositivos son más probabilidades de funcionar y los proveedores de hardware pueden ofrecer instrucciones de compatibilidad para los dispositivos.  Para estos otros dispositivos, póngase en contacto con los proveedores de hardware para obtener soporte técnico.

Si está listo para probar la asignación discreta de dispositivos, también puede avanzar a través a [implementar gráficos dispositivos utilizando discretos dispositivo asignación](../deploy/Deploying-graphics-devices-using-dda.md) o [implementar dispositivos de almacenamiento mediante la asignación discreta de dispositivos](../deploy/Deploying-storage-devices-using-dda.md) Para empezar a trabajar.

## <a name="supported-virtual-machines-and-guest-operating-systems"></a>Las máquinas virtuales compatibles y los sistemas operativos invitados
Se admite la asignación de dispositivo discretos para la generación 1 o 2 máquinas virtuales.  Además, los invitados compatibles incluyen Windows 10, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 con [KB 3133690](https://support.microsoft.com/kb/3133690) aplicado y varias distribuciones de la [del sistema operativo Linux.](../supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)

## <a name="system-requirements"></a>Requisitos del sistema
Además el [requisitos del sistema para Windows Server](../../../get-started/System-Requirements--and-Installation.md) y [requisitos del sistema para Hyper-V](../System-requirements-for-Hyper-V-on-Windows.md), asignación discreta de dispositivos requiere hardware de clase de servidor que es capaz de conceder la control del sistema operativo a través de configuración del tejido PCIe (Control nativo de Express de PCI). Además, la compleja de PCIe raíz debe ser compatible con "Access Control Services" (ACS), lo que permite a Hyper-V forzar que todo el tráfico a través de la E/S de MMU PCIe.

Normalmente estas funcionalidades no se exponen directamente en el BIOS del servidor y a menudo están ocultos detrás de otros valores de configuración.  Por ejemplo, las mismas funcionalidades son necesarias para el soporte de SR-IOV y en el BIOS es posible que deba establecer "Habilitar SR-IOV."  Póngase en contacto con el proveedor del sistema si no puede identificar la configuración correcta en el BIOS.

Para ayudar a garantizar que el hardware de hardware es capaz de asignación discreta de dispositivos, nuestros ingenieros han reunido una [Script de perfil de equipo](#machine-profile-script) que puede ejecutar en un host de Hyper-V habilitado para probar si el servidor está correctamente el programa de instalación y qué los dispositivos son capaces de asignación discreta de dispositivos.

## <a name="device-requirements"></a>Requisitos del dispositivo
No todos los dispositivos PCIe pueden utilizarse con asignación discreta de dispositivos.  Por ejemplo, no se admiten dispositivos antiguos que aprovechan las interrupciones de PCI (INTx) heredado. De Jake Oshin [entradas de blog](https://blogs.technet.microsoft.com/virtualization/2015/11/20/discrete-device-assignment-machines-and-devices/) vaya con más detalle: sin embargo, para el consumidor, ejecutando el [Script de perfil de equipo](#machine-profile-script) mostrará qué dispositivos son capaces de que se usa para la asignación de dispositivo discretos.

Fabricantes de dispositivos pueden establecer contacto con su representante de Microsoft para obtener más detalles.

## <a name="device-driver"></a>Controlador de dispositivo
Como asignación discreta de dispositivo pasa todo el dispositivo PCIe en la máquina virtual invitada, un controlador de host no es necesario para instalarse antes que el dispositivo que se va a montar dentro de la máquina virtual.  El único requisito en el host es que el dispositivo [ruta de acceso de ubicación de PCIe](#pcie-location-path) puede determinarse.  Opcionalmente se puede instalar el controlador del dispositivo si esto ayuda a identificar el dispositivo.  Por ejemplo, una GPU sin su controlador de dispositivo instalado en el host puede aparecer como un dispositivo de representación básica de Microsoft.  Si está instalado el controlador de dispositivo, es probable que se mostrará su fabricante y modelo.

Una vez que el dispositivo se monta dentro del invitado, ahora se pueden instalar controladores de dispositivo del fabricante como lo haría normalmente dentro de la máquina virtual invitada.  

## <a name="virtual-machine-limitations"></a>Limitaciones de la máquina virtual
Dada la naturaleza de cómo se implementa la asignación discreta de dispositivos, algunas características de una máquina virtual están restringidos mientras esté conectado a un dispositivo.  No están disponibles las siguientes características:
- Guardar ni restaurar la máquina virtual
- Migración en vivo de una máquina virtual
- El uso de memoria dinámica
- Agregar la máquina virtual a un clúster de alta disponibilidad (HA)

## <a name="security"></a>Seguridad
Asignación discreta de dispositivo pasa todo el dispositivo a la máquina virtual.  Esto significa que todas las funcionalidades de dichos dispositivos son accesibles desde el sistema operativo invitado. Algunas capacidades, como la actualización de firmware, pueden afectar negativamente a la estabilidad del sistema. Por lo tanto, se presentan varias advertencias al administrador que al desmontar el dispositivo desde el host. Se recomienda encarecidamente que sólo se utiliza asignación discreta de dispositivos, donde los inquilinos de las máquinas virtuales son de confianza.  

Si el administrador desea usar un dispositivo con un inquilino de confianza, proporcionamos los fabricantes de dispositivos con la capacidad para crear un controlador de mitigación de dispositivo que se puede instalar en el host.  Póngase en contacto con el fabricante del dispositivo para obtener más información sobre si proporciona un controlador de mitigación de dispositivo.

Si desea omitir las comprobaciones de seguridad para un dispositivo que no tiene un controlador de mitigación de dispositivo, tendrá que pasar el `-Force` parámetro para el `Dismount-VMHostAssignableDevice` cmdlet.  Comprender que haciendo esto, ha cambiado el perfil de seguridad de dicho sistema y esto solo se recomienda durante la creación de prototipos o de confianza entornos.

## <a name="pcie-location-path"></a>Ruta de acceso de ubicación de PCIe
La ruta de acceso de ubicación de PCIe debe desmontar y montar el dispositivo desde el Host.  Una ruta de acceso de ubicación de ejemplo es similar al siguiente: `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`.   El [Script de perfil de equipo](#machine-profile-script) también devolverá la ruta de acceso de ubicación de los dispositivos de PCIe.

### <a name="getting-the-location-path-by-using-device-manager"></a>Obtener la ruta de acceso de ubicación mediante el Administrador de dispositivos
![Administrador de dispositivos](../deploy/media/dda-devicemanager.png)
- Abra el Administrador de dispositivos y busque el dispositivo.  
- A la derecha, haga clic en el dispositivo y seleccione "Propiedades".
- Vaya a la pestaña Detalles y seleccione "Rutas de acceso de ubicación" en la lista desplegable de la propiedad.  
- Haga clic en la entrada que comienza con "PCIROOT" y seleccione "Copiar".  Ahora tiene la ruta de acceso de ubicación para dicho dispositivo.

## <a name="mmio-space"></a>Espacio MMIO
Algunos dispositivos, especialmente las GPU, requieren espacio MMIO adicional que se asignará a la máquina virtual para la memoria de ese dispositivo sea accesible. De forma predeterminada, cada máquina virtual comienza con 128MB de espacio MMIO baja y 512MB de espacio MMIO alta asignados a él. Sin embargo, un dispositivo puede requerir más espacio MMIO, o se pueden pasar varios dispositivos a través de modo que los requisitos combinados superan estos valores.  Cambiar el espacio de MMIO es muy sencillo y se puede realizar en PowerShell mediante los comandos siguientes:

```
Set-VM -LowMemoryMappedIoSpace 3Gb -VMName $vm
Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName $vm
```
La manera más fácil de determinar cuánto espacio MMIO asignar consiste en usar el [Script de perfil de equipo](#machine-profile-script).  Como alternativa, puede calcular mediante el Administrador de dispositivos. Consulte el blog de TechNet registrar [asignación discreta de dispositivos - GPU](https://blogs.technet.microsoft.com/virtualization/2015/11/23/discrete-device-assignment-gpus/) para obtener más detalles.

## <a name="machine-profile-script"></a>Script de perfil de equipo
Con el fin de simplificar que identifica si el servidor está configurado correctamente y los dispositivos que están disponibles para pasarse a través de la asignación discreta de dispositivos, uno de nuestros ingenieros poner juntos el siguiente script de PowerShell: [SurveyDDA.ps1.](https://github.com/Microsoft/Virtualization-Documentation/blob/live/hyperv-tools/DiscreteDeviceAssignment/SurveyDDA.ps1)

Antes de usar el script, asegúrese de tener instalado el rol de Hyper-V y ejecute el script desde una ventana de comandos de PowerShell que tenga privilegios de administrador.

Si el sistema está configurado correctamente para admitir la asignación discreta de dispositivos, la herramienta mostrará un mensaje de error sobre cuál es el problema. Si la herramienta encuentra el sistema configurado correctamente, se enumerarán todos los dispositivos que puede encontrar en el PCIe Bus.

Para cada dispositivo que encuentra, la herramienta mostrará ya sea que puede usar con la asignación discreta de dispositivos. Si un dispositivo se identifica como compatible con la asignación discreta de dispositivos, la secuencia de comandos proporcionará una razón.  Cuando un dispositivo se identifica correctamente como compatible, se mostrará la ruta de acceso de ubicación del dispositivo.  Además, si requiere que el dispositivo [espacio MMIO](#mmio-space), se mostrará también.

![SurveyDDA.ps1](./images/hyper-v-surveydda-ps1.png)

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="how-does-remote-desktops-remotefx-vgpu-technology-relate-to-discrete-device-assignment"></a>¿Cómo se relacionan con tecnología de vGPU de RemoteFX del escritorio remoto para la asignación discreta de dispositivo?
Son tecnologías completamente independientes. RemoteFX vGPU no deben instalarse para que la asignación de dispositivo discretos para que funcione. Además, no hay roles adicionales se debe estar instalada. RemoteFX vGPU requiere el rol RDVH para instalarse en orden para el controlador de la vGPU de RemoteFX esté presente en la máquina virtual. Para la asignación de dispositivo discretos, puesto que va a instalar a los controladores del fabricante del Hardware en la máquina virtual, roles adicionales no deben estar presente.  

### <a name="ive-passed-a-gpu-into-a-vm-but-remote-desktop-or-an-application-isnt-recognizing-the-gpu"></a>He pasado una GPU en una máquina virtual, pero una aplicación o escritorio remoto no reconoce la GPU
Hay una serie de motivos, que esto podría suceder, pero varios problemas comunes se enumeran a continuación.
- Asegúrese de controlador del proveedor GPU más reciente está instalado y no notifica un error al comprobar el estado del dispositivo en el Administrador de dispositivos.
- Asegúrese de que el dispositivo tiene suficiente [espacio MMIO](#mmio-space) asignado para él en la máquina virtual.
- Asegúrese de que está usando una GPU compatibles con el proveedor que se va a usar en esta configuración. Por ejemplo, algunos proveedores impide sus tarjetas de consumidor funcionar cuando se pasa a una máquina virtual.
- Asegúrese de la aplicación que se ejecuta admite la ejecución dentro de una máquina virtual y que la GPU y sus controladores asociados son compatibles con la aplicación. Algunas aplicaciones tienen listas blancas de GPU y entornos.
- Si usa el rol de Host de sesión de escritorio remoto o Windows Multipoint Services en el invitado, deberá asegurarse de que se establece una entrada específica de la directiva de grupo para permitir el uso de la GPU de forma predeterminada. Mediante un objeto de directiva de grupo aplicado en el invitado (o el Editor de directivas de grupo Local en el invitado), vaya al siguiente elemento de directiva de grupo:
   - Configuración del equipo
   - Plantillas de administrador
   - Componentes de Windows
   - Servicios de Escritorio remoto
   - Host de sesión de Escritorio remoto
   - Entorno de la sesión remota
   - Utilice el adaptador de gráficos de hardware predeterminado para todas las sesiones de servicios de escritorio remoto

    Establecer este valor en habilitado, a continuación, reinicie la máquina virtual una vez que se ha aplicado la directiva.

### <a name="can-discrete-device-assignment-take-advantage-of-remote-desktops-avc444-codec"></a>¿Asignación discreta de dispositivos puede sacar partido de códec de AVC444 del escritorio remoto?
Sí, visite esta entrada de blog para obtener más información: [Mejoras de AVC/H.264 Desktop Protocol (RDP) 10 remotas en Windows 10 y Windows Server 2016 Technical Preview.](https://blogs.technet.microsoft.com/enterprisemobility/2016/01/11/remote-desktop-protocol-rdp-10-avch-264-improvements-in-windows-10-and-windows-server-2016-technical-preview/)

### <a name="can-i-use-powershell-to-get-the-location-path"></a>¿Puedo usar PowerShell para obtener la ruta de acceso de ubicación?
Sí, hay varias maneras de hacerlo. Este es un ejemplo:
```
#Enumerate all PNP Devices on the system
$pnpdevs = Get-PnpDevice -presentOnly
#Select only those devices that are Display devices manufactured by NVIDIA
$gpudevs = $pnpdevs |where-object {$_.Class -like "Display" -and $_.Manufacturer -like "NVIDIA"}
#Select the location path of the first device that's available to be dismounted by the host.
$locationPath = ($gpudevs | Get-PnpDeviceProperty DEVPKEY_Device_LocationPaths).data[0]
```

### <a name="can-discrete-device-assignment-be-used-to-pass-a-usb-device-into-a-vm"></a>¿Asignación discreta de dispositivos permite pasar un dispositivo USB en una máquina virtual?
Aunque oficialmente no se admite, nuestros clientes han usado asignación discreta de dispositivos para hacer esto al pasar todo el controlador USB3 a una máquina virtual.  Como se pasa en el controlador de todo, cada dispositivo USB conectado a ese controlador también serán accesible en la máquina virtual.  Tenga en cuenta que es posible que funcione solo algunos controladores USB3 y controladores USB 2 no se puede usar con la asignación discreta de dispositivos.
