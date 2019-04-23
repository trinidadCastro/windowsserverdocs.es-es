---
title: Estado del almacén de espacios directo y estados operativos
description: Cómo buscar y comprender el mantenimiento distintos y estados operativos de espacios de almacenamiento directo y espacios de almacenamiento (incluidos los discos físicos, grupos y discos virtuales) y qué hacer con ellos.
keywords: Espacios de almacenamiento, desasociados, disco virtual, disco físico, degradado
author: jasongerend
ms.author: jgerend
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-spaces
manager: brianlic
ms.openlocfilehash: 5090a68270438bd9a06c7d50f9d4abca066d31e6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849146"
---
# <a name="troubleshoot-storage-spaces-direct-health-and-operational-states"></a>Solucionar problemas de mantenimiento de espacios de almacenamiento directo y estados operativos

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canal semianual), Windows 10, Windows 8.1

En este tema se describe el estado y los Estados operativos de grupos de almacenamiento, los discos virtuales (que se encuentran debajo de los volúmenes en espacios de almacenamiento) y unidades de [espacios de almacenamiento directo](storage-spaces-direct-overview.md) y [espacios de almacenamiento](overview.md). Estos Estados pueden ser muy útil cuando se intenta solucionar diferentes problemas, por ejemplo, ¿por qué no se puede eliminar un disco virtual debido a una configuración de solo lectura. También se explica por qué no se puede agregar una unidad a un grupo (el CannotPoolReason).

Espacios de almacenamiento tiene tres objetos principales - *discos físicos* (unidades de disco duro, SSD, etc.) que se agregan a un *bloque de almacenamiento*, virtualizar el almacenamiento para que pueda crear *discos virtuales* de espacio disponible en el grupo, como se muestra aquí. Metadatos de un grupo se escriben en cada unidad en el grupo. Los volúmenes se crean encima de los discos virtuales y almacenan los archivos, pero no nos vamos a hablar acerca de los volúmenes aquí.

![Discos físicos se agregan a un grupo de almacenamiento y, a continuación, se crean discos virtuales desde el espacio de grupo](media/storage-spaces-states/storage-spaces-object-model.png)

Puede ver los Estados operativos y mantenimiento en el administrador del servidor o con PowerShell. Este es un ejemplo de una variedad de mantenimiento (principalmente errónea) y estados operativos en un clúster de espacios de almacenamiento directo que le falta la mayoría de sus nodos de clúster (haga clic en los encabezados de columna para agregar **estado operativo**). Esto no es un clúster feliz.

![Administrador del servidor que muestra los resultados de dos nodos que faltan en un clúster de espacios de almacenamiento directo: una gran cantidad de falta de discos físicos y los discos virtuales en un estado incorrecto](media/storage-spaces-states/unhealthy-disks-in-server-manager.png)

## <a name="storage-pool-states"></a>Estados del grupo de almacenamiento

