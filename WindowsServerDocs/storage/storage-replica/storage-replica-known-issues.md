---
title: Problemas conocidos de Réplica de almacenamiento
ms.prod: windows-server
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 06/25/2019
ms.assetid: ceddb0fa-e800-42b6-b4c6-c06eb1d4bc55
ms.openlocfilehash: 1ab4c0946c1081019747420448a0217359282bf1
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85469730"
---
# <a name="known-issues-with-storage-replica"></a>Problemas conocidos de Réplica de almacenamiento

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

En este tema se describen los problemas conocidos de réplica de almacenamiento en Windows Server.

## <a name="after-removing-replication-disks-are-offline-and-you-cannot-configure-replication-again"></a>Después de quitar la replicación, los discos están sin conexión y no se puede configurar la replicación de nuevo.

En Windows Server 2016, es posible que no sea capaz de aprovisionar la replicación en un volumen que se replicó previamente o que encuentre volúmenes que no se pueden montar. Esto puede ocurrir cuando alguna condición de error evita la eliminación de replicación o cuando vuelve a instalar el sistema operativo en un equipo que anteriormente estaba replicando datos.

Para solucionar el problema, debe borrar la partición oculta de Réplica de almacenamiento de los discos y devolver a estos a un estado de escritura con el cmdlet `Clear-SRMetadata`.

-   Para quitar todas las ranuras huérfanas de bases de datos de partición de Réplica de almacenamiento y volver a montar todas las particiones, use el parámetro `-AllPartitions` como sigue:

    ```PowerShell
    Clear-SRMetadata -AllPartitions
    ```

-   Para quitar todos los datos de registro huérfanos de Réplica de almacenamiento, utilice el parámetro `-AllLogs` como sigue:

    ```PowerShell
    Clear-SRMetadata -AllLogs
    ```

-   Para quitar todos los datos de configuración huérfanos del clúster de conmutación por error, utilice el parámetro `-AllConfiguration` como sigue:

    ```PowerShell
    Clear-SRMetadata -AllConfiguration
    ```

-   Para quitar los metadatos individuales del grupo de replicación, utilice el parámetro `-Name` y especifique un grupo de replicación como sigue:

    ```PowerShell
    Clear-SRMetadata -Name RG01 -Logs -Partition
    ```

Es posible que el servidor tenga que reiniciarse después de limpiar la base de datos de la partición; puede obviar esto temporalmente con `-NoRestart`, pero no debe omitir el reinicio del servidor si lo solicita el cmdlet. Este cmdlet no elimina los volúmenes de datos ni los datos contenidos dentro de esos volúmenes.

## <a name="during-initial-sync-see-event-log-4004-warnings"></a>Durante la sincronización inicial, consulte las advertencias 4004 del registro de eventos.

En Windows Server 2016, al configurar la replicación, es posible que tanto el servidor de origen como el de destino muestren cada uno varias advertencias 4004 del registro de eventos **StorageReplica\Admin** durante la sincronización inicial, con un estado de "No hay recursos de sistema suficientes para completar la llamada a la API". Es probable que vea también errores 5014. Estos indican que los servidores no tienen suficiente memoria disponible (RAM) para realizar la sincronización inicial y ejecutar cargas de trabajo. Agregue RAM o reduzca la cantidad de RAM usada en características y aplicaciones que no sean de Réplica de almacenamiento.

## <a name="when-using-guest-clusters-with-shared-vhdx-and-a-host-without-a-csv-virtual-machines-stop-responding-after-configuring-replication"></a>Al usar clústeres invitados con VHDX compartidos y un host sin CSV, las máquinas virtuales dejan de responder tras configurar la replicación.

En Windows Server 2016 , al usar clústeres invitados de Hyper-V para fines de prueba o demostración de Réplica de almacenamiento, y con VHDX compartido como almacenamiento de clúster invitado, las máquinas virtuales dejan de responder después de configurar la replicación. Si reinicia el host de Hyper-V, las máquinas virtuales empiezan a responder, pero la configuración de replicación no se completará y no se producirá ninguna replicación.

Este comportamiento se produce cuando se usa **fltmc.exe attach svhdxflt** para omitir el requisito para el host de Hyper-V que ejecuta un CSV. El uso de este comando no se admite y está destinado únicamente a fines de prueba y demostración.

La causa de la ralentización es un problema de interoperabilidad de diseño entre la característica Calidad de servicio de almacenamiento de Windows Server 2016 y el filtro de VHDX compartido adjunto manualmente. Para resolver este problema, deshabilite el controlador de filtro de calidad de servicio de almacenamiento y reinicie el host de Hyper-V:

