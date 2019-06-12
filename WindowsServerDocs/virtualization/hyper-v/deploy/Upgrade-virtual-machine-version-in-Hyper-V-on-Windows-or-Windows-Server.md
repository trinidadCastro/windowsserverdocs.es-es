---
title: Actualizar versión de la máquina virtual de Hyper-V en Windows 10 o Windows Server
description: Proporciona instrucciones y consideraciones para actualizar la versión de una máquina virtual
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 897f2454-5aee-445c-a63e-f386f514a0f6
author: jasongerend
ms.author: jgerend
ms.date: 05/22/2019
ms.openlocfilehash: 160adc0e838cb732ba792cbdd7fd9fa200c68794
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810512"
---
# <a name="upgrade-virtual-machine-version-in-hyper-v-on-windows-10-or-windows-server"></a>Actualizar versión de la máquina virtual de Hyper-V en Windows 10 o Windows Server

>Se aplica a: Windows 10, Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

Disponer de las características más recientes de Hyper-V en las máquinas virtuales mediante la actualización de la versión de configuración. No es así hasta que:

- Actualice los hosts de Hyper-V para la versión más reciente de Windows o Windows Server.
- Actualice el nivel funcional del clúster.
- Está seguro de que no tendrá que mover la máquina virtual de vuelta a un host de Hyper-V que se ejecuta una versión anterior de Windows o Windows Server.