Cada bloque de almacenamiento tiene un estado de mantenimiento - **correcto**, **advertencia**, o **desconocido**/**incorrecto**, así como uno o más Estados operativos.

Para averiguar qué estado que se encuentra un grupo, use los siguientes comandos de PowerShell:

```PowerShell
Get-StoragePool -IsPrimordial $False | Select-Object HealthStatus, OperationalStatus, ReadOnlyReason
```

Este es un ejemplo de salida que muestra un grupo de almacenamiento en el estado de mantenimiento desconocido con el estado operativo de solo lectura:

```
FriendlyName                OperationalStatus HealthStatus IsPrimordial IsReadOnly
------------                ----------------- ------------ ------------ ----------
S2D on StorageSpacesDirect1 Read-only         Unknown      False        True
```

Las siguientes secciones enumeran el estado y los Estados operativos.

### <a name="pool-health-state-healthy"></a>Estado de mantenimiento del grupo: Correcto

|Estado operativo    |Descripción|
|---------            |---------  |
|Aceptar|El grupo de almacenamiento es correcto.|

### <a name="pool-health-state-warning"></a>Estado de mantenimiento del grupo: Advertencia

Cuando el bloque de almacenamiento está en el **advertencia** el estado de mantenimiento, significa que el grupo es accesible, pero una o varias unidades no se pudo o faltan. Como resultado, es posible que el bloque de almacenamiento podría reduce resistencia.

|Estado operativo    |Descripción|
|---------            |---------  |
|Degradado|Hay unidades con errores o que falta en el bloque de almacenamiento. Esta condición se produce solo con unidades de metadatos de un grupo de hospedaje. <br><br>**Acción**: Comprobar el estado de las unidades de disco y reemplazar las unidades con errores antes de que hay errores adicionales.|

### <a name="pool-health-state-unknown-or-unhealthy"></a>Estado de mantenimiento del grupo: Desconocido o no es correcto

Cuando un grupo de almacenamiento está en el **desconocido** o **incorrecto** el estado de mantenimiento, significa que el bloque de almacenamiento es de solo lectura y no se puede modificar hasta que el grupo se devuelve a la **advertencia**o **Aceptar** Estados de mantenimiento.

|Estado operativo    |Motivo de solo lectura |Descripción|
|---------            |---------       |--------   |
|Solo lectura|Incompleta|Esto puede ocurrir si el bloque de almacenamiento pierde su [quórum](understand-quorum.md), lo que significa que la mayoría de las unidades en el grupo ha producido un error o está sin conexión por alguna razón. Cuando un grupo pierde el quórum, espacios de almacenamiento establece automáticamente la configuración del grupo a solo lectura hasta que suficientes unidades estén disponibles de nuevo.<br><br>**Acción:** <br>1. Volver a conectar las unidades que faltan y, si usa espacios de almacenamiento directo, ponga todos los servidores en línea. <br>2. Establezca el grupo de vuelta en lectura y escritura, abrir una sesión de PowerShell con permisos administrativos y, a continuación, escriba:<br><br> <code>Get-StoragePool <PoolName> -IsPrimordial $False \| Set-StoragePool -IsReadOnly $false</code>|
||Directiva|Un administrador definir el grupo de almacenamiento de sólo lectura.<br><br>**Acción:** Para configurar un grupo de almacenamiento en clúster en Administrador de clústeres de conmutación por error de acceso de lectura y escritura, vaya a **grupos**, haga clic en el grupo y, a continuación, seleccione **poner en línea**.<br><br>Para otros servidores y equipos, abra una sesión de PowerShell con permisos administrativos y, a continuación, escriba:<br><br><code>Get-StoragePool <PoolName> \| Set-StoragePool -IsReadOnly $false</code><br><br> |
||Starting|Espacios de almacenamiento está iniciando o esperando para que unidades de disco al estar conectado en el grupo. Debe tratarse de un estado temporal. Una vez que haya iniciado totalmente, el grupo debe realizar la transición a un estado operativo diferente.<br><br>**Acción:** Si el grupo se queda en el *iniciando* estado, asegúrese de que todas las unidades del grupo están conectadas correctamente.|

Vea también [modificar un grupo de almacenamiento que tiene una configuración de sólo lectura](https://social.technet.microsoft.com/wiki/contents/articles/14861.modifying-a-storage-pool-that-has-a-read-only-configuration.aspx).

## <a name="virtual-disk-states"></a>Estados del disco virtual

En espacios de almacenamiento, se colocan los volúmenes en discos virtuales (espacios de almacenamiento) que se extraen de espacio libre en un grupo. Cada disco virtual tiene un estado de mantenimiento - **correcto**, **advertencia**, **Unhealthy**, o **desconocido** , así como uno o varios de los Estados operativos.

Para averiguar qué discos virtuales de estado están en, utilice los siguientes comandos de PowerShell:

```PowerShell
Get-VirtualDisk | Select-Object FriendlyName,HealthStatus, OperationalStatus, DetachedReason
```

Este es un ejemplo de salida que muestra un disco virtual desasociado y un disco virtual degradado incompletos:

```
FriendlyName HealthStatus OperationalStatus      DetachedReason
------------ ------------ -----------------      --------------
Volume1      Unknown      Detached               By Policy
Volume2      Warning      {Degraded, Incomplete} None
```

Las siguientes secciones enumeran el estado y los Estados operativos.

### <a name="virtual-disk-health-state-healthy"></a>Estado de mantenimiento de disco virtual: Correcto

|Estado operativo    |Descripción|
|---------            |---------          |
|Aceptar    |El disco virtual es correcto.|
|No óptimo    |No se escriben datos uniformemente en las unidades. <br><br>**Acción**: Optimizar el uso de la unidad en el bloque de almacenamiento mediante la ejecución de la [Optimize-StoragePool](https://technet.microsoft.com/itpro/powershell/windows/storage/optimize-storagepool) cmdlet.|

### <a name="virtual-disk-health-state-warning"></a>Estado de mantenimiento de disco virtual: Advertencia

Cuando el disco virtual está en un **advertencia** el estado de mantenimiento, significa que uno o más copias de los datos no están disponibles, pero los espacios de almacenamiento puede seguir leyendo al menos una copia de los datos.

|Estado operativo    |Descripción|
|---------            |---------          |
|En el servicio            |Windows está reparando el disco virtual, como después de agregar o quitar una unidad. Una vez completada la reparación, debe devolver el disco virtual para el estado correcto.|
|Incompleta           |La resistencia del disco virtual se reduce porque faltan una o varias unidades o error. Sin embargo, las unidades que faltan contienen copias actualizadas de los datos.<br><br> **Acción**: <br>1. Volver a conectar las unidades que faltan, reemplace las unidades con errores y, si usa espacios de almacenamiento directo, poner en línea todos los servidores que están sin conexión. <br>2. Si no usa espacios de almacenamiento directo, a continuación reparar el disco virtual con el [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet.<br> Espacios de almacenamiento directo se inicia automáticamente una reparación si es necesario después de volver a conectar o reemplazar una unidad.|
|Degradado             |La resistencia del disco virtual se reduce porque faltan una o varias unidades o no se pudo, y hay copias obsoletos de los datos en estas unidades. <br><br>**Acción**: <br> 1. Volver a conectar las unidades que faltan, reemplace las unidades con errores y, si usa espacios de almacenamiento directo, poner en línea todos los servidores que están sin conexión. <br> 2. Si no usa espacios de almacenamiento directo, a continuación reparar el disco virtual con el [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet. <br>Espacios de almacenamiento directo se inicia automáticamente una reparación si es necesario después de volver a conectar o reemplazar una unidad.|

### <a name="virtual-disk-health-state-unhealthy"></a>Estado de mantenimiento de disco virtual: Incorrecto

Cuando un disco virtual está en un **incorrecto** estado de mantenimiento de algunos o todos los datos en el disco virtual son accesibles en este momento.

|Estado operativo    |Descripción|
|---------            |--------   |
|No hay redundancia|El disco virtual ha perdido datos debido a un error demasiadas unidades.<br><br>**Acción**: Reemplace las unidades con errores y, a continuación, restaurar los datos de copia de seguridad.|

### <a name="virtual-disk-health-state-informationunknown"></a>Estado de mantenimiento de disco virtual: Información o desconocido

También puede ser el disco virtual en el **información** estado de mantenimiento (como se muestra en el elemento del Panel de Control de espacios de almacenamiento) o **desconocido** health state (como se muestra en PowerShell) si un administrador tardó el disco virtual sin conexión o se ha convertido en desasociar el disco virtual.

|Estado operativo    |Motivo desasociado |Descripción|
|---------            |---------       |--------   |
|Desasociado             |Mediante la directiva            |Un administrador ha tomado el disco virtual sin conexión o establece el disco virtual que requiere la inserción manual, en cuyo caso debe asociar manualmente el disco virtual cada vez que se reinicie Windows.,<br><br>**Acción**: Ponga en línea el disco virtual. Para realizar esta acción cuando el disco virtual está en un grupo de almacenamiento en clúster, seleccione Administrador de clústeres de conmutación por error **almacenamiento** > **grupos** > **discos virtuales** , seleccione el disco virtual que muestra la **Offline** estado y, a continuación, seleccione **poner en línea**. <br><br>Para poner en línea en un disco virtual cuando no esté en un clúster, abra una sesión de PowerShell como administrador y, a continuación, intente usar el comando siguiente:<br><br> <code>Get-VirtualDisk \| Where-Object -Filter { $_.OperationalStatus -eq "Detached" } \| Connect-VirtualDisk</code><br><br>Para asociar automáticamente todos los discos virtuales no agrupado después de reiniciar Windows, abra una sesión de PowerShell como administrador y, a continuación, use el siguiente comando:<br><br> <code>Get-VirtualDisk \| Set-VirtualDisk -ismanualattach $false</code>|
|            |Discos de mayoría en mal estado |Demasiadas unidades utilizadas por este disco virtual no se pudo, faltan o tienen datos obsoletos.   <br><br>**Acción**: <br> 1. Volver a conectar las unidades que faltan y, si usa espacios de almacenamiento directo, poner en línea todos los servidores que están sin conexión. <br> 2. Después de que todos los servidores y las unidades están en línea, reemplace las unidades con errores. Consulte [servicio de mantenimiento](../../failover-clustering/health-service-overview.md) para obtener más información. <br>Espacios de almacenamiento directo se inicia automáticamente una reparación si es necesario después de volver a conectar o reemplazar una unidad.<br>3. Si no usa espacios de almacenamiento directo, a continuación reparar el disco virtual con el [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet.  <br><br>Si no se pudo más discos que tienen copias de los datos y el disco virtual no estaba reparados errores intermedias, todos los datos en el disco virtual se perderá permanentemente. En este caso desafortunado, eliminar el disco virtual, cree un nuevo disco virtual y, a continuación, restaure una copia de seguridad.|
|                     |Incompleta |No hay suficientes unidades estén presentes para leer el disco virtual.    <br><br>**Acción**: <br> 1. Volver a conectar las unidades que faltan y, si usa espacios de almacenamiento directo, poner en línea todos los servidores que están sin conexión. <br> 2. Después de que todos los servidores y las unidades están en línea, reemplace las unidades con errores. Consulte [servicio de mantenimiento](../../failover-clustering/health-service-overview.md) para obtener más información. <br>Espacios de almacenamiento directo se inicia automáticamente una reparación si es necesario después de volver a conectar o reemplazar una unidad.<br>3. Si no usa espacios de almacenamiento directo, a continuación reparar el disco virtual con el [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet.<br><br>Si no se pudo más discos que tienen copias de los datos y el disco virtual no estaba reparados errores intermedias, todos los datos en el disco virtual se perderá permanentemente. En este caso desafortunado, eliminar el disco virtual, cree un nuevo disco virtual y, a continuación, restaure una copia de seguridad.|
| |tiempoDeEspera|Conectar el disco virtual tardó demasiado tiempo <br><br> **Acción:** Esto no debería ocurrir a menudo, por lo que podría intentar ver si la condición se pasa en el tiempo. O bien puede intentar desconectar el disco virtual con el [Disconnect-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/disconnect-virtualdisk?view=win10-ps) cmdlet, a continuación, usa el [Connect-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/connect-virtualdisk?view=win10-ps) para volver a conectarse a él. |

## <a name="drive-physical-disk-states"></a>Estados de unidad (disco físico)

Las secciones siguientes describen los Estados de mantenimiento puede ser una unidad en. Las unidades en un grupo se representan en PowerShell como *disco físico* objetos.

### <a name="drive-health-state-healthy"></a>Estado de mantenimiento de la unidad: Correcto

|Estado operativo    |Descripción|
|---------            |---------          |
|Aceptar|La unidad es correcta.|
|En el servicio|La unidad es realizar algunas operaciones de mantenimiento interno. Una vez completada la acción, la unidad debe devolver a la *Aceptar* estado de mantenimiento.|

### <a name="drive-health-state-warning"></a>Estado de mantenimiento de la unidad: Advertencia

Una unidad en el puede de estado de advertencia leer y escribir los datos correctamente, pero tiene un problema.

|Estado operativo    |Descripción|
|---------            |---------          |
|Pérdida de comunicación|Falta la unidad. Si usa espacios de almacenamiento directo, puede que un servidor está inactivo.<br><br>**Acción**: Si usa espacios de almacenamiento directo, hacer que todos los servidores en línea. Si no se soluciona, vuelva a conectar la unidad, reemplazarlo o intentar obtener la información de diagnóstico detallada sobre esta unidad siguiendo los pasos de solución de problemas con el informe de errores de Windows > [agotó el disco físico](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-timed-out).|
|Quitando del grupo|Espacios de almacenamiento está en proceso de quitar la unidad de su grupo de almacenamiento. <br><br> Se trata de un estado temporal. Cuando la eliminación se complete, si todavía está asociado el disco al sistema, la unidad realiza la transición a otro estado operativo (generalmente Aceptar) en un grupo primordial.|
|Modo de mantenimiento inicial|Espacios de almacenamiento está en proceso de poner la unidad en modo de mantenimiento después de que un administrador poner la unidad en modo de mantenimiento. Este es un estado temporal: la unidad debe estar pronto en el *en modo de mantenimiento* estado.|
|En modo de mantenimiento|Un administrador coloca la unidad en modo de mantenimiento, cómo detener lee y escribe desde la unidad. Esto se suele hacer antes de actualizar el firmware de la unidad o errores de pruebas.<br><br>**Acción**: Para aprovechar la unidad del modo de mantenimiento, utilice el [Disable StorageMaintenanceMode](https://technet.microsoft.com/itpro/powershell/windows/storage/disable-storagemaintenancemode) cmdlet.|
|Detener modo de mantenimiento|Un administrador tardó la unidad del modo de mantenimiento y espacios de almacenamiento está en proceso de poner en línea la unidad. Se trata de un estado temporal: la unidad pronto debe ser lo ideal es que en otro estado - *correcto*.|
|Error predictivo|La unidad informó de que está cerca del error.<br><br>**Acción**: Reemplazar la unidad.|
|Error de E/S|Se produjo un error temporal al acceder a la unidad.<br><br>**Acción**: <br>1. Si la unidad no realizar la transición a la **Aceptar** de estado, puede intentar usar el [Reset-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) cmdlet para borrar la unidad. <br> 2. Use [Repair-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk) para restaurar la resistencia de los discos virtuales afectados. <br>3. Si esto se sigue produciendo, reemplace la unidad.|
|Error transitorio|Se produjo un error con la unidad temporal. Esto suele significar la unidad no responde, pero también podría significar que los espacios de almacenamiento partición protectora incorrectamente se quitó de la unidad. <br><br>**Acción**: <br>1. Si la unidad no realizar la transición a la **Aceptar** de estado, puede intentar usar el [Reset-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) cmdlet para borrar la unidad. <br> 2. Use [Repair-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk) para restaurar la resistencia de los discos virtuales afectados. <br>3. Si esto se sigue produciendo, reemplace el disco o intentar obtener la información de diagnóstico detallada sobre esta unidad siguiendo los pasos de solución de problemas con el informe de errores de Windows > [disco físico no se pudo poner en línea](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online).|
|Latencia anómala|La unidad se ejecutan con lentitud, medido por el servicio de mantenimiento de espacios de almacenamiento directo.<br><br>**Acción**: Si esto se sigue produciendo, reemplace la unidad, por lo que no reduce el rendimiento de espacios de almacenamiento como un todo.

### <a name="drive-health-state-unhealthy"></a>Estado de mantenimiento de la unidad: Incorrecto

Actualmente se no se escribirá o tener acceso a una unidad en un estado incorrecto.

|Estado operativo    |Descripción|
|---------            |---------          |
|No se puede usar|Esta unidad no se puede usar espacios de almacenamiento. Para obtener más información, consulte [requisitos de hardware de espacios de almacenamiento directo](storage-spaces-direct-hardware-requirements.md); si no usa espacios de almacenamiento directo, consulte [Introducción a los espacios de almacenamiento](https://technet.microsoft.com/library/hh831739(v=ws.11).aspx#Requirements).|
|División|La unidad se convertirá en separado desde el grupo.<br><br>**Acción**: Restablecimiento de la unidad, borrar todos los datos de la unidad y agregándolo al grupo como una unidad vacía. Para ello, abra una sesión de PowerShell como administrador, ejecute el [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) cmdlet y, a continuación, ejecute [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk). <br><br>Para obtener información de diagnóstico detallada sobre esta unidad, siga los pasos de solución de problemas con el informe de errores de Windows > [disco físico no se pudo poner en línea](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online).|
|Metadatos obsoletos|Espacios de almacenamiento encuentra metadatos anterior en la unidad.<br><br>**Acción**: Debe tratarse de un estado temporal. Si la unidad no realizar la transición a correcto, puede ejecutar [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) iniciar una operación de reparación en los discos virtuales afectados. Si esto no resuelve el problema, puede restablecer la unidad con la [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) cmdlet, borrar todos los datos de la unidad y, a continuación, ejecute [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk).|
|Metadatos no reconocidos|Espacios de almacenamiento encontró metadatos no reconocidos en la unidad, lo que suele significa que la unidad tiene metadatos de un grupo diferente en él.<br><br>**Acción**: Para borrar la unidad y lo agrega al grupo actual, restablecer la unidad. Para restablecer la unidad, abra una sesión de PowerShell como administrador, ejecute el [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) cmdlet y, a continuación, ejecute [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk).|
|Error en los medios|La unidad de error y no puede utilizarse por espacios de almacenamiento más.<br><br>**Acción**: Reemplazar la unidad. <br><br>Para obtener información de diagnóstico detallada sobre esta unidad, siga los pasos de solución de problemas con el informe de errores de Windows > [disco físico no se pudo poner en línea](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online).|
|Error de hardware de dispositivo|Se produjo un error de hardware en esta unidad. <br><br>**Acción**: Reemplazar la unidad.|
|La actualización del firmware|Windows está actualizando el firmware en la unidad. Se trata de un estado temporal, que suele durar menos de un minuto y durante el cual otras unidades en el grupo de controlan todo el tiempo, lecturas y escrituras. Para obtener más información, consulte [actualizar firmware de la unidad](../update-firmware.md).|
|Starting|La unidad se está preparando para la operación. Debe tratarse de un estado temporal - una vez que haya finalizado, que la unidad debe realizar la transición a un estado operativo diferente.|

## <a name="reasons-a-drive-cant-be-pooled"></a>No se puede agrupar una unidad de motivos

Algunas unidades simplemente no está listos para estar en un grupo de almacenamiento. Puede averiguar por qué una unidad no es válida para la agrupación examinando el `CannotPoolReason` propiedad de un disco físico. Este es un ejemplo de script de PowerShell para mostrar la propiedad CannotPoolReason:

```PowerShell
Get-PhysicalDisk | Format-Table FriendlyName,MediaType,Size,CanPool,CannotPoolReason
```

Este es un ejemplo de salida:

```
FriendlyName          MediaType          Size CanPool CannotPoolReason
------------          ---------          ---- ------- ----------------
ATA MZ7LM120HCFD00D3  SSD        120034123776   False Insufficient Capacity
Msft Virtual Disk     SSD         10737418240    True
Generic Physical Disk SSD        119990648832   False In a Pool
```

En la tabla siguiente ofrece un poco más detalle en cada uno de los motivos.

|Razón|Descripción|
|---|---|
|En un grupo|La unidad ya pertenece a un grupo de almacenamiento. <br><br>Las unidades pueden pertenecer a solo un único grupo de almacenamiento a la vez. Para usar esta unidad en otro grupo de almacenamiento, quite primero la unidad de su grupo existente, lo que indica a espacios de almacenamiento para mover los datos en la unidad a otras unidades en el grupo. O restablezca la unidad si la unidad se ha desconectado de su grupo sin notificar a los espacios de almacenamiento. <br><br>Para quitar una unidad de forma segura desde un bloque de almacenamiento, utilice [Remove-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/remove-physicaldisk), o bien vaya al administrador del servidor > **File and Storage Services** > **grupos de almacenamiento**, > **Discos físicos**, haga clic en la unidad y, a continuación, seleccione **Quitar disco**.<br><br>Para restablecer una unidad, use [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk).|
|No es correcto|La unidad no está en un estado correcto y es posible que deba reemplazarse.|
|Medios extraíbles|La unidad se clasifica como una unidad extraíble. <br><br>Espacios de almacenamiento no es compatible con los medios que son reconocidos por Windows como extraíbles, como las unidades de disco Blu-Ray. Aunque muchas se ha corregido unidades son en ranuras extraíbles, en general, medios *clasificado* por Windows como extraíbles no son adecuadas para su uso con espacios de almacenamiento.|
|En uso por clúster|La unidad actualmente está usando un clúster de conmutación por error.|
|Desconectado|La unidad está sin conexión. <br><br>Para poner sin conexión todas las unidades de disco en línea y se establece en lectura/escritura, abra una sesión de PowerShell como administrador y use los siguientes scripts:<br><br><code>Get-Disk \| Where-Object -Property OperationalStatus -EQ "Offline" \| Set-Disk -IsOffline $false</code><br><br><code>Get-Disk \| Where-Object -Property IsReadOnly -EQ $true \| Set-Disk -IsReadOnly $false</code>|
|Capacidad insuficiente|Esto suele ocurrir cuando hay particiones ocupando todo el espacio libre en la unidad. <br><br>**Acción**: Elimine los volúmenes en la unidad, borrar todos los datos en la unidad. Una manera de hacerlo es usar el [Clear-Disk](https://docs.microsoft.com/powershell/module/storage/clear-disk?view=win10-ps) cmdlet de PowerShell.|
|Comprobación en curso|El [servicio de mantenimiento](../../failover-clustering/health-service-overview.md#supported-components-document) está comprobando para ver si se aprueba la unidad o el firmware en la unidad para su uso por el administrador del servidor.|
|Error de comprobación|El [servicio de mantenimiento](../../failover-clustering/health-service-overview.md#supported-components-document) no se pudo comprobar para ver si se aprueba la unidad o el firmware en la unidad para su uso por el administrador del servidor.|
|Firmware no compatible|El firmware de la unidad física no se encuentra en la lista de las revisiones de firmware aprobado especificados por el administrador del servidor utilizando el [servicio de mantenimiento](../../failover-clustering/health-service-overview.md#supported-components-document). |
|Hardware no compatible|No se encuentra la unidad en la lista de modelos de almacenamiento aprobadas especificados por el administrador del servidor utilizando el [servicio de mantenimiento](../../failover-clustering/health-service-overview.md#supported-components-document).|

## <a name="see-also"></a>Vea también

- [Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
- [Requisitos de hardware de almacenamiento directo en espacios](storage-spaces-direct-hardware-requirements.md)
- [Quórum de clúster y grupo de descripción](understand-quorum.md)