```
SC config storqosflt start= disabled
```

## <a name="cannot-configure-replication-when-using-new-volume-and-differing-storage"></a>No se puede configurar la replicación al usar New-Volume y un almacenamiento diferente.

Cuando se usa el cmdlet `New-Volume` con diferentes conjuntos de almacenamiento en el servidor de origen y de destino, como dos SAN diferentes o dos JBOD con discos diferentes, es posible que no pueda configurar posteriormente la replicación mediante `New-SRPartnership`. El error que aparece puede incluir:

    Data partition sizes are different in those two groups

Use el cmdlet `New-Partition**` para crear volúmenes y darles formato en lugar de `New-Volume`, ya que este último puede redondear el tamaño del volumen en diferentes matrices de almacenamiento. Si ya ha creado un volumen NTFS, puede usar `Resize-Partition` para aumentar o reducir uno de los volúmenes para que coincida con el otro (esto no es posible con volúmenes ReFS). Si usa **Diskmgmt** o el **Administrador del servidor**, no se producirá ningún redondeo.

## <a name="running-test-srtopology-fails-with-name-related-errors"></a>Error al ejecutarTest-SRTopology con errores relacionados con el nombre

Al intentar utilizar `Test-SRTopology`, recibirá uno de los siguientes errores:

**EJEMPLO DE ERROR 1:**

    WARNING: Invalid value entered for target computer name: sr-srv03. Test-SrTopology cmdlet does not accept IP address as
    input for target computer name parameter. NetBIOS names and fully qualified domain names are acceptable inputs
    WARNING: System.Exception
    WARNING:    at Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand.BeginProcessing()
    Test-SRTopology : Invalid value entered for target computer name: sr-srv03. Test-SrTopology cmdlet does not accept IP
    address as input for target computer name parameter. NetBIOS names and fully qualified domain names are acceptable
    inputs
    At line:1 char:1
    + Test-SRTopology -SourceComputerName sr-srv01 -SourceVolumeName d: -So ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : InvalidArgument: (:) [Test-SRTopology], Exception
        + FullyQualifiedErrorId : TestSRTopologyFailure,Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand

**EJEMPLO DE ERROR 2:**

    WARNING: Invalid value entered for source computer name

**EJEMPLO DE ERROR 3:**

    The specified volume cannot be found G: cannot be found on computer SRCLUSTERNODE1

Este cmdlet tiene limitado el informe de errores en Windows Server 2016 y devolverá el mismo resultado para muchos problemas comunes. El error puede aparecer por las razones siguientes:

* Ha iniciado sesión en el equipo de origen como usuario local, no como usuario de dominio.
* El equipo de destino no se está ejecutando o no es accesible a través de la red.
* Ha especificado un nombre incorrecto para el equipo de destino.
* Ha especificado una dirección IP para el servidor de destino.
* El firewall del equipo de destino está bloqueando el acceso a PowerShell o las llamadas de CIM.
* El equipo de destino no está ejecutando el servicio WMI.
* No utilizó CREDSSP al ejecutar el cmdlet `Test-SRTopology` remotamente desde un equipo de administración.
* El volumen de origen o de destino especificado es un disco local en un nodo de clúster, no discos agrupados.

## <a name="configuring-new-storage-replica-partnership-returns-an-error---failed-to-provision-partition"></a>La configuración de nueva asociación de Réplica de almacenamiento devuelve un error que especifica que no se pudo aprovisionar la partición.

Al intentar crear una nueva asociación de replicación con `New-SRPartnership`, recibe el error siguiente:

    New-SRPartnership : Unable to create replication group test01, detailed reason: Failed to provision partition ed0dc93f-107c-4ab4-a785-afd687d3e734.
    At line: 1 char: 1
    + New-SRPartnership -SourceComputerName srv1 -SourceRGName test01
    + Categorylnfo : ObjectNotFound: (MSFT_WvrAdminTasks : root/ Microsoft/. . s) CNew-SRPartnership], CimException
    + FullyQua1ifiedErrorId : Windows System Error 1168 ,New-SRPartnership

Esto se produce al seleccionar un volumen de datos que se encuentra en la misma partición que la unidad del sistema (es decir, la unidad **C:** con su carpeta de Windows). Por ejemplo, en una unidad que contiene los volúmenes **C:** y **D:** creados desde la misma partición. Esto no se admite en Réplica de almacenamiento; debe elegir un volumen diferente para replicar.