Para obtener más información, consulte [actualización gradual de clúster del sistema operativo](../../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md) y [realizar una actualización gradual de un clúster de hosts de Hyper-V en VMM](https://docs.microsoft.com/system-center/vmm/hyper-v-rolling-upgrade).

## <a name="step-1-check-the-virtual-machine-configuration-versions"></a>Paso 1: Comprobar las versiones de configuración de máquina virtual

1. En el Escritorio de Windows, haga clic en el botón Inicio y escriba cualquier parte del nombre **Windows PowerShell**.
2. Haga clic en Windows PowerShell y seleccione **ejecutar como administrador**.
3. Use la [Get-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm)cmdlet. Ejecute el siguiente comando para obtener las versiones de las máquinas virtuales.

```PowerShell
Get-VM * | Format-Table Name, Version
```

También puede ver la versión de configuración en el Administrador de Hyper-V, seleccione la máquina virtual y examinando el **resumen** ficha.

## <a name="step-2-upgrade-the-virtual-machine-configuration-version"></a>Paso 2: Actualizar la versión de configuración de máquina virtual

1. Apague la máquina virtual en Hyper-V Manager.
2. Seleccione la acción > Actualizar versión de configuración. Si esta opción no está disponible para la máquina virtual, a continuación, que está ya en la última versión de configuración compatible con el host de Hyper-V.

Para actualizar la versión de configuración de máquina virtual con Windows PowerShell, use el [actualización VMVersion](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion) cmdlet. Ejecute el siguiente comando donde vmname es el nombre de la máquina virtual.

```PowerShell
Update-VMVersion <vmname>
```

## <a name="supported-virtual-machine-configuration-versions"></a>Versiones de configuración de máquina virtual admitidas

Ejecute el cmdlet de PowerShell [Get VMHostSupportedVersion](https://docs.microsoft.com/powershell/module/hyper-v/get-vmhostsupportedversion) para ver qué versiones de configuración de máquina virtual es compatible con el Host de Hyper-V. Cuando se crea una máquina virtual, se crea con la versión de la configuración predeterminada. Para ver cuál es el valor predeterminado, ejecute el siguiente comando.

```PowerShell
Get-VMHostSupportedVersion -Default
```

Si necesita crear una máquina virtual que se puede mover a un Host de Hyper-V que ejecuta una versión anterior de Windows, use el [New-VM](https://docs.microsoft.com/powershell/module/hyper-v/new-vm) cmdlet con el parámetro - version. Por ejemplo, para crear una máquina virtual que se puede mover a un host de Hyper-V que ejecute Windows Server 2012 R2, ejecute el siguiente comando. Este comando crea una máquina virtual denominada "WindowsCV5" con una versión 5.0 de configuración.

```PowerShell
New-VM -Name "WindowsCV5" -Version 5.0
```

>[!NOTE]
>Puede importar las máquinas virtuales que se han creado para un host de Hyper-V que ejecuta una versión anterior de Windows o restaurar copia de seguridad. Si la versión de configuración de la máquina virtual no aparezca como compatible para su sistema operativo del host de Hyper-V en la tabla siguiente, tendrá que actualizar la versión de configuración de máquina virtual antes de iniciar la máquina virtual.

### <a name="supported-vm-configuration-versions-for-long-term-servicing-hosts"></a>Versiones admitidas de configuración de máquina virtual para hosts de mantenimiento a largo plazo

En la tabla siguiente se enumera las versiones de configuración de máquina virtual que se admiten en hosts que ejecutan una versión de mantenimiento a largo plazo de Windows.

| Versión de Windows de host de Hyper-V | 9.1 | 9.0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
| --- |---|---|---|---|---|---|---|---|---|---|
|Windows Server 2019|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Enterprise LTSC 2019|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Enterprise 2016 LTSB|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Enterprise 2015 LTSB|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|
|Windows Server 2012 R2|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|
|Windows 8.1|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|

### <a name="supported-vm-configuration-versions-for-semi-annual-channel-hosts"></a>Versiones admitidas de configuración de máquina virtual para hosts de canal semestral

En la tabla siguiente se enumera las versiones de configuración de máquina virtual de hosts que ejecutan una versión de canal semestral actualmente compatible de Windows. Para obtener más información sobre las versiones de canal semestral de Windows, visite las páginas siguientes para [Windows Server](../../../get-started-19/servicing-channels-19.md) y [Windows 10](https://docs.microsoft.com/windows/deployment/update/waas-overview#servicing-channels)

| Versión de Windows de host de Hyper-V | 9.1 | 9.0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
| --- |---|---|---|---|---|---|---|---|---|---|
| Actualización de Windows 10 puede 2019 (versión 1903) |&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;| &#10004;|
| Windows Server, versión 1903 |&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;| &#10004;|
|Windows Server, versión 1809|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|10 de octubre de 2018 de Windows Update (versión 1809)|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server, versión 1803|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|10 de abril de 2018 de Windows Update (versión 1803)|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Fall Creators Update (versión 1709)|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Creators Update (versión 1703)|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Actualización de aniversario de Windows 10 (versión 1607)|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|

## <a name="why-should-i-upgrade-the-virtual-machine-configuration-version"></a>¿Por qué debo actualizar la versión de configuración de máquina virtual?

Al mover o importar una máquina virtual en un equipo que ejecuta Hyper-V en Windows Server 2019, Windows Server 2016 o Windows 10, la máquina virtual "configuración no se actualiza automáticamente. Esto significa que puede devolver la máquina virtual a un host de Hyper-V que se ejecuta una versión anterior de Windows o Windows Server. Sin embargo, esto también significa que no se puede usar de algunas de las nuevas características de la máquina virtual hasta que actualice manualmente la versión de configuración. No se puede cambiar la versión de configuración de máquina virtual una vez que haya actualizado.

La versión de configuración de máquina virtual representa la compatibilidad de configuración de la máquina virtual, guarda el estado y los archivos de instantánea con la versión de Hyper-V. Cuando se actualiza la versión de configuración, cambie la estructura de archivos que se usa para almacenar la configuración de las máquinas virtuales y los archivos de punto de comprobación. También actualizar la versión de configuración a la versión más reciente compatible con ese host de Hyper-V. Las máquinas virtuales actualizadas usan un nuevo formato de archivo de configuración, que está diseñado para aumentar la eficacia de lectura y escritura de datos de configuración de máquina virtual. La actualización también reduce la posibilidad de daños en los datos si se produce un error de almacenamiento.

En la tabla siguiente se enumera las descripciones, las extensiones de nombre de archivo y las ubicaciones predeterminadas para cada tipo de archivo que se usa para las máquinas virtuales nuevas o actualizadas.

 |Tipos de archivos de máquina virtual | Descripción|
 |---|---|
|Configuración |Información de configuración de máquina virtual que se almacena en formato de archivo binario. <br /> Extensión de nombre de archivo: .vmcx <br /> Ubicación predeterminada: C:\Archivos de Programa\Microsoft\Windows\Hyper-V\Virtual Machines|
 |Estado de tiempo de ejecución|Máquina virtual información de estado de tiempo de ejecución que se almacena en formato de archivo binario. <br />Extensión de nombre de archivo: .vmrs y .vmgs <br />Ubicación predeterminada: C:\Archivos de Programa\Microsoft\Windows\Hyper-V\Virtual Machines|
|Disco duro virtual|Almacena los discos duros virtuales para la máquina virtual. <br /> Extensión de nombre de archivo: .vhd o .vhdx <br />Ubicación predeterminada: C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Hard Disks|
 |Automáticos de disco duro virtual |Archivos de disco utilizados para los puntos de control de máquina virtual de diferenciación. <br /> Extensión de nombre de archivo: .avhdx <br /> Ubicación predeterminada: C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Hard Disks|
 |Checkpoint|Los puntos de control se almacenan en varios archivos de puntos de control. Cada punto de control crea un archivo de configuración y un archivo de estado del tiempo de ejecución. <br /> Las extensiones de nombre de archivo: .vmrs y .vmcx <br />Ubicación predeterminada: C:\Archivos de programa\Microsoft\Windows\Snapshots|

## <a name="what-happens-if-i-dont-upgrade-the-virtual-machine-configuration-version"></a>¿Qué ocurre si no actualizo la versión de configuración de máquina virtual?

Si tiene máquinas virtuales que ha creado con una versión anterior de Hyper-V, algunas características que están disponibles en el host más reciente que sistema operativo no funcionen con esas máquinas virtuales hasta que actualice la versión de configuración.

Como una guía general, se recomienda actualizar la versión de configuración una vez que se ha actualizado correctamente los hosts de virtualización a una versión más reciente de Windows y se sienta seguros de que no es necesario revertir. Cuando se utiliza el [actualización gradual de SO del clúster](https://docs.microsoft.com/windows-server/failover-clustering/Cluster-Operating-System-Rolling-Upgrade) característica, esto sería normalmente después de actualizar el nivel funcional del clúster. De este modo, se beneficiará de las nuevas características y cambios internos y también las optimizaciones.

>[!NOTE]
>Una vez que se actualiza la versión de configuración de máquina virtual, la máquina virtual no podrá iniciar en hosts que no admiten la versión de la configuración actualizada.

En la tabla siguiente se muestra la versión de configuración mínimo de la máquina virtual debe usar algunas características de Hyper-V.

|Característica|Versión mínima de configuración de máquina virtual|
|---|---|
|Agregar o quitar memoria en caliente|6.2|
|Arranque seguro para máquinas virtuales de Linux|6.2|
|Puntos de control de producción|6.2|
|PowerShell Direct |6.2|
|Agrupación de máquinas virtuales|6.2|
|Módulo de plataforma segura virtual (vTPM)|7.0|
|Máquina virtual con varias colas (VMMQ)|7.1|
|Compatibilidad con XSAVE|8.0|
|Unidad de almacenamiento de claves|8.0|
|Compatibilidad con la seguridad basada en virtualización de invitado (VBS)|8.0|
|Virtualización anidada|8.0|
|Número de procesadores virtuales|8.0|
|Gran cantidad de memoria de las máquinas virtuales|8.0|
|Aumentar el número máximo predeterminado para que los dispositivos virtuales 64 por dispositivo (por ejemplo, redes y asignados dispositivos)|8.3|
|Permitir características adicionales de procesador para el Monitor de rendimiento|9.0|
|Exponer automáticamente [simultáneas multithreading](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#background) configuración de máquinas virtuales que se ejecutan en hosts con el [programador Core](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler)|9.0|
|Compatibilidad con la hibernación|9.0|

Para obtener más información acerca de estas características, consulte [cuáles son las novedades en Hyper-V en Windows Server](../What-s-new-in-Hyper-V-on-Windows.md).

