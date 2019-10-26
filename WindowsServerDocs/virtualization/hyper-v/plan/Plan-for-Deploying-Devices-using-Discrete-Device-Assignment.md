---
title: Planeación de la implementación de dispositivos mediante la asignación discreta de dispositivos
description: Más información sobre cómo funciona DDA en Windows Server
ms.prod: windows-server
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.date: 08/21/2019
ms.openlocfilehash: 114dd87b86bfffd1070229af57ae65deea2c2db0
ms.sourcegitcommit: 81198fbf9e46830b7f77dcd345b02abb71ae0ac2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2019
ms.locfileid: "72923866"
---
# <a name="plan-for-deploying-devices-using-discrete-device-assignment"></a>Planeación de la implementación de dispositivos mediante la asignación discreta de dispositivos
>Se aplica a: Microsoft Hyper-V Server 2016, Windows Server 2016, Microsoft Hyper-V Server 2019, Windows Server 2019

La asignación discreta de dispositivos permite que el hardware físico de PCIe sea accesible directamente desde una máquina virtual.  En esta guía se describe el tipo de dispositivos que pueden usar la asignación discreta de dispositivos, los requisitos del sistema host, las limitaciones impuestas en las máquinas virtuales, así como las implicaciones de seguridad de la asignación discreta de dispositivos.

En el caso de la versión inicial de la asignación de dispositivos discretos, hemos centrado en dos clases de dispositivos para que Microsoft los admita formalmente: adaptadores de gráficos y dispositivos de almacenamiento de NVMe.  Es probable que otros dispositivos funcionen y los proveedores de hardware puedan ofrecer instrucciones de soporte técnico para esos dispositivos.  Para estos otros dispositivos, póngase en contacto con los proveedores de hardware para obtener soporte técnico.

Para obtener información sobre otros métodos de virtualización de GPU, consulte [Plan for GPU Acceleration in Windows Server](plan-for-gpu-acceleration-in-windows-server.md). Si está listo para probar la asignación discreta de dispositivos, puede ir a la implementación de [dispositivos de gráficos mediante la asignación discreta](../deploy/Deploying-graphics-devices-using-dda.md) de dispositivos o la [implementación de dispositivos de almacenamiento mediante la asignación discreta](../deploy/Deploying-storage-devices-using-dda.md) de dispositivos para comenzar.

