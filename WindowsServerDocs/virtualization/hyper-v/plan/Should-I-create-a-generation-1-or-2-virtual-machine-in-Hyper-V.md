---
title: ¿Debo crear una máquina virtual de generación 1 o 2 en Hyper-V?
description: Ofrece consideraciones como métodos de arranque admitidos y otras diferencias de características para ayudarle a elegir qué generación satisface sus necesidades.
ms.topic: article
ms.assetid: 02e31413-6140-4723-a8d6-46c7f667792d
ms.author: benarm
author: BenjaminArmstrong
ms.date: 12/05/2016
ms.openlocfilehash: 19416bbb8800f12d37d3fdd28760082eb8ea523d
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96866014"
---
# <a name="should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v"></a>¿Debo crear una máquina virtual de generación 1 o 2 en Hyper-V?

>Se aplica a: Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

> [!NOTE]
> Si tiene previsto cargar máquinas virtuales (VM) de Windows desde una ubicación local a Microsoft Azure, se admiten las máquinas virtuales de generación 1 y generación 2 en el formato de archivo VHD y tienen un disco de tamaño fijo. Consulte [máquinas virtuales de segunda generación en Azure](/azure/virtual-machines/windows/generation-2) para más información sobre las capacidades de generación 2 que se admiten en Azure. Para obtener más información sobre cómo cargar un VHD de Windows o VHDX, consulte [preparación de un VHD de Windows o vhdx para cargarlos en Azure](/azure/virtual-machines/windows/prepare-for-upload-vhd-image).

La opción de crear una máquina virtual de generación 1 o de generación 2 depende del sistema operativo invitado que desea instalar y el método de arranque que desea usar para implementar la máquina virtual. Se recomienda crear una máquina virtual de generación 2 para aprovechar las ventajas de las características como el arranque seguro, a menos que se cumpla una de las siguientes instrucciones:

- El disco duro virtual desde el que desea arrancar no es [compatible con UEFI](/previous-versions/windows/it-pro/windows-8.1-and-8/hh824898(v=win.10)).
- La generación 2 no es compatible con el sistema operativo que desea ejecutar en la máquina virtual.
- La generación 2 no es compatible con el método de arranque que desea usar.

Para obtener más información sobre las características que están disponibles con las máquinas virtuales de generación 2, consulte compatibilidad de las características de [Hyper-V mediante generación e invitado](../Hyper-V-feature-compatibility-by-generation-and-guest.md).

No se puede cambiar la generación de una máquina virtual después de haberla creado. Por lo tanto, se recomienda revisar las consideraciones que se indican aquí, así como elegir el sistema operativo, el método de arranque y las características que quiere usar antes de elegir una generación.

## <a name="which-guest-operating-systems-are-supported"></a>¿Qué sistemas operativos invitados son compatibles?

Las máquinas virtuales de generación 1 admiten la mayoría de los sistemas operativos invitados. Las máquinas virtuales de generación 2 admiten la mayoría de las versiones de 64 bits de Windows y más versiones actuales de los sistemas operativos Linux y FreeBSD. Use las secciones siguientes para ver qué generación de máquina virtual es compatible con el sistema operativo invitado que desea instalar.

