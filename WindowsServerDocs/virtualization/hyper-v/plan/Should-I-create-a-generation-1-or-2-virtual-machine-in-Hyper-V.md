---
title: ¿Debo crear una máquina virtual de generación 1 o 2 en Hyper-V?
description: Proporciona consideraciones, como se admiten métodos de inicio y otras diferencias de características para ayudarle a elegir qué generación satisfaga sus necesidades.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 02e31413-6140-4723-a8d6-46c7f667792d
author: KBDAzure
ms.author: kathydav
ms.date: 12/05/2016
ms.openlocfilehash: 95ececde8a1b8c591ea2baf367a93f63ee55a6e3
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811987"
---
# <a name="should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v"></a>¿Debo crear una máquina virtual de generación 1 o 2 en Hyper-V?

>Se aplica a: Windows 10, Windows Server 2016 y Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

> [!NOTE]
> Si planea nunca cargar una máquina virtual (VM) de Windows desde local a Microsoft Azure, generación 1 y las máquinas virtuales de generación 2 en el formato de archivo de disco duro virtual y tiene un disco de tamaño fijo se admiten. Consulte [las máquinas virtuales de generación 2 en Azure](https://docs.microsoft.com/azure/virtual-machines/windows/generation-2) para obtener más información sobre las capacidades de generación 2 admitidas en Azure. Para obtener más información acerca de cómo cargar un VHD de Windows o un VHDX, vea [preparar un VHD de Windows o un VHDX para cargar en Azure](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image).

La elección para crear una generación 1 o una máquina virtual de generación 2 depende del sistema operativo invitado que desea instalar y el método de arranque que desea usar para implementar la máquina virtual. Le recomendamos que cree una máquina virtual de generación 2 para aprovechar características como arranque seguro, a menos que una de las siguientes afirmaciones es verdadera:  

- El disco duro virtual que desea arrancar desde no es [compatibles con UEFI](https://technet.microsoft.com/library/hh824898.aspx).  
- Generación 2 no es compatible con el sistema operativo que desea ejecutar en la máquina virtual.  
- Generación 2 no es compatible con el método de arranque que desea usar.  

Para obtener más información acerca de qué características están disponibles con las máquinas virtuales de generación 2, consulte [compatibilidad de características de Hyper-V mediante la generación e invitado](../Hyper-V-feature-compatibility-by-generation-and-guest.md).

No se puede cambiar la generación de una máquina virtual después de que haya creado. Por lo tanto, se recomienda que revisar las consideraciones de aquí, así como elegir el sistema operativo, el método de arranque y características que desea usar antes de elegir una generación.  

## <a name="which-guest-operating-systems-are-supported"></a>¿Qué sistemas operativos de invitado son compatibles?

Máquinas virtuales de generación 1 admiten la mayoría de los sistemas operativos invitados. Máquinas virtuales de generación 2 admiten más versiones de 64 bits de Windows y las versiones más actuales de los sistemas operativos Linux y FreeBSD. Use las secciones siguientes para ver la generación de máquina virtual es compatible con el sistema operativo que desea instalar.  

- [Compatibilidad de sistema operativo invitado de Windows](#windows-guest-operating-system-support)  

- [CentOS y Red Hat Enterprise Linux compatibilidad de sistema operativo de invitado](#centos-and-red-hat-enterprise-linux-guest-operating-system-support)  

- [Compatibilidad de sistema operativo invitado Debian](#debian-guest-operating-system-support)  

- [Compatibilidad de sistema operativo invitado de FreeBSD](#freebsd-guest-operating-system-support)  

- [Compatibilidad de sistema operativo de invitado de Linux de Oracle](#oracle-linux-guest-operating-system-support)  

- [Compatibilidad de sistema operativo invitado SUSE](#suse-guest-operating-system-support)  

- [Compatibilidad de sistema operativo invitado de Ubuntu](#ubuntu-guest-operating-system-support)  

### <a name="windows-guest-operating-system-support"></a>Compatibilidad de sistema operativo invitado de Windows

La siguiente tabla muestra qué versiones de 64 bits de Windows puede usar como un sistema operativo invitado para máquinas virtuales de generación 2 y la generación 1.  

|versiones de 64 bits de Windows|Generación 1|Generación 2|  
|-------------------------------|----------------|----------------|  
| Windows Server 2019 |&#10004;|&#10004;|  
| Windows Server 2016 |&#10004;|&#10004;|  
| Windows Server 2012 R2 |&#10004;|&#10004;|  
| Windows Server 2012 |&#10004;|&#10004;|  
|Windows Server 2008 R2|&#10004;| &#10006;|  
|Windows Server 2008|&#10004;| &#10006;|  
|Windows 10|&#10004;|&#10004;|  
|Windows 8.1|&#10004;|&#10004;|  
|Windows 8|&#10004;|&#10004;|  
|Windows 7|&#10004;| &#10006;|

La siguiente tabla muestra qué versiones de 32 bits de Windows puede usar como un sistema operativo invitado para máquinas virtuales de generación 2 y la generación 1.

|versiones de 32 bits de Windows|Generación 1|Generación 2|  
|-------------------------------|----------------|----------------|  
|Windows 10|&#10004;| &#10006;|  
|Windows 8.1|&#10004;| &#10006;|  
|Windows 8|&#10004;| &#10006;|  
|Windows 7|&#10004;| &#10006;|  

### <a name="centos-and-red-hat-enterprise-linux-guest-operating-system-support"></a>CentOS y Red Hat Enterprise Linux compatibilidad de sistema operativo de invitado

La siguiente tabla muestra qué versiones de Red Hat Enterprise Linux \(RHEL\) y CentOS puede usar como un sistema operativo invitado para máquinas virtuales de generación 2 y la generación 1.

|Versiones de sistema operativo|Generación 1|Generación 2|  
|-----------------------------|----------------|----------------|  
|RHEL/CentOS 7.x series|&#10004;|&#10004;|  
|Serie RHEL/CentOS 6.x|&#10004;|&#10004;<br />**Nota:** Solo se admite en Windows Server 2016 y versiones posteriores.|  
|Serie RHEL/CentOS 5.x|&#10004;| &#10006;|  

Para obtener más información, consulte [CentOS y Red Hat Enterprise Linux las máquinas virtuales de Hyper-V](../Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md).  

### <a name="debian-guest-operating-system-support"></a>Compatibilidad de sistema operativo invitado Debian  

La siguiente tabla muestra qué versiones de Debian puede usar como un sistema operativo invitado para la generación 1 y máquinas virtuales de generación 2.

|Versiones de sistema operativo|Generación 1|Generación 2|  
|-----------------------------|----------------|----------------|  
|Serie de Debian 7.x|&#10004;| &#10006;|  
|Serie de Debian 8.x|&#10004;|&#10004;|  

Para obtener más información, consulte [máquinas virtuales de Debian en Hyper-V](../Supported-Debian-virtual-machines-on-Hyper-V.md).  

### <a name="freebsd-guest-operating-system-support"></a>Compatibilidad de sistema operativo invitado de FreeBSD

La siguiente tabla muestra qué versiones de FreeBSD puede usar como un sistema operativo invitado para máquinas virtuales de generación 2 y la generación 1.  

|Versiones de sistema operativo|Generación 1|Generación 2|  
|-----------------------------|----------------|----------------|  
|FreeBSD 10 y 10.1|&#10004;| &#10006;|  
|9.1 de FreeBSD y 9.3|&#10004;| &#10006;|  
|FreeBSD 8.4|&#10004;| &#10006;|  

Para obtener más información, consulte [máquinas virtuales de FreeBSD en Hyper-V](../Supported-FreeBSD-virtual-machines-on-Hyper-V.md).  

### <a name="oracle-linux-guest-operating-system-support"></a>Compatibilidad de sistema operativo de invitado de Linux de Oracle  

La siguiente tabla muestra qué versiones de las Series de Kernel Red Hat compatibles puede usar como un sistema operativo invitado para la generación 1 y máquinas virtuales de generación 2.  

|Versiones de la serie de Kernel Red Hat Compatible|Generación 1|Generación 2|  
|---------------------------------------------|----------------|----------------|  
|Serie de Oracle Linux 7.x|&#10004;|&#10004;|
|Serie de Oracle Linux 6.x|&#10004;| &#10006;|  

La siguiente tabla muestra qué versiones de Unbreakable Enterprise Kernel puede usar como un sistema operativo invitado para la generación 1 y máquinas virtuales de generación 2.

|Versiones Unbreakable Enterprise Kernel (UEK)|Generación 1|Generación 2|  
|--------------------------------------------------|----------------|----------------|  
|Oracle Linux UEK R3 QU3|&#10004;| &#10006;|  
|Oracle Linux UEK R3 QU2|&#10004;| &#10006;|  
|Oracle Linux UEK R3 QU1|&#10004;| &#10006;|  

Para obtener más información, consulte [máquinas virtuales Linux de Oracle en Hyper-V](../Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md).  

### <a name="suse-guest-operating-system-support"></a>Compatibilidad de sistema operativo invitado SUSE

La siguiente tabla muestra qué versiones de SUSE puede usar como un sistema operativo invitado para máquinas virtuales de generación 2 y la generación 1.

|Versiones de sistema operativo|Generación 1|Generación 2|  
|-----------------------------|----------------|----------------|  
|Serie de SUSE Linux Enterprise Server 12|&#10004;|&#10004;|  
|Serie de SUSE Linux Enterprise Server 11|&#10004;| &#10006;|  
|Abra SUSE 12.3|&#10004;| &#10006;|  

Para obtener más información, consulte [las máquinas virtuales SUSE en Hyper-V](../Supported-SUSE-virtual-machines-on-Hyper-V.md).  

### <a name="ubuntu-guest-operating-system-support"></a>Compatibilidad de sistema operativo invitado de Ubuntu

La siguiente tabla muestra qué versiones de Ubuntu puede usar como un sistema operativo invitado para máquinas virtuales de generación 2 y la generación 1.

|Versiones de sistema operativo|Generación 1|Generación 2|  
|-----------------------------|----------------|----------------|  
|Ubuntu 14.04 y versiones posteriores|&#10004;|&#10004;|  
|Ubuntu 12.04|&#10004;| &#10006;|  

Para obtener más información, consulte [las máquinas virtuales de Ubuntu en Hyper-V](../Supported-Ubuntu-virtual-machines-on-Hyper-V.md).  

## <a name="how-can-i-boot-the-virtual-machine"></a>¿Cómo se puede arrancar la máquina virtual?

La siguiente tabla muestra qué métodos son compatibles con la generación 1 y máquinas virtuales de generación 2 de arranque.  

|Método de inicio|Generación 1|Generación 2|  
|---------------|----------------|----------------|  
|Arranque PXE con un adaptador de red estándar| &#10006;|&#10004;|  
|Arranque PXE mediante un adaptador de red heredado|&#10004;| &#10006;|  
|Arranque desde un disco duro virtual SCSI (. VHDX) o un DVD virtual (. ISO)| &#10006;|&#10004;|  
|Arranque desde disco duro virtual de controladora IDE (. Disco duro virtual) o un DVD virtual (. ISO)|&#10004;| &#10006;|  
|Arranque desde un disquete (. VFD)|&#10004;| &#10006;|  

## <a name="what-are-the-advantages-of-using-generation-2-virtual-machines"></a>¿Cuáles son las ventajas del uso de máquinas virtuales de generación 2?

Estas son algunas de las ventajas que obtendrá al usar una máquina virtual de generación 2:  
- **Arranque seguro** esta es una característica que comprueba que el cargador de arranque está firmado por una autoridad de confianza en la base de datos UEFI para ayudar a impedir que se ejecuta en tiempo de arranque no autorizado firmware, sistemas operativos o controladores UEFI. Arranque seguro está habilitado de manera predeterminada para máquinas virtuales de generación 2. Si necesita ejecutar un sistema operativo invitado que no es compatible con el arranque seguro, puede deshabilitar una vez creada la máquina virtual.  Para obtener más información, consulta [Arranque seguro](https://technet.microsoft.com/library/dn486875.aspx).  

    Para máquinas de virtuales de Linux de generación 2 de arranque seguro, deberá elegir la plantilla de arranque seguro de UEFI CA cuando se crea la máquina virtual.  

- **Mayor volumen de arranque** el volumen de arranque máximo para las máquinas virtuales de generación 2 es de 64 TB. Este es el tamaño de disco máximo admitido por una. VHDX. Las máquinas virtuales de generación 1, el volumen de arranque máximo es 2TB para un. VHDX y 2040GB para una. DISCO DURO VIRTUAL. Para obtener más información, consulte [Hyper-V Virtual Hard Disk Format Overview](https://technet.microsoft.com/library/hh831446.aspx).  

  También puede ver una ligera mejora en los tiempos de arranque e instalación de máquina virtual con máquinas virtuales de generación 2.

## <a name="whats-the-difference-in-device-support"></a>¿Qué es la diferencia en la compatibilidad con dispositivos?

En la tabla siguiente compara los dispositivos disponibles entre la generación 1 y máquinas virtuales de generación 2.  

|Dispositivo de generación 1|Sustitución de generación 2|Mejoras de generación 2|  
|-----------------------|----------------------------|-----------------------------|  
|Controladora IDE|Controladora SCSI virtual|Arranque desde .vhdx (tamaño máximo de 64 TB y capacidad de cambio de tamaño en línea)|  
|CD-ROM IDE|CD-ROM SCSI virtual|Admite hasta 64 dispositivos DVD SCSI por controladora SCSI.|  
|BIOS heredado|Firmware UEFI|Arranque seguro|  
|Adaptador de red heredado|Adaptador de red sintético|Arranque de red con IPv4 e IPv6|  
|Controladora de disquete y controladora DMA|No es compatible con controladoras de disquete|N/D|  
|Receptor/transmisor asincrónico universal (UART) para puertos COM|UART opcional para depuración|Más rápido y fiable|  
|Controladora de teclado i8042|Entrada basada en software|Usa menos recursos, ya que no se produce emulación. Además, también reduce la superficie expuesta a ataques del sistema operativo invitado.|  
|Teclado PS/2|Teclado basado en software|Usa menos recursos, ya que no se produce emulación. Además, también reduce la superficie expuesta a ataques del sistema operativo invitado.|  
|Mouse PS/2|Mouse basado en software|Usa menos recursos, ya que no se produce emulación. Además, también reduce la superficie expuesta a ataques del sistema operativo invitado.|  
|Vídeo S3|Vídeo basado en software|Usa menos recursos, ya que no se produce emulación. Además, también reduce la superficie expuesta a ataques del sistema operativo invitado.|  
|Bus PCI|Ya no es necesario|N/D|  
|Controladora programable de interrupciones (PIC)|Ya no es necesario|N/D|  
|Temporizador de informes programable (PIT)|Ya no es necesario|N/D|  
|Dispositivo Super I/O|Ya no es necesario|N/D|  

## <a name="more-about-generation-2-virtual-machines"></a>Más información acerca de las máquinas virtuales de generación 2

Estas son algunas sugerencias adicionales sobre el uso de máquinas virtuales de generación 2.

### <a name="attach-or-add-a-dvd-drive"></a>Asociar o agregar una unidad de DVD

- No se puede adjuntar una unidad de CD o DVD física en una máquina virtual de generación 2. La unidad de DVD virtual de las máquinas virtuales de generación 2 solo admiten archivos de imagen ISO. Para crear un archivo de imagen ISO de un entorno de Windows, puede utilizar la herramienta de línea de comandos Oscdimg. Para más información, vea [Opciones de la línea de comandos de Oscdimg](https://msdn.microsoft.com/library/hh824847.aspx).
- Cuando se crea una nueva máquina virtual con el cmdlet New-VM de Windows PowerShell, la máquina virtual de generación 2 no tiene una unidad de DVD. Puede agregar una unidad de DVD mientras se está ejecutando la máquina virtual.

### <a name="use-uefi-firmware"></a>Usan el firmware UEFI

- Arranque seguro o firmware UEFI no es necesario en el host de Hyper-V físico. Hyper-V ofrece el firmware virtual a las máquinas virtuales que es independiente de lo que aparece en el host de Hyper-V.
- Firmware UEFI de una máquina virtual de generación 2 no es compatible con el modo de configuración para el arranque seguro.
- No se admite ejecutar una shell de UEFI u otras aplicaciones UEFI en una máquina virtual de generación 2. Desde el punto de vista técnico, es posible usar una shell de UEFI o aplicaciones UEFI que no sean de Microsoft, siempre que se compilen directamente desde los archivos de origen. Si estas aplicaciones no están correctamente firmadas digitalmente, debe deshabilitar el arranque seguro para la máquina virtual.

### <a name="work-with-vhdx-files"></a>Trabajar con archivos VHDX

- Puede cambiar el tamaño de un archivo VHDX que contiene el volumen de arranque para una máquina virtual de generación 2 mientras se está ejecutando la máquina virtual.
- Se admiten ni se recomienda que cree un archivo VHDX que es de arranque para la generación 1 y máquinas virtuales de generación 2.  
- La generación de máquina virtual es una propiedad de la máquina virtual, no una propiedad del disco duro virtual. Por lo que no puede saber si un archivo VHDX fue creado por una generación 1 o una máquina virtual de generación 2.  
- Un archivo VHDX creado con una generación 2 máquina virtual puede asociarse a la controladora IDE o la controladora SCSI de una máquina virtual de generación 1. Sin embargo, si se trata de un archivo VHDX de arranque, no arrancar la máquina virtual de generación 1.

### <a name="use-ipv6-instead-of-ipv4"></a>Usar IPv6 en lugar de IPv4

De manera predeterminada, las máquinas virtuales de generación 2 usan IPv4. Para usar IPv6 en su lugar, ejecute el [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx) cmdlet de Windows PowerShell. Por ejemplo, el siguiente comando establece el protocolo preferido para IPv6 para una máquina virtual llamada TestVM:  

```powershell
Set-VMFirmware -VMName TestVM -IPProtocolPreference IPv6  
```  

## <a name="add-a-com-port-for-kernel-debugging"></a>Agregar un puerto COM para la depuración de kernel

Puertos COM no están disponibles en las máquinas virtuales de generación 2 hasta que los agregue. Puede hacerlo con Windows PowerShell o Instrumental de administración de Windows (WMI). Estos pasos muestran cómo hacerlo con Windows PowerShell.

Para agregar un puerto COM:  

1. Deshabilita arranque seguro. Depuración del kernel no es compatible con el arranque seguro. Asegúrese de que la máquina virtual está en estado desactivado, a continuación, utilice el [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx) cmdlet. Por ejemplo, el siguiente comando deshabilita el arranque seguro en la máquina virtual TestVM:  

    ```powershell  
    Set-VMFirmware -Vmname TestVM -EnableSecureBoot Off  
    ```  

2. Agregar un puerto COM. Use la [Set-VMComPort](https://technet.microsoft.com/library/hh848616.aspx) cmdlet para hacer esto. Por ejemplo, el siguiente comando configura el primer puerto COM en la máquina virtual, TestVM, para conectarse a la canalización llamada TestPipe en el equipo local:  

    ```powershell
    Set-VMComPort -VMName TestVM 1 \\.\pipe\TestPipe  
    ```  

> [!NOTE]  
> Los puertos COM configurados no aparecen en la configuración de una máquina virtual en Hyper-V Manager.

## <a name="see-also"></a>Vea también  

- [Linux y FreeBSD máquinas virtuales Hyper-V](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)
- [Usar los recursos locales en la máquina virtual de Hyper-V con VMConnect](../learn-more/Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)
- [Planear la escalabilidad de Hyper-V en Windows Server 2016](Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)