## <a name="supported-virtual-machines-and-guest-operating-systems"></a>Sistemas operativos invitados y Virtual Machines admitidos
La asignación de dispositivos discretos se admite para las máquinas virtuales de generación 1 o 2.  Además, los invitados admitidos son Windows 10, Windows Server 2019, Windows Server 2016, Windows Server 2012r2 con [KB 3133690](https://support.microsoft.com/kb/3133690) aplicado y diversas distribuciones del [sistema operativo Linux.](../supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)

## <a name="system-requirements"></a>Requisitos del sistema
Además de los [requisitos del sistema para Windows Server](../../../get-started/System-Requirements--and-Installation.md) y los [requisitos del sistema para Hyper-V](../System-requirements-for-Hyper-V-on-Windows.md), la asignación discreta de dispositivos requiere hardware de clase de servidor capaz de conceder el control del sistema operativo sobre la configuración de la tarjeta PCIe tejido (control nativo de PCI Express). Además, el complejo raíz de PCIe tiene que admitir "Access Control Services" (ACS), que permite a Hyper-V forzar todo el tráfico de PCIe a través de la MMU de e/s.

Estas funcionalidades normalmente no se exponen directamente en el BIOS del servidor y suelen estar ocultas detrás de otras configuraciones.  Por ejemplo, se requieren las mismas capacidades para la compatibilidad con SR-IOV y, en el BIOS, puede que tenga que establecer "habilitar SR-IOV".  Póngase en contacto con el proveedor del sistema si no puede identificar la configuración correcta en el BIOS.

Para ayudar a garantizar que el hardware sea capaz de realizar la asignación discreta de dispositivos, nuestros ingenieros han unido un [script de Perfil de equipo](#machine-profile-script) que puede ejecutar en un host habilitado para Hyper-V para probar si el servidor se configura correctamente y qué dispositivos son capaces Asignación discreta de dispositivos.

## <a name="device-requirements"></a>Requisitos del dispositivo
No todos los dispositivos PCIe se pueden usar con la asignación discreta de dispositivos.  Por ejemplo, no se admiten los dispositivos más antiguos que aprovechan interrupciones PCI heredadas (INTx). Las entradas de [blog](https://blogs.technet.microsoft.com/virtualization/2015/11/20/discrete-device-assignment-machines-and-devices/) de Juan OSHIN tienen más detalles. sin embargo, para el consumidor, al ejecutar el [script de Perfil de equipo](#machine-profile-script) se mostrarán los dispositivos que pueden usarse para la asignación discreta de dispositivos.

Los fabricantes de dispositivos pueden ponerse en contacto con su representante de Microsoft para obtener más información.

## <a name="device-driver"></a>Controlador de dispositivo
Como la asignación de dispositivos discretos pasa todo el dispositivo PCIe a la máquina virtual invitada, no es necesario instalar un controlador de host antes de montar el dispositivo dentro de la máquina virtual.  El único requisito en el host es que se pueda determinar la ruta de acceso de la [Ubicación de PCIe](#pcie-location-path) del dispositivo.  Opcionalmente, se puede instalar el controlador del dispositivo si esto ayuda a identificar el dispositivo.  Por ejemplo, una GPU sin su controlador de dispositivo instalado en el host puede aparecer como un dispositivo de representación básica de Microsoft.  Si el controlador del dispositivo está instalado, es probable que se muestre el fabricante y el modelo.

Una vez que el dispositivo se monta dentro del invitado, el controlador de dispositivo del fabricante se puede instalar ahora como normal dentro de la máquina virtual invitada.  

## <a name="virtual-machine-limitations"></a>Limitaciones de las máquinas virtuales
Debido a la naturaleza de la implementación discreta de la asignación de dispositivos, algunas características de una máquina virtual están restringidas mientras se conecta un dispositivo.  Las siguientes características no están disponibles:
- Almacenamiento y restauración de máquinas virtuales
- Migración en vivo de una máquina virtual
- El uso de memoria dinámica
- Adición de la máquina virtual a un clúster de alta disponibilidad (HA)

## <a name="security"></a>Seguridad
La asignación de dispositivos discretos pasa todo el dispositivo a la máquina virtual.  Esto significa que se puede tener acceso a todas las funciones de ese dispositivo desde el sistema operativo invitado. Algunas funciones, como la actualización de firmware, pueden afectar negativamente a la estabilidad del sistema. Como tal, se presentan numerosas advertencias al administrador al desmontar el dispositivo del host. Se recomienda encarecidamente que la asignación de dispositivos discretos solo se use cuando se confíe en los inquilinos de las máquinas virtuales.  

Si el administrador desea usar un dispositivo con un inquilino que no es de confianza, hemos proporcionado fabricantes de dispositivos con la posibilidad de crear un controlador de mitigación de dispositivos que se pueda instalar en el host.  Póngase en contacto con el fabricante del dispositivo para obtener más información sobre si proporcionan un controlador de mitigación de dispositivos.

Si desea omitir las comprobaciones de seguridad de un dispositivo que no tiene un controlador de mitigación de dispositivos, tendrá que pasar el parámetro `-Force` al cmdlet `Dismount-VMHostAssignableDevice`.  Tenga en cuenta que, al hacerlo, ha cambiado el perfil de seguridad de ese sistema y esto solo se recomienda durante los entornos de prototipo o de confianza.

## <a name="pcie-location-path"></a>Ruta de acceso de ubicación de PCIe
La ruta de acceso de ubicación de PCIe es necesaria para desmontar y montar el dispositivo del host.  Una ruta de acceso de ubicación de ejemplo tiene un aspecto similar al siguiente: `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`.   El [script de Perfil de equipo](#machine-profile-script) también devolverá la ruta de acceso de la ubicación del dispositivo PCIe.

### <a name="getting-the-location-path-by-using-device-manager"></a>Obtener la ruta de acceso de ubicación mediante Device Manager
![Administrador de dispositivos](../deploy/media/dda-devicemanager.png)
- Abra Device Manager y busque el dispositivo.  
- Haga clic con el botón derecho en el dispositivo y seleccione "propiedades".
- Vaya a la pestaña detalles y seleccione "rutas de acceso de ubicación" en la lista desplegable de propiedades.  
- Haga clic con el botón derecho en la entrada que empieza por "PCIROOT" y seleccione "copiar".  Ahora tiene la ruta de acceso de la ubicación para ese dispositivo.

## <a name="mmio-space"></a>Espacio MMIO
Algunos dispositivos, especialmente las GPU, requieren espacio MMIO adicional para asignarse a la máquina virtual para que la memoria de ese dispositivo sea accesible. De forma predeterminada, cada máquina virtual se inicia con 128 MB de espacio de enmmio bajo y 512 MB de espacio de MMIO elevado asignado. Sin embargo, un dispositivo puede requerir más espacio MMIO, o bien se pueden pasar varios dispositivos a través de tal que los requisitos combinados superen estos valores.  Cambiar el espacio MMIO es sencillo y se puede realizar en PowerShell mediante los siguientes comandos:

```PowerShell
Set-VM -LowMemoryMappedIoSpace 3Gb -VMName $vm
Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName $vm
```

La forma más fácil de determinar la cantidad de espacio MMIO que se va a asignar es usar el [script de Perfil de equipo](#machine-profile-script). Para descargar y ejecutar el script de Perfil de equipo, ejecute los siguientes comandos en una consola de PowerShell:

```PowerShell
curl -o SurveyDDA.ps1 https://raw.githubusercontent.com/MicrosoftDocs/Virtualization-Documentation/live/hyperv-tools/DiscreteDeviceAssignment/SurveyDDA.ps1
.\SurveyDDA.ps1
```

En el caso de los dispositivos que se pueden asignar, el script mostrará los requisitos de MMIO de un dispositivo determinado, como en el ejemplo siguiente:

```PowerShell
NVIDIA GRID K520
Express Endpoint -- more secure.
    ...
    And it requires at least: 176 MB of MMIO gap space
...
```

El espacio bajo MMIO solo lo usan los sistemas operativos de 32 bits y los dispositivos que usan direcciones de 32 bits. En la mayoría de los casos, la configuración del espacio MMIO máximo de una máquina virtual será suficiente, ya que las configuraciones de 32 bits no son muy comunes.

> [!IMPORTANT]
> Al asignar espacio MMIO a una máquina virtual, el usuario debe asegurarse de establecer el espacio de MMIO en la suma del espacio de MMIO solicitado para todos los dispositivos asignados deseados más un búfer adicional si hay otros dispositivos virtuales que requieren unos pocos MB de espacio de MMIO. Use los valores de MMIO predeterminados descritos anteriormente como búfer para enmmio bajo y alto (128 MB y 512 MB, respectivamente).

Si un usuario asignara una sola GPU de K520 como en el ejemplo anterior, debe establecer el espacio de MMIO de la máquina virtual en el valor que genera el script de Perfil de equipo más un búfer, 176 MB + 512 MB. Si un usuario asignara tres GPU de K520, debe establecer el espacio de MMIO en tres veces 176 MB más un búfer, o 528 MB + 512 MB.

Para ver información más detallada sobre el espacio de MMIO, consulte [asignación de dispositivos discretos: GPU](https://techcommunity.microsoft.com/t5/Virtualization/Discrete-Device-Assignment-GPUs/ba-p/382266) en el blog de TechCommunity.

## <a name="machine-profile-script"></a>Script de Perfil de equipo
Con el fin de simplificar la identificación si el servidor está configurado correctamente y los dispositivos que están disponibles para pasarlos mediante la asignación discreta de dispositivos, uno de nuestros ingenieros reúnen el siguiente script de PowerShell: [SurveyDDA. ps1.](https://github.com/Microsoft/Virtualization-Documentation/blob/live/hyperv-tools/DiscreteDeviceAssignment/SurveyDDA.ps1)

Antes de usar el script, asegúrese de que tiene instalado el rol de Hyper-V y ejecute el script desde una ventana de comandos de PowerShell que tenga privilegios de administrador.

Si el sistema está configurado incorrectamente para admitir la asignación discreta de dispositivos, la herramienta mostrará un mensaje de error en lo que se repasa. Si la herramienta encuentra el sistema configurado correctamente, enumerará todos los dispositivos que puede encontrar en el bus de PCIe.

Para cada dispositivo que encuentre, la herramienta mostrará si se puede usar con la asignación discreta de dispositivos. Si un dispositivo se identifica como compatible con la asignación discreta de dispositivos, el script proporcionará un motivo.  Cuando un dispositivo se identifica correctamente como compatible, se mostrará la ruta de acceso de la ubicación del dispositivo.  Además, si ese dispositivo requiere [espacio MMIO](#mmio-space), también se mostrará.

![SurveyDDA. ps1](./images/hyper-v-surveydda-ps1.png)