- [Compatibilidad con sistemas operativos invitados de Windows](#windows-guest-operating-system-support)

- [Compatibilidad con el sistema operativo invitado de acRed Hat Enterprise Linux y de](#centos-and-red-hat-enterprise-linux-guest-operating-system-support)

- [Compatibilidad del sistema operativo invitado de Debian](#debian-guest-operating-system-support)

- [Compatibilidad con el sistema operativo invitado de FreeBSD](#freebsd-guest-operating-system-support)

- [Compatibilidad con Oracle Linux sistema operativo invitado](#oracle-linux-guest-operating-system-support)

- [Compatibilidad con sistemas operativos invitados SUSE](#suse-guest-operating-system-support)

- [Compatibilidad con el sistema operativo invitado de Ubuntu](#ubuntu-guest-operating-system-support)

### <a name="windows-guest-operating-system-support"></a>Compatibilidad con sistemas operativos invitados de Windows

En la tabla siguiente se muestran las versiones de Windows de 64 bits que puede usar como sistema operativo invitado para las máquinas virtuales de generación 1 y generación 2.

|versiones de 64 bits de Windows|Generación 1|Generación 2|
|-------------------------------|----------------|----------------|
| Windows Server 2019 |&#10004;|&#10004;|
| Windows Server 2016 |&#10004;|&#10004;|
| Windows Server 2012 R2 |&#10004;|&#10004;|
| Windows Server 2012 |&#10004;|&#10004;|
|Windows Server 2008 R2|&#10004;| &#10006;|
|Windows Server 2008|&#10004;| &#10006;|
|Windows 10|&#10004;|&#10004;|
|Windows 8.1|&#10004;|&#10004;|
|Windows 8|&#10004;|&#10004;|
|Windows 7|&#10004;| &#10006;|

En la tabla siguiente se muestran las versiones de Windows de 32 bits que puede usar como sistema operativo invitado para las máquinas virtuales de generación 1 y generación 2.

|versiones de 32 bits de Windows|Generación 1|Generación 2|
|-------------------------------|----------------|----------------|
|Windows 10|&#10004;| &#10006;|
|Windows 8.1|&#10004;| &#10006;|
|Windows 8|&#10004;| &#10006;|
|Windows 7|&#10004;| &#10006;|

### <a name="centos-and-red-hat-enterprise-linux-guest-operating-system-support"></a>Compatibilidad con el sistema operativo invitado de acRed Hat Enterprise Linux y de

En la tabla siguiente se muestran las versiones de Red Hat Enterprise Linux \( RHEL \) y la versión de usuario que puede usar como sistema operativo invitado para máquinas virtuales de generación 1 y generación 2.

|Versiones de sistema operativo|Generación 1|Generación 2|
|-----------------------------|----------------|----------------|
|RHEL/la serie 7. x|&#10004;|&#10004;|
|RHEL/la serie 6. x|&#10004;|&#10004;<br />**Nota:** Solo se admite en Windows Server 2016 y versiones posteriores.|
|RHEL/la serie 5. x|&#10004;| &#10006;|

Para obtener más información, vea las [máquinas virtuales de alta y Red Hat Enterprise Linux en Hyper-V](../Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md).

### <a name="debian-guest-operating-system-support"></a>Compatibilidad del sistema operativo invitado de Debian

En la tabla siguiente se muestran las versiones de Debian que puede usar como sistema operativo invitado para las máquinas virtuales de generación 1 y generación 2.

|Versiones de sistema operativo|Generación 1|Generación 2|
|-----------------------------|----------------|----------------|
|Serie Debian 7. x|&#10004;| &#10006;|
|Serie Debian 8. x|&#10004;|&#10004;|

Para obtener más información, vea las [máquinas virtuales de Debian en Hyper-V](../Supported-Debian-virtual-machines-on-Hyper-V.md).

### <a name="freebsd-guest-operating-system-support"></a>Compatibilidad con el sistema operativo invitado de FreeBSD

En la tabla siguiente se muestran las versiones de FreeBSD que puede usar como sistema operativo invitado para las máquinas virtuales de generación 1 y generación 2.

|Versiones de sistema operativo|Generación 1|Generación 2|
|-----------------------------|----------------|----------------|
|FreeBSD 10 y 10,1|&#10004;| &#10006;|
|FreeBSD 9,1 y 9,3|&#10004;| &#10006;|
|FreeBSD 8,4|&#10004;| &#10006;|

Para obtener más información, consulte [máquinas virtuales de FreeBSD en Hyper-V](../Supported-FreeBSD-virtual-machines-on-Hyper-V.md).

### <a name="oracle-linux-guest-operating-system-support"></a>Compatibilidad con Oracle Linux sistema operativo invitado

En la tabla siguiente se muestran las versiones de la serie de kernel compatibles con Red Hat que puede usar como sistema operativo invitado para las máquinas virtuales de generación 1 y generación 2.

|Versiones de la serie de kernel compatibles con Red Hat|Generación 1|Generación 2|
|---------------------------------------------|----------------|----------------|
|Serie Oracle Linux 7. x|&#10004;|&#10004;|
|Serie Oracle Linux 6. x|&#10004;| &#10006;|

En la tabla siguiente se muestran las versiones de kernel empresarial ininterrumpible que puede usar como sistema operativo invitado para las máquinas virtuales de generación 1 y generación 2.

|Versiones del kernel empresarial ininterrumpido (UEK)|Generación 1|Generación 2|
|--------------------------------------------------|----------------|----------------|
|Oracle Linux UEK R3 QU3|&#10004;| &#10006;|
|Oracle Linux UEK R3 QU2|&#10004;| &#10006;|
|Oracle Linux UEK R3 QU1|&#10004;| &#10006;|

Para obtener más información, vea [Oracle Linux máquinas virtuales en Hyper-V](../Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md).

### <a name="suse-guest-operating-system-support"></a>Compatibilidad con sistemas operativos invitados SUSE

En la tabla siguiente se muestran las versiones de SUSE que puede usar como sistema operativo invitado para las máquinas virtuales de generación 1 y generación 2.

|Versiones de sistema operativo|Generación 1|Generación 2|
|-----------------------------|----------------|----------------|
|Serie SUSE Linux Enterprise Server 12|&#10004;|&#10004;|
|Serie SUSE Linux Enterprise Server 11|&#10004;| &#10006;|
|Abra SUSE 12,3|&#10004;| &#10006;|

Para obtener más información, consulte [máquinas virtuales de SUSE en Hyper-V](../Supported-SUSE-virtual-machines-on-Hyper-V.md).

### <a name="ubuntu-guest-operating-system-support"></a>Compatibilidad con el sistema operativo invitado de Ubuntu

En la tabla siguiente se muestran las versiones de Ubuntu que puede usar como sistema operativo invitado para las máquinas virtuales de generación 1 y generación 2.

|Versiones de sistema operativo|Generación 1|Generación 2|
|-----------------------------|----------------|----------------|
|Ubuntu 14,04 y versiones posteriores|&#10004;|&#10004;|
|Ubuntu 12.04|&#10004;| &#10006;|

Para obtener más información, consulte las [máquinas virtuales de Ubuntu en Hyper-V](../Supported-Ubuntu-virtual-machines-on-Hyper-V.md).

## <a name="how-can-i-boot-the-virtual-machine"></a>¿Cómo se puede arrancar la máquina virtual?

En la tabla siguiente se muestran los métodos de arranque que son compatibles con las máquinas virtuales de generación 1 y generación 2.

|Método de arranque|Generación 1|Generación 2|
|---------------|----------------|----------------|
|Arranque PXE con un adaptador de red estándar| &#10006;|&#10004;|
|Arranque PXE con un adaptador de red heredado|&#10004;| &#10006;|
|Arranque desde un disco duro virtual SCSI (. VHDX) o DVD virtual (. NORMAS| &#10006;|&#10004;|
|Arranque desde el disco duro virtual de la controladora IDE (. VHD) o DVD virtual (. NORMAS|&#10004;| &#10006;|
|Arranque desde disquete (. VFD|&#10004;| &#10006;|

## <a name="what-are-the-advantages-of-using-generation-2-virtual-machines"></a>¿Cuáles son las ventajas de usar las máquinas virtuales de generación 2?

Estas son algunas de las ventajas que obtendrá al usar una máquina virtual de generación 2:
- **Arranque seguro** Se trata de una característica que comprueba que el cargador de arranque está firmado por una autoridad de confianza en la base de datos UEFI para evitar que el firmware no autorizado, los sistemas operativos o los controladores UEFI se ejecuten durante el arranque. Arranque seguro está habilitado de manera predeterminada para máquinas virtuales de generación 2. Si necesita ejecutar un sistema operativo invitado que no sea compatible con el arranque seguro, puede deshabilitarlo una vez creada la máquina virtual.  Para obtener más información, consulta [Arranque seguro](/previous-versions/windows/it-pro/windows-8.1-and-8/dn486875(v=ws.11)).

    Para proteger las máquinas virtuales Linux de generación de arranque 2, debe elegir la plantilla de arranque seguro de CA de UEFI al crear la máquina virtual.

- **Volumen de arranque más grande** El volumen de arranque máximo para las máquinas virtuales de generación 2 es de 64 TB. Es el tamaño máximo de disco admitido por. VHDX. En el caso de las máquinas virtuales de generación 1, el volumen de arranque máximo es de 2 TB para un. VHDX y 2040GB para. VHD. Para obtener más información, vea [información general sobre el formato de disco duro virtual de Hyper-V](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831446(v=ws.11)).

  También puede ver una ligera mejora en el arranque de la máquina virtual y los tiempos de instalación con máquinas virtuales de generación 2.

## <a name="whats-the-difference-in-device-support"></a>¿Cuál es la diferencia en la compatibilidad de dispositivos?

En la tabla siguiente se comparan los dispositivos disponibles entre las máquinas virtuales de generación 1 y generación 2.

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

### <a name="attach-or-add-a-dvd-drive"></a>Conectar o agregar una unidad de DVD

- No se puede conectar una unidad física de CD o DVD a una máquina virtual de generación 2. La unidad de DVD virtual de las máquinas virtuales de generación 2 solo admiten archivos de imagen ISO. Para crear un archivo de imagen ISO de un entorno de Windows, puede utilizar la herramienta de línea de comandos Oscdimg. Para más información, vea [Opciones de la línea de comandos de Oscdimg](/previous-versions/windows/it-pro/windows-8.1-and-8/hh824847(v=win.10)).
- Cuando se crea una nueva máquina virtual con el cmdlet New-VM de Windows PowerShell, la máquina virtual de generación 2 no tiene una unidad de DVD. Puede Agregar una unidad de DVD mientras se ejecuta la máquina virtual.

### <a name="use-uefi-firmware"></a>Usar el firmware UEFI

- No es necesario el firmware de arranque seguro o UEFI en el host de Hyper-V físico. Hyper-V proporciona firmware virtual a las máquinas virtuales que es independiente de lo que hay en el host de Hyper-V.
- El firmware UEFI de una máquina virtual de generación 2 no admite el modo de instalación para el arranque seguro.
- No se admite la ejecución de un shell UEFI u otras aplicaciones UEFI en una máquina virtual de generación 2. Desde el punto de vista técnico, es posible usar una shell de UEFI o aplicaciones UEFI que no sean de Microsoft, siempre que se compilen directamente desde los archivos de origen. Si estas aplicaciones no están firmadas digitalmente de forma adecuada, debe deshabilitar el arranque seguro para la máquina virtual.

### <a name="work-with-vhdx-files"></a>Trabajar con archivos VHDX

- Puede cambiar el tamaño de un archivo VHDX que contiene el volumen de arranque de una máquina virtual de generación 2 mientras se ejecuta la máquina virtual.
- No se admite ni se recomienda crear un archivo VHDX que sea de arranque para máquinas virtuales de generación 1 y de generación 2.
- La generación de máquina virtual es una propiedad de la máquina virtual, no una propiedad del disco duro virtual. Por lo tanto, no puede saber si un archivo VHDX fue creado por una máquina virtual de generación 1 o de generación 2.
- Un archivo VHDX creado con una máquina virtual de generación 2 se puede conectar a la controladora IDE o a la controladora SCSI de una máquina virtual de generación 1. Sin embargo, si se trata de un archivo VHDX de arranque, la máquina virtual de generación 1 no arrancará.

### <a name="use-ipv6-instead-of-ipv4"></a>Usar IPv6 en lugar de IPv4

De manera predeterminada, las máquinas virtuales de generación 2 usan IPv4. Para usar IPv6 en su lugar, ejecute el cmdlet [set-VMFirmware de](/powershell/module/hyper-v/set-vmfirmware) Windows PowerShell. Por ejemplo, el comando siguiente establece el protocolo preferido en IPv6 para una máquina virtual denominada TestVM:

```powershell
Set-VMFirmware -VMName TestVM -IPProtocolPreference IPv6
```

## <a name="add-a-com-port-for-kernel-debugging"></a>Agregar un puerto COM para la depuración del kernel

Los puertos COM no están disponibles en las máquinas virtuales de generación 2 hasta que los agregue. Puede hacerlo con Windows PowerShell o Instrumental de administración de Windows (WMI). En estos pasos se muestra cómo hacerlo con Windows PowerShell.

Para agregar un puerto COM:

1. Deshabilita arranque seguro. La depuración del kernel no es compatible con arranque seguro. Asegúrese de que la máquina virtual se encuentra en un estado desactivado y, a continuación, use el cmdlet [set-VMFirmware](/powershell/module/hyper-v/set-vmfirmware) . Por ejemplo, el siguiente comando deshabilita el arranque seguro en la máquina virtual TestVM:

    ```powershell
    Set-VMFirmware -Vmname TestVM -EnableSecureBoot Off
    ```

2. Agregue un puerto COM. Use el cmdlet [set-VMComPort](/powershell/module/hyper-v/set-vmcomport) para hacerlo. Por ejemplo, el comando siguiente configura el primer puerto COM de la máquina virtual, TestVM, para conectarse a la canalización con nombre, llamada testpipe, en el equipo local:

    ```powershell
    Set-VMComPort -VMName TestVM 1 \\.\pipe\TestPipe
    ```

> [!NOTE]
> Los puertos COM configurados no aparecen en la configuración de una máquina virtual en el administrador de Hyper-V.

## <a name="see-also"></a>Vea también

- [Máquinas virtuales Linux y FreeBSD en Hyper-V](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)
- [Usos de recursos locales en la máquina virtual de Hyper-V con VMConnect](../learn-more/Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)
- [Planear la escalabilidad de Hyper-V en Windows Server 2016](./plan-hyper-v-scalability-in-windows-server.md)
