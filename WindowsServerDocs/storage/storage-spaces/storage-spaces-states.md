---
title: Espacios de almacenamiento y Estados operativos y estado de Espacios de almacenamiento directo
description: Cómo buscar y comprender los distintos Estados operativos y de estado de Espacios de almacenamiento directo y espacios de almacenamiento (incluidos los discos físicos, los grupos y los discos virtuales) y qué hacer con ellos.
keywords: Espacios de almacenamiento, desconectados, disco virtual, disco físico, degradado
author: jasongerend
ms.author: jgerend
ms.date: 12/06/2019
ms.topic: article
ms.prod: windows-server
ms.technology: storage-spaces
manager: brianlic
ms.openlocfilehash: 40e820971f9d2ab0ba48fe30b100f07302ed7206
ms.sourcegitcommit: e817a130c2ed9caaddd1def1b2edac0c798a6aa2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2019
ms.locfileid: "74945280"
---
# <a name="troubleshoot-storage-spaces-and-storage-spaces-direct-health-and-operational-states"></a>Solución de problemas de espacios de almacenamiento y Estados operativos y estado de Espacios de almacenamiento directo

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canal semianual), Windows 10, Windows 8.1

En este tema se describen los Estados operativos y de mantenimiento de los grupos de almacenamiento, los discos virtuales (que se encuentran debajo de los volúmenes en espacios de almacenamiento) y las unidades de [espacios de almacenamiento directo](storage-spaces-direct-overview.md) y [espacios de almacenamiento](overview.md). Estos Estados pueden ser muy valiosos al intentar solucionar diversos problemas, por ejemplo, por qué no se puede eliminar un disco virtual debido a una configuración de solo lectura. También se explica por qué no se puede Agregar una unidad a un grupo (CannotPoolReason).

Los espacios de almacenamiento tienen tres objetos principales: *discos físicos* (discos duros, SSD, etc.) que se agregan a un *bloque de almacenamiento*, virtualizando el almacenamiento para que pueda crear *discos virtuales* a partir del espacio libre en el grupo, como se muestra aquí. Los metadatos del grupo se escriben en cada unidad del grupo. Los volúmenes se crean sobre los discos virtuales y almacenan los archivos, pero no vamos a hablar sobre los volúmenes aquí.

![Los discos físicos se agregan a un grupo de almacenamiento y, a continuación, a los discos virtuales creados a partir del espacio del grupo](media/storage-spaces-states/storage-spaces-object-model.png)

Puede ver Estados operativos y de estado en Administrador del servidor o con PowerShell. A continuación se muestra un ejemplo de una variedad de Estados operativos y de mantenimiento (principalmente incorrectos) en un clúster de Espacios de almacenamiento directo que no tiene la mayoría de los nodos de clúster (haga clic con el botón secundario en los encabezados de columna para agregar el **estado operativo**). No es un clúster feliz.

![Administrador del servidor que muestra los resultados de dos nodos que faltan en un clúster de Espacios de almacenamiento directo, muchos discos físicos que faltan y discos virtuales en un estado incorrecto](media/storage-spaces-states/unhealthy-disks-in-server-manager.png)

## <a name="storage-pool-states"></a>Estados del bloque de almacenamiento

