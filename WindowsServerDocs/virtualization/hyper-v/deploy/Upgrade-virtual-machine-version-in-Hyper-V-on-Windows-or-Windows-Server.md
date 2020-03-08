---
title: Actualización de la versión de la máquina virtual en Hyper-V en Windows 10 o Windows Server
description: Proporciona instrucciones y consideraciones para actualizar la versión de una máquina virtual
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 897f2454-5aee-445c-a63e-f386f514a0f6
author: jasongerend
ms.author: jgerend
ms.date: 05/22/2019
ms.openlocfilehash: 96678dfab2a3d5b6f503d8ce9d00850a3c437b35
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78370609"
---
# <a name="upgrade-virtual-machine-version-in-hyper-v-on-windows-10-or-windows-server"></a>Actualización de la versión de la máquina virtual en Hyper-V en Windows 10 o Windows Server

>Se aplica a: Windows 10, Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

Haga que las características de Hyper-V más recientes estén disponibles en las máquinas virtuales mediante la actualización de la versión de configuración. No lo haga hasta:

- Actualice los hosts de Hyper-V a la versión más reciente de Windows o Windows Server.
- Actualice el nivel funcional del clúster.
- Está seguro de que no necesitará volver a migrar la máquina virtual a un host de Hyper-V que ejecute una versión anterior de Windows o Windows Server.