## <a name="attempting-to-grow-a-replicated-volume-fails-due-to-missing-update"></a>Se produce un error al intentar aumentar un volumen replicado debido a que falta una actualización

Cuando intenta aumentar o extender un volumen replicado, recibe el error siguiente:

    PS C:\> Resize-Partition -DriveLetter d -Size 44GB
    Resize-Partition : The operation failed with return code 8
    At line:1 char:1
    + Resize-Partition -DriveLetter d -Size 44GB
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (StorageWMI:ROOT/Microsoft/.../MSFT_Partition
    [Resize-Partition], CimException
    + FullyQualifiedErrorId : StorageWMI 8,Resize-Partition

Si utiliza el complemento MMC de administración de discos, recibe este error:

    Element not found

Esto sucede incluso si habilita correctamente el cambio de tamaño del volumen en el servidor de origen mediante `Set-SRGroup -Name rg01 -AllowVolumeResize $TRUE` .

Este problema se corrigió en la actualización acumulativa para Windows 10, versión 1607 (actualización de aniversario) y Windows Server 2016:9 de diciembre de 2016 (KB3201845).

## <a name="attempting-to-grow-a-replicated-volume-fails-due-to-missing-step"></a>Se produce un error al intentar aumentar un volumen replicado debido a que falta un paso

Si intenta cambiar el tamaño de un volumen replicado en el servidor de origen sin establecer `-AllowResizeVolume $TRUE` primero, recibirá los siguientes errores:

    PS C:\> Resize-Partition -DriveLetter I -Size 8GB
    Resize-Partition : Failed

    Activity ID: {87aebbd6-4f47-4621-8aa4-5328dfa6c3be}
    At line:1 char:1
    + Resize-Partition -DriveLetter I -Size 8GB
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (StorageWMI:ROOT/Microsoft/.../MSFT_Partition) [Resize-Partition], CimException
        + FullyQualifiedErrorId : StorageWMI 4,Resize-Partition

    Storage Replica Event log error 10307:

    Attempted to resize a partition that is protected by Storage Replica .

    DeviceName: \Device\Harddisk1\DR1
    PartitionNumber: 7
    PartitionId: {b71a79ca-0efe-4f9a-a2b9-3ed4084a1822}

    Guidance: To grow a source data partition, set the policy on the replication group containing the data partition.

    Set-SRGroup -ComputerName [ComputerName] -Name [ReplicationGroupName] -AllowVolumeResize $true

    Before you grow the source data partition, ensure that the destination data partition has enough space to grow to an equal size. Shrinking of data partition protected by Storage Replica is blocked.

Error en el complemento Administración de discos:

    An unexpected error has occurred

Después de cambiar el tamaño del volumen, no olvide deshabilitar el cambio de tamaño con `Set-SRGroup -Name rg01 -AllowVolumeResize $FALSE` . Este parámetro impide que los administradores intenten cambiar el tamaño de los volúmenes antes de asegurarse de que hay suficiente espacio en el volumen de destino, normalmente porque no eran conscientes de la presencia de la réplica de almacenamiento.

## <a name="attempting-to-move-a-pdr-resource-between-sites-on-an-asynchronous-stretch-cluster-fails"></a>Si intenta mover un recurso PDR entre sitios en un clúster extendido asincrónico, se producirá un error

Al intentar mover un rol adjunto a un recurso de disco físico, como un servidor de archivos para uso general, para mover el almacenamiento asociado en un clúster extendido asincrónico, recibe un error.

Si usa el complemento Administrador de clústeres de conmutación por error:

    Error
    The operation has failed.
    The action 'Move' did not complete.
    Error Code: 0x80071398
    The operation failed because either the specified cluster node is not the owner of the group, or the node is not a possible owner of the group

Si utiliza el cmdlet de PowerShell de clúster:

    PS C:\> Move-ClusterGroup -Name sr-fs-006 -Node sr-srv07
    Move-ClusterGroup : An error occurred while moving the clustered role 'sr-fs-006'.
    The operation failed because either the specified cluster node is not the owner of the group, or the node is not a
    possible owner of the group
    At line:1 char:1
    + Move-ClusterGroup -Name sr-fs-006 -Node sr-srv07
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [Move-ClusterGroup], ClusterCmdletException
    + FullyQualifiedErrorId : Move-ClusterGroup,Microsoft.FailoverClusters.PowerShell.MoveClusterGroupCommand

Esto se produce debido a un comportamiento predeterminado de Windows Server 2016. Usa `Set-SRPartnership` para mover estos discos de PDR a un clúster extendido asincrónico.

Este comportamiento se ha cambiado en Windows Server, versión 1709 para permitir conmutaciones por error manuales y automatizadas con la replicación asincrónica, en función de los comentarios de los clientes.

## <a name="attempting-to-add-disks-to-a-two-node-asymmetric-cluster-returns-no-disks-suitable-for-cluster-disks-found"></a>Al intentar agregar discos a un clúster asimétrico de dos nodos, se devuelve "No hay discos adecuados para los discos de clúster encontrados"

Al intentar aprovisionar un clúster con solo dos nodos, antes de añadir la replicación elástica de réplica de almacenamiento, intente agregar los discos en el segundo sitio a los discos disponibles. Recibe el siguiente error:

    "No disks suitable for cluster disks found. For diagnostic information about disks available to the cluster, use the Validate a Configuration Wizard to run Storage tests."

Esto no ocurre si tiene al menos tres nodos en el clúster. Este problema se produce debido a un cambio de código predeterminado en Windows Server 2016 para comportamientos de clústeres de almacenamiento asimétrico.

Para agregar el almacenamiento, puede ejecutar el siguiente comando en el nodo en el segundo sitio:

`Get-ClusterAvailableDisk -All | Add-ClusterDisk`

Esto no funcionará con el almacenamiento local de nodo. Puede usar réplica de almacenamiento para replicar un clúster extendido entre dos nodos en total, **cada uno con su propio conjunto de almacenamiento compartido.**

## <a name="the-smb-bandwidth-limiter-fails-to-throttle-storage-replica-bandwidth"></a>El límite de ancho de banda SMB no puede limitar el ancho de banda de réplica de almacenamiento

Al especificar un límite de ancho de banda para la réplica de almacenamiento, se omite el límite y se usa el ancho de banda completo. Por ejemplo:

`Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond 32MB`

Este problema se produce debido a un problema de interoperabilidad entre la réplica de almacenamiento y SMB. Este problema se corrigió por primera vez en la actualización acumulativa de julio de 2017 de Windows Server 2016 y en Windows Server, versión 1709.

## <a name="event-1241-warning-repeated-during-initial-sync"></a>ADVERTENCIA del evento 1241 repetida durante la sincronización inicial

Cuando se especifica una asociación de replicación es asincrónica, el equipo de origen registra repetidamente el evento de advertencia 1241 en el canal de administración de réplica de almacenamiento. Por ejemplo:

    Log Name:      Microsoft-Windows-StorageReplica/Admin
    Source:        Microsoft-Windows-StorageReplica
    Date:          3/21/2017 3:10:41 PM
    Event ID:      1241
    Task Category: (1)
    Level:         Warning
    Keywords:      (1)
    User:          SYSTEM
    Computer:      sr-srv05.corp.contoso.com
    Description:
    The Recovery Point Objective (RPO) of the asynchronous destination is unavailable.

    LocalReplicationGroupName: rg01
    LocalReplicationGroupId: {e20b6c68-1758-4ce4-bd3b-84a5b5ef2a87}
    LocalReplicaName: f:\
    LocalPartitionId: {27484a49-0f62-4515-8588-3755a292657f}
    ReplicaSetId: {1f6446b5-d5fd-4776-b29b-f235d97d8c63}
    RemoteReplicationGroupName: rg02
    RemoteReplicationGroupId: {7f18e5ea-53ca-4b50-989c-9ac6afb3cc81}
    TargetRPO: 30

    Guidance: This is typically due to one of the following reasons:

El destino asincrónico está actualmente desconectado. El RPO puede estar disponible después de que se restaure la conexión.

    The asynchronous destination is unable to keep pace with the source such that the most recent destination log record is no longer present in the source log. The destination will start block copying. The RPO should become available after block copying completes.

Este es el comportamiento esperado durante la sincronización inicial y se puede omitir de forma segura. Este comportamiento puede cambiar en una versión posterior. Si ve este comportamiento durante la replicación asincrónica continua, investigue la Asociación para determinar por qué la replicación se retrasa más allá del RPO configurado (de forma predeterminada, 30 segundos).

## <a name="event-4004-warning-repeated-after-rebooting-a-replicated-node"></a>ADVERTENCIA del evento 4004 repetida después de reiniciar un nodo replicado

En circunstancias poco frecuentes y normalmente unreproducable, el reinicio de un servidor que se encuentra en una asociación conduce a un error de replicación y el evento de advertencia de registro de nodo reiniciado 4004 con un error de acceso denegado.

    Log Name:      Microsoft-Windows-StorageReplica/Admin
    Source:        Microsoft-Windows-StorageReplica
    Date:          3/21/2017 11:43:25 AM
    Event ID:      4004
    Task Category: (7)
    Level:         Warning
    Keywords:      (256)
    User:          SYSTEM
    Computer:      server.contoso.com
    Description:
    Failed to establish a connection to a remote computer.

    RemoteComputerName: server
    LocalReplicationGroupName: rg01
    LocalReplicationGroupId: {a386f747-bcae-40ac-9f4b-1942eb4498a0}
    RemoteReplicationGroupName: rg02
    RemoteReplicationGroupId: {a386f747-bcae-40ac-9f4b-1942eb4498a0}
    ReplicaSetId: {00000000-0000-0000-0000-000000000000}
    RemoteShareName:{a386f747-bcae-40ac-9f4b-1942eb4498a0}.{00000000-0000-0000-0000-000000000000}
    Status: {Access Denied}
    A process has requested access to an object, but has not been granted those access rights.

    Guidance: Possible causes include network failures, share creation failures for the remote replication group, or firewall settings. Make sure SMB traffic is allowed and there are no connectivity issues between the local computer and the remote computer. You should expect this event when suspending replication or removing a replication partnership.

Tenga en cuenta el `Status: "{Access Denied}"` y el mensaje `A process has requested access to an object, but has not been granted those access rights.` este es un problema conocido dentro de la réplica de almacenamiento y se corrigió en la actualización de la calidad 12 de septiembre de 2017: KB4038782 (compilación del sistema operativo 14393,1715)https://support.microsoft.com/help/4038782/windows-10-update-kb4038782

## <a name="error-failed-to-bring-the-resource-cluster-disk-x-online-with-a-stretch-cluster"></a>Error "no se pudo conectar el recurso ' disco de clúster x ' en línea." con un clúster extendido

Al intentar poner un disco de clúster en línea después de una conmutación por error correcta, donde está intentando volver a crear el sitio de origen original, recibirá un error en Administrador de clústeres de conmutación por error. Por ejemplo:

    Error
    The operation has failed.
    Failed to bring the resource 'Cluster Disk 2' online.

    Error Code: 0x80071397
    The operation failed because either the specified cluster node is not the owner of the resource, or the node is not a possible owner of the resource.

Si intenta trasladar el disco o CSV manualmente, recibirá un error adicional. Por ejemplo:

    Error
    The operation has failed.
    The action 'Move' did not complete.

    Error Code: 0x8007138d
    A cluster node is not available for this operation

Este problema se debe a que uno o varios discos no inicializados están conectados a uno o varios nodos del clúster. Para resolver el problema, inicialice todos los almacenamientos asociados mediante DiskMgmt. msc, DISKPART.EXE o el cmdlet de PowerShell Initialize-Disk.

Estamos trabajando para proporcionar una actualización que resuelva permanentemente este problema. Si está interesado en ayudarnos y tiene un contrato de Microsoft soporte técnico Premier, envíe un mensaje de correo electrónico SRFEED@microsoft.com para que podamos trabajar con usted en el envío de una solicitud de reenvío.

## <a name="gpt-error-when-attempting-to-create-a-new-sr-partnership"></a>Error de GPT al intentar crear una nueva asociación de SR

Al ejecutar New-SRPartnership, se produce el error:

    Disk layout type for volume \\?\Volume{GUID}\ is not a valid GPT style layout.
    New-SRPartnership : Unable to create replication group SRG01, detailed reason: Disk layout type for volume
    \\?\Volume{GUID}\ is not a valid GPT style layout.
    At line:1 char:1
    + New-SRPartnership -SourceComputerName nodesrc01 -SourceRGName SRG01 ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (MSFT_WvrAdminTasks:root/Microsoft/...T_WvrAdminTasks) [New-SRPartnership]
    , CimException
    + FullyQualifiedErrorId : Windows System Error 5078,New-SRPartnership

En la interfaz gráfica de usuario de Administrador de clústeres de conmutación por error, no hay ninguna opción para configurar la replicación para el disco.

Al ejecutar test-SRTopology, se produce un error con:

    WARNING: Object reference not set to an instance of an object.
    WARNING: System.NullReferenceException
    WARNING:    at Microsoft.FileServices.SR.Powershell.MSFTPartition.GetPartitionInStorageNodeByAccessPath(String AccessPath, String ComputerName, MIObject StorageNode)
       at Microsoft.FileServices.SR.Powershell.Volume.GetVolume(String Path, String ComputerName)
       at Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand.BeginProcessing()
    Test-SRTopology : Object reference not set to an instance of an object.
    At line:1 char:1
    + Test-SRTopology -SourceComputerName nodesrc01 -SourceVolumeName U: - ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidArgument: (:) [Test-SRTopology], NullReferenceException
    + FullyQualifiedErrorId : TestSRTopologyFailure,Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand

Esto se debe a que el nivel funcional del clúster todavía se está estableciendo en Windows Server 2012 R2 (es decir, FL 8). Se supone que la réplica de almacenamiento debe devolver un error específico aquí, pero en su lugar devuelve una asignación de errores incorrecta.

Ejecute Get-Cluster | FL * en cada nodo.

Si ClusterFunctionalLevel = 9, es la versión de ClusterFunctionalLevel de Windows 2016 necesaria para implementar la réplica de almacenamiento en este nodo.
Si ClusterFunctionalLevel no es 9, el ClusterFunctionalLevel deberá actualizarse para poder implementar la réplica de almacenamiento en este nodo.

Para resolver el problema, eleve el nivel funcional del clúster mediante la ejecución del cmdlet de PowerShell: [Update-ClusterFunctionalLevel](https://docs.microsoft.com/powershell/module/failoverclusters/update-clusterfunctionallevel)

## <a name="small-unknown-partition-listed-in-diskmgmt-for-each-replicated-volume"></a>Pequeña partición desconocida enumerada en DISKMGMT para cada volumen replicado

Al ejecutar el complemento Administración de discos (DISKMGMT. MSC), observará que uno o varios volúmenes aparecen sin etiqueta o letra de unidad y 1 MB de tamaño. Es posible que pueda eliminar el volumen desconocido o que reciba:

    "An Unexpected Error has Occurred"

Este comportamiento es así por diseño. No es un volumen, sino una partición. Réplica de almacenamiento crea una partición de 512 KB como una ranura de base de datos para las operaciones de replicación (la herramienta DiskMgmt. msc heredada se redondea al MB más cercano). Tener una partición como esta para cada volumen replicado es normal y deseable. Cuando ya no esté en uso, puede eliminar esta partición de 512 KB; los en uso no se pueden eliminar. La partición nunca aumentará ni se reducirá. Si va a volver a crear la replicación, se recomienda que abandone la partición, ya que la réplica de almacenamiento reclamará los no utilizados.

Para ver los detalles, use la herramienta DiskPart o el cmdlet Get-Partition. Estas particiones tendrán el tipo GPT `558d43c5-a1ac-43c0-aac8-d1472b2923d1` .

## <a name="a-storage-replica-node-hangs-when-creating-snapshots"></a>Un nodo de réplica de almacenamiento se bloquea al crear instantáneas

Al crear una instantánea de VSS (a través de backup, VSSADMIN, etc.), se bloquea un nodo de réplica de almacenamiento y debe forzar el reinicio del nodo para que se recupere. No hay ningún error, solo se bloquea el servidor.

Este problema se produce cuando se crea una instantánea de VSS del volumen de registro. La causa subyacente es un aspecto de diseño heredado de VSS, no de la réplica de almacenamiento. El comportamiento resultante cuando se genera una instantánea del volumen de registro de réplica de almacenamiento es un mecanismo de poner de e/s de VSS que interbloquea el servidor.

Para evitar este comportamiento, no haga instantáneas de los volúmenes de registro de réplica de almacenamiento. No es necesario realizar instantáneas de los volúmenes de registro de réplica de almacenamiento, ya que estos registros no se pueden restaurar. Además, el volumen de registro nunca debe contener otras cargas de trabajo, por lo que no se necesita ninguna instantánea en general.

## <a name="high-io-latency-increase-when-using-storage-spaces-direct-with-storage-replica"></a>Aumento de latencia de e/s alta al usar Espacios de almacenamiento directo con réplica de almacenamiento

Al usar Espacios de almacenamiento directo con una memoria caché de NVME o SSD, verá un aumento de latencia mayor que el esperado al configurar la replicación de réplica de almacenamiento entre los clústeres de Espacios de almacenamiento directo. El cambio de latencia es proporcionalmente mucho mayor de lo que se ve cuando se usa NVME y SSD en una configuración de rendimiento y capacidad, y no en el nivel de capacidad y en el de HDD.

Este problema se produce debido a las limitaciones arquitectónicas en el mecanismo de registro de la réplica de almacenamiento combinado con la latencia extremadamente baja de NVME en comparación con los medios más lentos. Cuando se usa la memoria caché de Espacios de almacenamiento directo, todas las e/s de los registros de réplica de almacenamiento, junto con todas las operaciones de e/s de lectura/escritura recientes de las aplicaciones, se producirán en la memoria caché y nunca en los niveles de rendimiento o capacidad. Esto significa que toda la actividad de réplica de almacenamiento se produce en el mismo medio de velocidad (se admite esta configuración, pero no se recomienda https://aka.ms/srfaq ).

Cuando se usa Espacios de almacenamiento directo con HDD, no se puede deshabilitar o evitar la memoria caché. Como solución alternativa, si usa solo SSD y NVME, puede configurar solo los niveles de rendimiento y capacidad. Si se usa esa configuración y se colocan los registros de SR en el nivel de rendimiento únicamente con los volúmenes de datos que solo están en el nivel de capacidad, se evitará el problema de latencia alta que se ha descrito anteriormente. Lo mismo puede hacerse con una combinación de SSD más rápidas y lentas y sin NVME.

Esta solución alternativa no es idónea y es posible que algunos clientes no puedan usarla. El equipo de réplica de almacenamiento está trabajando en optimizaciones y un mecanismo de registro actualizado para el futuro con el fin de reducir estos cuellos de botella artificiales. Este registro v 1.1 comenzó a estar disponible en Windows Server 2019 y su rendimiento mejorado se describe en el [blog de almacenamiento del servidor](https://blogs.technet.microsoft.com/filecab/2018/12/13/chelsio-rdma-and-storage-replica-perf-on-windows-server-2019-are-💯/).

## <a name="error-could-not-find-file-when-running-test-srtopology-between-two-clusters"></a>Error "no se pudo encontrar el archivo" al ejecutar test-SRTopology entre dos clústeres

Al ejecutar test-SRTopology entre dos clústeres y sus rutas de CSV, se produce el error:

    PS C:\Windows\system32> Test-SRTopology -SourceComputerName NedClusterA -SourceVolumeName C:\ClusterStorage\Volume1 -SourceLogVolumeName L: -DestinationComputerName NedClusterB -DestinationVolumeName C:\ClusterStorage\Volume1 -DestinationLogVolumeName L: -DurationInMinutes 1 -ResultPath C:\Temp

    Validating data and log volumes...
    Measuring Storage Replica recovery and initial synchronization performance...
    WARNING: Could not find file '\\NedNode1\C$\CLUSTERSTORAGE\VOLUME1TestSRTopologyRecoveryTest\SRRecoveryTestFile01.txt'.
    WARNING: System.IO.FileNotFoundException
    WARNING:    at System.IO.__Error.WinIOError(Int32 errorCode, String maybeFullPath)
    at System.IO.FileStream.Init(String path, FileMode mode, FileAccess access, Int32 rights, Boolean useRights, FileShare share, Int32 buff
    erSize, FileOptions options, SECURITY_ATTRIBUTES secAttrs, String msgPath, Boolean bFromProxy, Boolean useLongPath, Boolean checkHost)
    at System.IO.FileStream..ctor(String path, FileMode mode, FileAccess access, FileShare share, Int32 bufferSize, FileOptions options)
    at Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand.GenerateWriteIOWorkload(String Path, UInt32 IoSizeInBytes, UInt32 Parallel
    IoCount, UInt32 Duration)
    at Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand.<>c__DisplayClass75_0.<PerformRecoveryTest>b__0()
    at System.Threading.Tasks.Task.Execute()
    Test-SRTopology : Could not find file '\\NedNode1\C$\CLUSTERSTORAGE\VOLUME1TestSRTopologyRecoveryTest\SRRecoveryTestFile01.txt'.
    At line:1 char:1
    + Test-SRTopology -SourceComputerName NedClusterA -SourceVolumeName  ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (:) [Test-SRTopology], FileNotFoundException
    + FullyQualifiedErrorId : TestSRTopologyFailure,Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand

Esto se debe a un defecto de código conocido en Windows Server 2016. Este problema se corrigió por primera vez en Windows Server, versión 1709 y las herramientas de RSAT asociadas. Para una resolución de nivel inferior, póngase en contacto con Soporte técnico de Microsoft y solicite una actualización del puerto. No hay ninguna solución alternativa.

## <a name="error-specified-volume-could-not-be-found-when-running-test-srtopology-between-two-clusters"></a>Error "no se pudo encontrar el volumen especificado" al ejecutar test-SRTopology entre dos clústeres

Al ejecutar test-SRTopology entre dos clústeres y sus rutas de CSV, se produce el error:

    PS C:\> Test-SRTopology -SourceComputerName RRN44-14-09 -SourceVolumeName C:\ClusterStorage\Volume1 -SourceLogVolumeName L: -DestinationComputerName RRN44-14-13 -DestinationVolumeName C:\ClusterStorage\Volume1 -DestinationLogVolumeName L: -DurationInMinutes 30 -ResultPath c:\report

    Test-SRTopology : The specified volume C:\ClusterStorage\Volume1 cannot be found on computer RRN44-14-09. If this is a cluster node, the volume must be part of a role or CSV; volumes in Available Storage are not accessible
    At line:1 char:1
    + Test-SRTopology -SourceComputerName RRN44-14-09 -SourceVolumeName C:\ ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : ObjectNotFound: (:) [Test-SRTopology], Exception
        + FullyQualifiedErrorId : TestSRTopologyFailure,Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand

Al especificar el CSV del nodo de origen como volumen de origen, debe seleccionar el nodo que posee el CSV. Puede trasladar el archivo CSV al nodo especificado o cambiar el nombre de nodo especificado en `-SourceComputerName` . Este error ha recibido un mensaje mejorado en Windows Server 2019.

## <a name="unable-to-access-the-data-drive-in-storage-replica-after-unexpected-reboot-when-bitlocker-is-enabled"></a>No se puede obtener acceso a la unidad de datos en la réplica de almacenamiento tras un reinicio inesperado cuando BitLocker está habilitado

Si BitLocker está habilitado en ambas unidades (unidad de registro y unidad de datos) y en las dos unidades de réplica de almacenamiento, si se reinicia el servidor principal, no podrá tener acceso a la unidad principal incluso después de desbloquear la unidad de registro de BitLocker.

Este es un comportamiento esperado. Para recuperar los datos o tener acceso a la unidad, primero debe desbloquear la unidad de registro y, a continuación, abrir diskmgmt. msc para buscar la unidad de datos. Vuelva a activar la unidad de datos sin conexión y en línea. Busque el icono de BitLocker en la unidad y desbloquee la unidad.

## <a name="issue-unlocking-the-data-drive-on-secondary-server-after-breaking-the-storage-replica-partnership"></a>Problema al desbloquear la unidad de datos en el servidor secundario después de interrumpir la Asociación de réplica de almacenamiento

Después de deshabilitar la Asociación de SR y quitar la réplica de almacenamiento, se espera si no puede desbloquear la unidad de datos del servidor secundario con su contraseña o clave respectivas.

Debe usar la clave o la contraseña de la unidad de datos del servidor principal para desbloquear la unidad de datos del servidor secundario.

## <a name="test-failover-doesnt-mount-when-using-asynchronous-replication"></a>La conmutación por error de prueba no se monta cuando se usa la replicación asincrónica

Al ejecutar Mount-SRDestination para poner en línea un volumen de destino como parte de la característica de conmutación por error de prueba, se produce el siguiente error:

    Mount-SRDestination: Unable to mount SR group <TEST>, detailed reason: The group or resource is not in the correct state to perform the supported operation.
    At line:1 char:1
    + Mount-SRDestination -ComputerName SRV1 -Name TEST -TemporaryP . . .
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (MSFT WvrAdminTasks : root/Microsoft/...(MSFT WvrAdminTasks : root/Microsoft/. T_WvrAdminTasks) (Mount-SRDestination], CimException
        + FullyQua1ifiedErrorId : Windows System Error 5823, Mount-SRDestination.

Si se usa un tipo de asociación sincrónica, la conmutación por error de prueba funciona con normalidad.

Esto se debe a un defecto de código conocido en Windows Server, versión 1709. Para resolver este problema, instale la [actualización del 18 de octubre de 2018](https://support.microsoft.com/help/4462932/windows-10-update-kb4462932). Este problema no está presente en Windows Server 2019 y Windows Server, versión 1809 y versiones más recientes.

## <a name="additional-references"></a>Referencias adicionales

- [Réplica de almacenamiento](storage-replica-overview.md)
- [Replicación de clúster extendido con almacenamiento compartido](stretch-cluster-replication-using-shared-storage.md)
- [Replicación de almacenamiento de servidor a servidor](server-to-server-storage-replication.md)
- [Replicación de almacenamiento de clúster a clúster](cluster-to-cluster-storage-replication.md)
- [Réplica de almacenamiento: Preguntas más frecuentes](storage-replica-frequently-asked-questions.md)
- [Espacios de almacenamiento directo](../storage-spaces/storage-spaces-direct-overview.md)