Cada bloque de almacenamiento tiene un estado de mantenimiento: **correcto**, **advertencia**o **desconocido**/**incorrecto**, así como uno o varios Estados operativos.

Para averiguar en qué estado se encuentra un grupo, use los siguientes comandos de PowerShell:

```PowerShell
Get-StoragePool -IsPrimordial $False | Select-Object HealthStatus, OperationalStatus, ReadOnlyReason
```

Esta es una salida de ejemplo que muestra un grupo de almacenamiento en el estado de mantenimiento desconocido con el estado operativo de solo lectura:

```
FriendlyName                OperationalStatus HealthStatus IsPrimordial IsReadOnly
------------                ----------------- ------------ ------------ ----------
S2D on StorageSpacesDirect1 Read-only         Unknown      False        True
```

En las secciones siguientes se enumeran los estados de mantenimiento y operativo.

### <a name="pool-health-state-healthy"></a>Estado de mantenimiento del Grupo: correcto

| Estado operativo    | Descripción |
| ---------            | ---------  |
| Aceptar | El bloque de almacenamiento es correcto. |

### <a name="pool-health-state-warning"></a>Estado de mantenimiento del Grupo: ADVERTENCIA

Cuando el grupo de almacenamiento se encuentra en el estado de mantenimiento de **ADVERTENCIA** , significa que el grupo es accesible, pero una o más unidades de discos no se pudieron ejecutar o no. Como resultado, el grupo de almacenamiento puede tener una resistencia reducida.

|Estado operativo    |Descripción|
|---------            |---------  |
|Degradado|Faltan unidades en el bloque de almacenamiento. Esta condición solo se produce con unidades que hospedan metadatos del grupo. <br><br>**Acción**: Compruebe el estado de las unidades y reemplace las unidades con errores antes de que se produzcan errores adicionales.|

### <a name="pool-health-state-unknown-or-unhealthy"></a>Estado de mantenimiento del Grupo: desconocido o incorrecto

Cuando un grupo de almacenamiento se encuentra en un estado de mantenimiento **desconocido** **o incorrecto** , significa que el grupo de almacenamiento es de solo lectura y no se puede modificar hasta que el grupo se devuelve a los Estados de mantenimiento de **ADVERTENCIA** o **correcto** .

|Estado operativo    |Motivo de solo lectura |Descripción|
|---------            |---------       |--------   |
|Solo lectura|Incompleto|Esto puede ocurrir si el grupo de almacenamiento pierde su [cuórum](understand-quorum.md), lo que significa que la mayoría de las unidades del grupo tienen errores o están sin conexión por algún motivo. Cuando un grupo pierde su cuórum, los espacios de almacenamiento establecen automáticamente la configuración del grupo en solo lectura hasta que haya suficientes unidades disponibles de nuevo.<br><br>**Acción:** <br>1. Vuelva a conectar las unidades que faltan y, si está usando Espacios de almacenamiento directo, ponga todos los servidores en línea. <br>2. Vuelva a establecer el grupo en lectura-escritura; para ello, abra una sesión de PowerShell con permisos administrativos y escriba:<br><br> <code>Get-StoragePool <PoolName> -IsPrimordial $False \| Set-StoragePool -IsReadOnly $false</code>|
||Directiva|Un administrador establece el grupo de almacenamiento en solo lectura.<br><br>**Acción:** Para establecer un grupo de almacenamiento en clúster en acceso de lectura y escritura en Administrador de clústeres de conmutación por error, vaya a **grupos**, haga clic con el botón secundario en el grupo y seleccione **poner en línea**.<br><br>Para otros servidores y equipos, abra una sesión de PowerShell con permisos administrativos y, a continuación, escriba:<br><br><code>Get-StoragePool <PoolName> \| Set-StoragePool -IsReadOnly $false</code><br><br> |
||Starting|Espacios de almacenamiento está iniciando o esperando que las unidades estén conectadas en el grupo. Debe ser un estado temporal. Una vez iniciado por completo, el grupo debe realizar la transición a un estado operativo diferente.<br><br>**Acción:** Si el grupo permanece en el estado *iniciando* , asegúrese de que todas las unidades del grupo estén conectadas correctamente.|

Vea también [modificar un grupo de almacenamiento que tiene una configuración de solo lectura](https://social.technet.microsoft.com/wiki/contents/articles/14861.modifying-a-storage-pool-that-has-a-read-only-configuration.aspx).

## <a name="virtual-disk-states"></a>Estados de disco virtual

En los espacios de almacenamiento, los volúmenes se colocan en discos virtuales (espacios de almacenamiento) que se extraen del espacio libre en un grupo. Cada disco virtual tiene un estado de mantenimiento: **correcto**, **ADVERTENCIA**, **incorrecto**o **desconocido** , así como uno o varios Estados operativos.

Para averiguar en qué estado están los discos virtuales, use los siguientes comandos de PowerShell:

```PowerShell
Get-VirtualDisk | Select-Object FriendlyName,HealthStatus, OperationalStatus, DetachedReason
```

Este es un ejemplo de salida que muestra un disco virtual desconectado y un disco virtual degradado o incompleto:

```
FriendlyName HealthStatus OperationalStatus      DetachedReason
------------ ------------ -----------------      --------------
Volume1      Unknown      Detached               By Policy
Volume2      Warning      {Degraded, Incomplete} None
```

En las secciones siguientes se enumeran los estados de mantenimiento y operativo.

### <a name="virtual-disk-health-state-healthy"></a>Estado de mantenimiento del disco virtual: correcto

|Estado operativo    |Descripción|
|---------            |---------          |
|Aceptar    |El disco virtual está en buen estado.|
|Subóptimo    |Los datos no se escriben uniformemente en las unidades. <br><br>**Acción**: optimice el uso de la unidad en el bloque de almacenamiento mediante la ejecución del cmdlet [Optimize-StoragePool](https://technet.microsoft.com/itpro/powershell/windows/storage/optimize-storagepool) .|

### <a name="virtual-disk-health-state-warning"></a>Estado de mantenimiento del disco virtual: ADVERTENCIA

Cuando el disco virtual está en un estado de mantenimiento de **ADVERTENCIA** , significa que una o varias copias de los datos no están disponibles, pero los espacios de almacenamiento todavía pueden leer al menos una copia de los datos.

|Estado operativo    |Descripción|
|---------            |---------          |
|En servicio            |Windows está reparando el disco virtual, por ejemplo, después de agregar o quitar una unidad. Una vez completada la reparación, el disco virtual debe volver al estado de mantenimiento correcto.|
|Incompleto           |La resistencia del disco virtual se reduce debido a que una o más unidades produjeron errores o faltan. Sin embargo, las unidades que faltan contienen copias actualizadas de los datos.<br><br> **Acción**: <br>1. Vuelva a conectar las unidades que faltan, reemplace las unidades con errores y, si utiliza Espacios de almacenamiento directo, ponga en línea los servidores que estén sin conexión. <br>2. Si no está usando Espacios de almacenamiento directo, repare el disco virtual mediante el cmdlet [repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) .<br> Espacios de almacenamiento directo inicia automáticamente una reparación si es necesario después de volver a conectar o reemplazar una unidad.|
|Degradado             |La resistencia del disco virtual se reduce debido a que una o varias unidades no han superado o faltan, y hay copias no actualizadas de los datos en estas unidades. <br><br>**Acción**: <br> 1. Vuelva a conectar las unidades que faltan, reemplace las unidades con errores y, si utiliza Espacios de almacenamiento directo, ponga en línea los servidores que estén sin conexión. <br> 2. Si no está usando Espacios de almacenamiento directo, repare el disco virtual mediante el cmdlet [repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) . <br>Espacios de almacenamiento directo inicia automáticamente una reparación si es necesario después de volver a conectar o reemplazar una unidad.|

### <a name="virtual-disk-health-state-unhealthy"></a>Estado de mantenimiento del disco virtual: incorrecto

Cuando un disco virtual está en un estado de mantenimiento **incorrecto** , algunos o todos los datos del disco virtual no son accesibles actualmente.

|Estado operativo    |Descripción|
|---------            |--------   |
|Sin redundancia|El disco virtual ha perdido datos porque se han producido errores en demasiadas unidades.<br><br>**Acción**: Reemplace las unidades con errores y, a continuación, restaure los datos desde la copia de seguridad.|

### <a name="virtual-disk-health-state-informationunknown"></a>Estado de mantenimiento del disco virtual: información/desconocido

El disco virtual también puede estar en el estado de mantenimiento de la **información** (como se muestra en el elemento del panel de control de espacios de almacenamiento) o el estado de mantenimiento **desconocido** (como se muestra en PowerShell) si un administrador tomó el disco virtual sin conexión o se desconectó el disco virtual.

|Estado operativo    |Motivo desasociado |Descripción|
|---------            |---------       |--------   |
|Desasociado             |Por directiva            |Un administrador desconecta el disco virtual o establece el disco virtual para que requiera datos adjuntos manuales, en cuyo caso tendrá que conectar manualmente el disco virtual cada vez que se reinicie Windows.<br><br>**Acción**: vuelva a poner el disco virtual en línea. Para ello, cuando el disco virtual se encuentra en un grupo de almacenamiento en clúster, en Administrador de clústeres de conmutación por error seleccione **grupos** de > de **almacenamiento** > **discos virtuales**, seleccione el disco virtual que muestra el estado **sin conexión** y, a continuación, seleccione **poner en línea**. <br><br>Para volver a poner en línea un disco virtual cuando no se encuentra en un clúster, abra una sesión de PowerShell como administrador e intente usar el siguiente comando:<br><br> <code>Get-VirtualDisk \| Where-Object -Filter { $_.OperationalStatus -eq "Detached" } \| Connect-VirtualDisk</code><br><br>Para asociar automáticamente todos los discos virtuales no agrupados después de que se reinicie Windows, abra una sesión de PowerShell como administrador y, después, use el siguiente comando:<br><br> <code>Get-VirtualDisk \| Set-VirtualDisk -ismanualattach $false</code>|
|            |Discos mayoritario incorrectos |Demasiadas unidades usadas por este disco virtual con errores, faltan o tienen datos obsoletos.   <br><br>**Acción**: <br> 1. Vuelva a conectar las unidades que faltan y, si está usando Espacios de almacenamiento directo, ponga en línea los servidores que estén sin conexión. <br> 2. una vez que todas las unidades y servidores estén en línea, reemplace las unidades con errores. Consulte [servicio de mantenimiento](../../failover-clustering/health-service-overview.md) para obtener más información. <br>Espacios de almacenamiento directo inicia automáticamente una reparación si es necesario después de volver a conectar o reemplazar una unidad.<br>3. Si no está usando Espacios de almacenamiento directo, repare el disco virtual mediante el cmdlet [repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) .  <br><br>Si se han producido errores en más discos de los que tiene copias de los datos y el disco virtual no se ha reparado entre errores, todos los datos del disco virtual se perderán de forma permanente. En este caso desafortunado, elimine el disco virtual, cree un nuevo disco virtual y, a continuación, restaure a partir de una copia de seguridad.|
|                     |Incompleto |No hay suficientes unidades para leer el disco virtual.    <br><br>**Acción**: <br> 1. Vuelva a conectar las unidades que faltan y, si está usando Espacios de almacenamiento directo, ponga en línea los servidores que estén sin conexión. <br> 2. una vez que todas las unidades y servidores estén en línea, reemplace las unidades con errores. Consulte [servicio de mantenimiento](../../failover-clustering/health-service-overview.md) para obtener más información. <br>Espacios de almacenamiento directo inicia automáticamente una reparación si es necesario después de volver a conectar o reemplazar una unidad.<br>3. Si no está usando Espacios de almacenamiento directo, repare el disco virtual mediante el cmdlet [repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) .<br><br>Si se han producido errores en más discos de los que tiene copias de los datos y el disco virtual no se ha reparado entre errores, todos los datos del disco virtual se perderán de forma permanente. En este caso desafortunado, elimine el disco virtual, cree un nuevo disco virtual y, a continuación, restaure a partir de una copia de seguridad.|
| |Tiempo de espera agotado|La conexión del disco virtual tardó demasiado <br><br> **Acción:** Esto no ocurre con frecuencia, por lo que puede probar si la condición pasa a tiempo. También puede intentar desconectar el disco virtual con el cmdlet [Disconnect-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/disconnect-virtualdisk?view=win10-ps) y, a continuación, usar el cmdlet [Connect-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/connect-virtualdisk?view=win10-ps) para volver a conectarlo. |

## <a name="drive-physical-disk-states"></a>Estados de unidad (disco físico)

En las secciones siguientes se describen los estados de mantenimiento que puede presentar una unidad. Las unidades de un grupo se representan en PowerShell como objetos de *disco físico* .

### <a name="drive-health-state-healthy"></a>Estado de mantenimiento de la unidad: correcto

|Estado operativo    |Descripción|
|---------            |---------          |
|Aceptar|La unidad es correcta.|
|En servicio|La unidad está realizando algunas operaciones de mantenimiento interno. Una vez completada la acción, la unidad debe volver al estado de mantenimiento *correcto* .|

### <a name="drive-health-state-warning"></a>Estado de mantenimiento de la unidad: ADVERTENCIA

Una unidad en estado de advertencia puede leer y escribir datos correctamente, pero tiene un problema.

|Estado operativo    |Descripción|
|---------            |---------          |
|Comunicación perdida|Falta la unidad. Si está utilizando Espacios de almacenamiento directo, esto podría deberse a que un servidor está inactivo.<br><br>**Acción**: Si está usando espacios de almacenamiento directo, vuelva a poner en línea todos los servidores. Si no se soluciona, vuelva a conectar la unidad, reemplácela o intente obtener información de diagnóstico detallada acerca de esta unidad siguiendo los pasos de solución de problemas con Informe de errores de Windows > [disco físico agotó el tiempo de espera](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-timed-out).|
|Quitando del grupo|Los espacios de almacenamiento están en proceso de quitar la unidad de su bloque de almacenamiento. <br><br> Se trata de un estado temporal. Una vez completada la eliminación, si la unidad sigue conectada al sistema, la unidad pasará a otro estado operativo (normalmente correcto) en un grupo primordial.|
|Iniciando el modo de mantenimiento|Los espacios de almacenamiento están en el proceso de poner la unidad en modo de mantenimiento después de que un administrador ponga la unidad en modo de mantenimiento. Se trata de un estado temporal: la unidad debe estar pronto en el estado *en modo de mantenimiento* .|
|En modo de mantenimiento|Un administrador colocó la unidad en modo de mantenimiento y detiene las lecturas y escrituras de la unidad. Esto se suele hacer antes de actualizar el firmware de la unidad o cuando se producen errores en las pruebas.<br><br>**Acción**: para sacar la unidad del modo de mantenimiento, use el cmdlet [Disable-StorageMaintenanceMode](https://technet.microsoft.com/itpro/powershell/windows/storage/disable-storagemaintenancemode) .|
|Deteniendo el modo de mantenimiento|Un administrador deshizo la unidad del modo de mantenimiento y los espacios de almacenamiento se encontraban en el proceso de volver a poner en línea la unidad. Se trata de un estado temporal: la unidad debe estar pronto en otro Estado: idealmente *correcto*.|
|Error predictivo|La unidad ha detectado que está cerca de un error.<br><br>**Acción**: Reemplace la unidad.|
|Error de E/S|Se produjo un error temporal al acceder a la unidad.<br><br>**Acción**: <br>1. Si la unidad no vuelve a pasar al estado **correcto** , puede intentar usar el cmdlet [RESET-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) para borrar la unidad. <br> 2. use [repair-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk) para restaurar la resistencia de los discos virtuales afectados. <br>3. Si esto sigue ocurriendo, reemplace la unidad.|
|Error transitorio|Se produjo un error temporal con la unidad. Normalmente, esto significa que la unidad no responde, pero también podría significar que la partición de protección de espacios de almacenamiento se ha quitado de la unidad de forma inapropiada. <br><br>**Acción**: <br>1. Si la unidad no vuelve a pasar al estado **correcto** , puede intentar usar el cmdlet [RESET-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) para borrar la unidad. <br> 2. use [repair-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk) para restaurar la resistencia de los discos virtuales afectados. <br>3. Si esto sigue ocurriendo, reemplace la unidad o intente obtener información detallada de diagnóstico acerca de esta unidad siguiendo los pasos de solución de problemas con Informe de errores de Windows > [disco físico no se pudo conectar](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online).|
|Latencia anómala|La unidad funciona lentamente, medida por el Servicio de mantenimiento en Espacios de almacenamiento directo.<br><br>**Acción**: Si esto sigue ocurriendo, reemplace la unidad para que no se reduzca el rendimiento de los espacios de almacenamiento en su conjunto.

### <a name="drive-health-state-unhealthy"></a>Estado de mantenimiento de la unidad: incorrecto

No se puede escribir en una unidad en el estado incorrecto, ni tampoco acceder a ella.

|Estado operativo    |Descripción|
|---------            |---------          |
|No se puede usar|Esta unidad no se puede usar en espacios de almacenamiento. Para obtener más información, vea [espacios de almacenamiento directo requisitos de hardware](storage-spaces-direct-hardware-requirements.md); Si no está usando Espacios de almacenamiento directo, consulte [Introducción a los espacios de almacenamiento](https://technet.microsoft.com/library/hh831739(v=ws.11).aspx#Requirements).|
|división|La unidad se ha separado del grupo.<br><br>**Acción**: restablezca la unidad y borre todos los datos de la unidad y vuelva a agregarlos al grupo como una unidad vacía. Para ello, abra una sesión de PowerShell como administrador, ejecute el cmdlet [RESET-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) y, a continuación, ejecute [repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk). <br><br>Para obtener información detallada de diagnóstico acerca de esta unidad, siga los pasos de solución de problemas con Informe de errores de Windows > [disco físico no se pudo conectar](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online).|
|Metadatos obsoletos|Espacios de almacenamiento encontró metadatos antiguos en la unidad.<br><br>**Acción**: debe ser un estado temporal. Si la unidad no vuelve a la transición a aceptar, puede ejecutar [repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) para iniciar una operación de reparación en los discos virtuales afectados. Si esto no resuelve el problema, puede restablecer la unidad con el cmdlet [RESET-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) , borrar todos los datos de la unidad y, a continuación, ejecutar [repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk).|
|Metadatos no reconocidos|Espacios de almacenamiento encontró metadatos no reconocidos en la unidad, lo que normalmente significa que la unidad tiene metadatos de un grupo diferente.<br><br>**Acción**: para borrar la unidad y agregarla al grupo actual, restablezca la unidad. Para restablecer la unidad, abra una sesión de PowerShell como administrador, ejecute el cmdlet [RESET-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) y, a continuación, ejecute [repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk).|
|Error en los medios|Se produjo un error en la unidad y Espacios de almacenamiento ya no la podrá usar.<br><br>**Acción**: Reemplace la unidad. <br><br>Para obtener información detallada de diagnóstico acerca de esta unidad, siga los pasos de solución de problemas con Informe de errores de Windows > [disco físico no se pudo conectar](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online).|
|Error de hardware del dispositivo|Se produjo un error de hardware en esta unidad. <br><br>**Acción**: Reemplace la unidad.|
|La actualización del firmware|Windows está actualizando el firmware de la unidad. Se trata de un estado temporal que suele durar menos de un minuto y durante el cual otras unidades del grupo controlan todas la lecturas y escrituras. Para obtener más información, consulte [actualización del firmware](../update-firmware.md)de la unidad.|
|Starting|La unidad se está preparando para la operación. Debería ser un estado temporal: una vez finalizado, la unidad debería cambiar a un estado operativo diferente.|

## <a name="reasons-a-drive-cant-be-pooled"></a>Motivos por los que no se puede agrupar una unidad

Algunas unidades no están preparadas para estar en un grupo de almacenamiento. Puede averiguar por qué una unidad no es válida para la agrupación examinando la propiedad `CannotPoolReason` de un disco físico. Este es un script de PowerShell de ejemplo para mostrar la propiedad CannotPoolReason:

```PowerShell
Get-PhysicalDisk | Format-Table FriendlyName,MediaType,Size,CanPool,CannotPoolReason
```

A continuación se muestra un ejemplo de salida:

```
FriendlyName          MediaType          Size CanPool CannotPoolReason
------------          ---------          ---- ------- ----------------
ATA MZ7LM120HCFD00D3  SSD        120034123776   False Insufficient Capacity
Msft Virtual Disk     SSD         10737418240    True
Generic Physical Disk SSD        119990648832   False In a Pool
```

En la tabla siguiente se ofrece un poco más de información sobre cada uno de los motivos.

|Razón|Descripción|
|---|---|
|En un grupo|La unidad ya pertenece a un grupo de almacenamiento. <br><br>Las unidades pueden pertenecer a un solo grupo de almacenamiento a la vez. Para usar esta unidad en otro grupo de almacenamiento, quite primero la unidad de su grupo existente, que indica a los espacios de almacenamiento que deben trasladar los datos de la unidad a otras unidades del grupo. O bien, restablezca la unidad si la unidad se ha desconectado de su grupo sin notificar a los espacios de almacenamiento. <br><br>Para quitar de forma segura una unidad de un bloque de almacenamiento, use [Remove-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/remove-physicaldisk)o vaya a administrador del servidor > **servicios de archivos y almacenamiento** > **grupos de almacenamiento**, > **discos físicos**, haga clic con el botón secundario en la unidad y seleccione **quitar disco**.<br><br>Para restablecer una unidad, utilice [RESET-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk).|
|Incorrecto|La unidad no está en un estado correcto y es posible que deba reemplazarse.|
|Medio extraíble|La unidad está clasificada como una unidad extraíble. <br><br>Los espacios de almacenamiento no admiten los medios que Windows reconoce como extraíbles, como las unidades Blu-Ray. Aunque muchas unidades fijas están en ranuras extraíbles, en general, los medios que Windows *clasifica* como extraíbles no son adecuados para su uso con espacios de almacenamiento.|
|Usado por el clúster|Un clúster de conmutación por error está usando la unidad actualmente.|
|Desconectado|La unidad está sin conexión. <br><br>Para poner en línea todas las unidades sin conexión y establecer en lectura/escritura, abra una sesión de PowerShell como administrador y use los siguientes scripts:<br><br><code>Get-Disk \| Where-Object -Property OperationalStatus -EQ "Offline" \| Set-Disk -IsOffline $false</code><br><br><code>Get-Disk \| Where-Object -Property IsReadOnly -EQ $true \| Set-Disk -IsReadOnly $false</code>|
|Capacidad insuficiente|Esto suele ocurrir cuando hay particiones que ocupan el espacio libre en la unidad. <br><br>**Acción**: Elimine todos los volúmenes de la unidad y borre todos los datos de la unidad. Una manera de hacerlo es usar el cmdlet [de PowerShell Clear-Disk](https://docs.microsoft.com/powershell/module/storage/clear-disk?view=win10-ps) .|
|Verificación en curso|En el [servicio de mantenimiento](../../failover-clustering/health-service-overview.md#supported-components-document) se comprueba si la unidad o el firmware de la unidad están aprobados para su uso por parte del administrador del servidor.|
|Error de verificación|El [servicio de mantenimiento](../../failover-clustering/health-service-overview.md#supported-components-document) no pudo comprobar si la unidad o el firmware de la unidad están aprobados para su uso por parte del administrador del servidor.|
|Firmware no compatible|El firmware de la unidad física no está en la lista de revisiones de firmware aprobadas especificadas por el administrador del servidor mediante el [servicio de mantenimiento](../../failover-clustering/health-service-overview.md#supported-components-document). |
|Hardware no compatible|La unidad no está en la lista de modelos de almacenamiento aprobados especificados por el administrador del servidor mediante el [servicio de mantenimiento](../../failover-clustering/health-service-overview.md#supported-components-document).|

## <a name="see-also"></a>Consulta también

- [Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
- [Requisitos de hardware Espacios de almacenamiento directo](storage-spaces-direct-hardware-requirements.md)
- [Descripción del cuórum de clúster y grupo](understand-quorum.md)