Para obtener más información, consulte [actualización gradual del sistema operativo de clúster](../../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md) y [realizar una actualización gradual de un clúster de hosts de Hyper-V en VMM](https://docs.microsoft.com/system-center/vmm/hyper-v-rolling-upgrade).

## <a name="step-1-check-the-virtual-machine-configuration-versions"></a>Paso 1: comprobar las versiones de configuración de la máquina virtual

1. En el Escritorio de Windows, haga clic en el botón Inicio y escriba cualquier parte del nombre **Windows PowerShell**.
2. Haga clic con el botón derecho en Windows PowerShell y seleccione **Ejecutar como administrador**.
3. Use el cmdlet [Get-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm). Ejecute el siguiente comando para obtener las versiones de las máquinas virtuales.

```PowerShell
Get-VM * | Format-Table Name, Version
```

También puede ver la versión de configuración en el administrador de Hyper-V. para ello, seleccione la máquina virtual y mire en la pestaña **Resumen** .

## <a name="step-2-upgrade-the-virtual-machine-configuration-version"></a>Paso 2: actualización de la versión de configuración de la máquina virtual

1. Apague la máquina virtual en el administrador de Hyper-V.
2. Seleccione acción > Actualizar versión de configuración. Si esta opción no está disponible para la máquina virtual, ya está en la versión de configuración más alta compatible con el host de Hyper-V.

Para actualizar la versión de configuración de la máquina virtual mediante Windows PowerShell, use el cmdlet [Update-VMVersion](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion) . Ejecute el siguiente comando, donde vmname es el nombre de la máquina virtual.

```PowerShell
Update-VMVersion <vmname>
```

## <a name="supported-virtual-machine-configuration-versions"></a>Versiones de configuración de máquina virtual admitidas

Ejecute el cmdlet de PowerShell [Get-VMHostSupportedVersion](https://docs.microsoft.com/powershell/module/hyper-v/get-vmhostsupportedversion) para ver qué versiones de configuración de máquina virtual admite el host de Hyper-V. Cuando se crea una máquina virtual, se crea con la versión de configuración predeterminada. Para ver cuál es el valor predeterminado, ejecute el siguiente comando.

```PowerShell
Get-VMHostSupportedVersion -Default
```

Si necesita crear una máquina virtual que pueda migrar a un host de Hyper-V que ejecute una versión anterior de Windows, use el cmdlet [New-VM](https://docs.microsoft.com/powershell/module/hyper-v/new-vm) con el parámetro-Version. Por ejemplo, para crear una máquina virtual que pueda migrar a un host de Hyper-V que ejecute Windows Server 2012 R2, ejecute el siguiente comando. Este comando creará una máquina virtual denominada "WindowsCV5" con una versión de configuración 5,0.

```PowerShell
New-VM -Name "WindowsCV5" -Version 5.0
```

>[!NOTE]
>Puede importar las máquinas virtuales que se han creado para un host de Hyper-V que ejecuta una versión anterior de Windows o restaurarlas a partir de una copia de seguridad. Si la versión de configuración de la máquina virtual no aparece como compatible con el sistema operativo del host de Hyper-V en la tabla siguiente, tendrá que actualizar la versión de configuración de la máquina virtual para poder iniciar la máquina virtual.

### <a name="supported-vm-configuration-versions-for-long-term-servicing-hosts"></a>Versiones de configuración de máquina virtual admitidas para hosts de mantenimiento a largo plazo

En la tabla siguiente se enumeran las versiones de configuración de máquina virtual que se admiten en los hosts que ejecutan una versión de mantenimiento a largo plazo de Windows.

| Versión de Windows del host de Hyper-V | 9,1 | 9,0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
| --- |---|---|---|---|---|---|---|---|---|---|
|Windows Server 2019|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Enterprise LTSC 2019|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Enterprise 2016 LTSB|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Enterprise 2015 LTSB|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|
|Windows Server 2012 R2|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|
|Windows 8.1|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|

### <a name="supported-vm-configuration-versions-for-semi-annual-channel-hosts"></a>Versiones de configuración de máquina virtual admitidas para hosts de canal semianual

En la tabla siguiente se enumeran las versiones de configuración de máquina virtual para hosts que ejecutan una versión de canal semianual compatible actualmente de Windows. Para obtener más información sobre las versiones de canal semianual de Windows, visite las siguientes páginas para [Windows Server](../../../get-started-19/servicing-channels-19.md) y [Windows 10](https://docs.microsoft.com/windows/deployment/update/waas-overview#servicing-channels) .

| Versión de Windows del host de Hyper-V | 9,1 | 9,0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
| --- |---|---|---|---|---|---|---|---|---|---|
| Actualización 2019 de Windows 10 (versión 1903) |&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;| &#10004;|
| Windows Server, versión 1903 |&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;| &#10004;|
|Windows Server, versión 1809|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Actualización 2018 de octubre de Windows 10 (versión 1809)|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server, versión 1803|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Actualización 2018 de abril de Windows 10 (versión 1803)|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Fall Creators Update (versión 1709)|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Creators Update (versión 1703)|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Actualización de aniversario de Windows 10 (versión 1607)|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|

## <a name="why-should-i-upgrade-the-virtual-machine-configuration-version"></a>¿Por qué debo actualizar la versión de configuración de la máquina virtual?

Al migrar o importar una máquina virtual en un equipo que ejecuta Hyper-V en Windows Server 2019, Windows Server 2016 o Windows 10, la configuración de la máquina virtual no se actualiza automáticamente. Esto significa que puede devolver la máquina virtual a un host de Hyper-V que ejecute una versión anterior de Windows o Windows Server. Sin embargo, esto también significa que no puede usar algunas de las nuevas características de máquina virtual hasta que actualice manualmente la versión de configuración. No se puede cambiar la versión de configuración de la máquina virtual después de haberla actualizado.

La versión de configuración de la máquina virtual representa la compatibilidad de la configuración de la máquina virtual, el estado guardado y los archivos de instantánea con la versión de Hyper-V. Al actualizar la versión de configuración, se cambia la estructura de archivos que se usa para almacenar la configuración de las máquinas virtuales y los archivos de punto de comprobación. También se actualiza la versión de configuración a la versión más reciente admitida por el host de Hyper-V. Las máquinas virtuales actualizadas usan un nuevo formato de archivo de configuración, que está diseñado para aumentar la eficacia de lectura y escritura de datos de configuración de máquina virtual. La actualización también reduce la posibilidad de daños en los datos si se produce un error de almacenamiento.

En la tabla siguiente se enumeran las descripciones, las extensiones de nombre de archivo y las ubicaciones predeterminadas para cada tipo de archivo que se usa para las máquinas virtuales nuevas o actualizadas.

 |Tipos de archivo de máquina virtual | Descripción|
 |---|---|
|Configuración |Información de configuración de la máquina virtual que se almacena en formato de archivo binario. <br /> Extensión de nombre de archivo:. vmcx <br /> Ubicación predeterminada: máquinas Programa\microsoft\windows\hyper-v\virtual|
 |Estado de tiempo de ejecución|Información de estado de tiempo de ejecución de la máquina virtual que se almacena en formato de archivo binario. <br />Extensión de nombre de archivo:. VMRS y. vmgs <br />Ubicación predeterminada: máquinas Programa\microsoft\windows\hyper-v\virtual|
|Disco duro virtual|Almacena los discos duros virtuales de la máquina virtual. <br /> Extensión de nombre de archivo:. vhd o. vhdx <br />Ubicación predeterminada: discos duros de Programa\microsoft\windows\hyper-v\virtual|
 |Disco duro virtual automático |Archivos de disco de diferenciación usados para los puntos de control de la máquina virtual. <br /> Extensión de nombre de archivo:. avhdx <br /> Ubicación predeterminada: discos duros de Programa\microsoft\windows\hyper-v\virtual|
 |Punto de control|Los puntos de control se almacenan en varios archivos de puntos de control. Cada punto de control crea un archivo de configuración y un archivo de estado del tiempo de ejecución. <br /> Extensiones de nombre de archivo:. VMRS y. vmcx <br />Ubicación predeterminada: Programa\microsoft\windows\snapshots|

## <a name="what-happens-if-i-dont-upgrade-the-virtual-machine-configuration-version"></a>¿Qué ocurre si no actualizo la versión de configuración de la máquina virtual?

Si tiene máquinas virtuales que creó con una versión anterior de Hyper-V, es posible que algunas características disponibles en el sistema operativo del host más reciente no funcionen con esas máquinas virtuales hasta que actualice la versión de configuración.

Como guía general, se recomienda actualizar la versión de configuración una vez que haya actualizado correctamente los hosts de virtualización a una versión más reciente de Windows y esté seguro de que no necesita revertir. Cuando se usa la característica de [actualización gradual de SO del clúster](https://docs.microsoft.com/windows-server/failover-clustering/Cluster-Operating-System-Rolling-Upgrade) , esto suele ser después de actualizar el nivel funcional del clúster. De este modo, también podrá beneficiarse de las nuevas características, así como de las optimizaciones y los cambios internos.

>[!NOTE]
>Una vez actualizada la versión de configuración de la máquina virtual, la máquina virtual no podrá iniciarse en los hosts que no admitan la versión de configuración actualizada.

En la tabla siguiente se muestra la versión mínima de configuración de máquina virtual necesaria para usar algunas características de Hyper-V.

|Característica|Versión mínima de configuración de VM|
|---|---|
|Agregar o quitar memoria en caliente|6.2|
|Arranque seguro para máquinas virtuales de Linux|6.2|
|Puntos de control de producción|6.2|
|PowerShell Direct |6.2|
|Agrupación de máquinas virtuales|6.2|
|Módulo de plataforma segura virtual (vTPM)|7.0|
|Varias colas de máquinas virtuales (VMMQ)|7.1|
|Compatibilidad con XSAVE|8.0|
|Unidad de almacenamiento de claves|8.0|
|Compatibilidad con la seguridad basada en la virtualización de invitados (VBS)|8.0|
|Virtualización anidada|8.0|
|Recuento de procesadores virtuales|8.0|
|Máquinas virtuales de memoria grande|8.0|
|Aumentar el número máximo predeterminado de dispositivos virtuales a 64 por dispositivo (por ejemplo, redes y dispositivos asignados)|8.3|
|Permitir características de procesador adicionales para Perfmon|9,0|
|Exponer automáticamente la configuración [multithreading simultánea](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#background) para las máquinas virtuales que se ejecutan en hosts mediante el [programador principal](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler)|9,0|
|Compatibilidad con la hibernación|9,0|

Para obtener más información acerca de estas características, consulte [novedades de Hyper-V en Windows Server](../What-s-new-in-Hyper-V-on-Windows.md).

