---
title: "Problemas conocidos de Réplica de almacenamiento"
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 10/13/2016
ms.assetid: ceddb0fa-e800-42b6-b4c6-c06eb1d4bc55
ms.openlocfilehash: 6f02ece1f327cf53667df09e6d13ace001259885
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="known-issues-with-storage-replica"></a>Problemas conocidos de Réplica de almacenamiento

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En esta sección se describen los problemas conocidos de Réplica de almacenamiento en Windows Server 2016.  

## <a name="after-removing-replication-disks-are-offline-and-you-cannot-configure-replication-again"></a>Después de quitar la replicación, los discos están sin conexión y no se puede configurar la replicación de nuevo.  
En Windows Server 2016, es posible que no sea capaz de aprovisionar la replicación en un volumen que se replicó previamente o que encuentre volúmenes que no se pueden montar. Esto puede ocurrir cuando alguna condición de error evita la eliminación de replicación o cuando vuelve a instalar el sistema operativo en un equipo que anteriormente estaba replicando datos.  

Para solucionar el problema, debe borrar la partición oculta de Réplica de almacenamiento de los discos y devolver a estos a un estado de escritura con el cmdlet `Clear-SRMetadata`.  

-   Para quitar todas las ranuras huérfanas de bases de datos de partición de Réplica de almacenamiento y volver a montar todas las particiones, use el parámetro `-AllPartitions` como sigue:  

    ```  
    Clear-SRMetadata -AllPartitions  
    ```  

-   Para quitar todos los datos de registro huérfanos de Réplica de almacenamiento, utilice el parámetro `-AllLogs` como sigue:  

    ```  
    Clear-SRMetadata -AllLogs  
    ```  

-   Para quitar todos los datos de configuración huérfanos del clúster de conmutación por error, utilice el parámetro `-AllConfiguration` como sigue:  

    ```  
    Clear-SRMetadata -AllConfiguration  
    ```  

-   Para quitar los metadatos individuales del grupo de replicación, utilice el parámetro `-Name` y especifique un grupo de replicación como sigue:  

    ```  
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

Esto se produce al seleccionar un volumen de datos que se encuentra en la misma partición que la unidad del sistema (es decir, la unidad **C:** con su carpeta de Windows). Por ejemplo, en una unidad que contiene los volúmenes **C:** y **D:** creados desde la misma partición. Esto no se admite en Réplica de almacenamiento; debes elegir un volumen diferente para replicar.

## <a name="attempting-to-grow-a-replicated-volume-fails-due-to-missing-update"></a>No se puede ampliar un volumen replicado porque falta una actualización
Cuando intentas ampliar o extender un volumen replicado, recibes el error siguiente:

    PS C:\> Resize-Partition -DriveLetter d -Size 44GB
    Resize-Partition : The operation failed with return code 8
    At line:1 char:1
    + Resize-Partition -DriveLetter d -Size 44GB
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (StorageWMI:ROOT/Microsoft/.../MSFT_Partition
    [Resize-Partition], CimException
    + FullyQualifiedErrorId : StorageWMI 8,Resize-Partition

Si usas el complemento MMC de administración de discos, recibes este error: 

    Element not found

Esto ocurre incluso si habilitas correctamente el cambio de tamaño de volumen en el servidor de origen mediante `Set-SRGroup -Name rg01 -AllowVolumeResize $TRUE`. 

Este problema se corrigió en la actualización acumulativa para Windows10, versión1607, y Windows Server2016, de 9 de diciembre de 2016 (KB3201845). 

## <a name="attempting-to-grow-a-replicated-volume-fails-due-to-missing-step"></a>No se puede ampliar un volumen replicado porque falta un paso
Si intentas cambiar el tamaño de un volumen replicado en el servidor de origen sin configurar antes `-AllowResizeVolume $TRUE`, recibirás los siguientes errores:

    PS C:\> Resize-Partition -DriveLetter I -Size 8GB
    Resize-Partition : Failed
    Activity ID: {87aebbd6-4f47-4621-8aa4-5328dfa6c3be}
    At line:1 char:1
    + Resize-Partition -DriveLetter I -Size 8GB
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (StorageWMI:ROOT/Microsoft/.../MSFT_Partition) [Resize-Partition], CimException
        + FullyQualifiedErrorId : StorageWMI 4,Resize-Partition

Error de registro de eventos de Réplica de almacenamiento 10307:

    Attempted to resize a partition that is protected by Storage Replica .

    DeviceName: \Device\Harddisk1\DR1
    PartitionNumber: 7
    PartitionId: {b71a79ca-0efe-4f9a-a2b9-3ed4084a1822}

    Guidance: To grow a source data partition, set the policy on the replication group containing the data partition.

    Set-SRGroup -ComputerName [ComputerName] -Name [ReplicationGroupName] -AllowVolumeResize $true

    Before you grow the source data partition, ensure that the destination data partition has enough space to grow to an equal size. Shrinking of data partition protected by Storage Replica is blocked.

Error de complemento de administración de discos: 

    An unexpected error has occurred 

Después de cambiar el tamaño del volumen, recuerda que tienes que deshabilitar el cambio de tamaño con `Set-SRGroup -Name rg01 -AllowVolumeResize $FALSE`. Este parámetro impide que los administradores intenten cambiar el tamaño de los volúmenes antes de asegurarse de que hay espacio suficiente en el volumen de destino, normalmente porque no tenían conocimiento de la presencia de Réplica de almacenamiento. 

## <a name="attempting-to-move-a-pdr-resource-between-sites-on-an-asynchronous-stretch-cluster-fails"></a>No se puede mover un recurso PDR entre sitios en un clúster extendido asincrónico
Al intentar mover un rol adjunto a un recurso de disco físico, como un servidor de archivos para uso general, para mover el almacenamiento asociado en un clúster extendido asincrónico, recibe un error.

Si usa el complemento Administrador de clústeres de conmutación por error:

    Error
    The operation has failed.
    The action 'Move' did not complete.
    Error Code: 0x80071398
    The operation failed because either the specified cluster node is not the owner of the group, or the node is not a possible owner of the group
    
Si se usa el cmdlet de PowerShell Cluster:

    PS C:\> Move-ClusterGroup -Name sr-fs-006 -Node sr-srv07
    Move-ClusterGroup : An error occurred while moving the clustered role 'sr-fs-006'.
    The operation failed because either the specified cluster node is not the owner of the group, or the node is not a
    possible owner of the group
    At line:1 char:1
    + Move-ClusterGroup -Name sr-fs-006 -Node sr-srv07
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [Move-ClusterGroup], ClusterCmdletException
    + FullyQualifiedErrorId : Move-ClusterGroup,Microsoft.FailoverClusters.PowerShell.MoveClusterGroupCommand

Esto se produce debido a un comportamiento predeterminado de Windows Server2016. Usa `Set-SRPartnership` para mover estos discos de PDR a un clúster extendido asincrónico.  

## <a name="attempting-to-add-disks-to-a-two-node-asymmetric-cluster-returns-no-disks-suitable-for-cluster-disks-found"></a>Al intentar agregar discos a un clúster asimétrico de dos nodos, se devuelve "No se han encontrado discos adecuados para los discos de clúster". 
Al intentar aprovisionar un clúster con solo dos nodos, antes de añadir la replicación elástica de réplica de almacenamiento, intente agregar los discos en el segundo sitio a los discos disponibles. Recibe el siguiente error:

    "No disks suitable for cluster disks found. For diagnostic information about disks available to the cluster, use the Validate a Configuration Wizard to run Storage tests." 

Esto no ocurre si tiene al menos tres nodos en el clúster. Este problema se produce debido a un cambio de código predeterminado en Windows Server 2016 para comportamientos de clústeres de almacenamiento asimétrico. 

Para agregar el almacenamiento, puedes ejecutar el siguiente comando en el nodo del segundo sitio:

`Get-ClusterAvailableDisk -All | Add-ClusterDisk`

Esto no funcionará con almacenamiento local de nodos. Puedes usar réplica de almacenamiento para replicar un clúster extendido entre dos nodos totales, **cada uno con su propio conjunto de almacenamiento compartido**. 

## <a name="the-smb-bandwidth-limiter-fails-to-throttle-storage-replica-bandwidth"></a>El limitador de ancho de banda SMB no es capaz de limitar el ancho de banda de la réplica de almacenamiento
Cuando se especifica un límite de ancho de banda en la réplica del almacenamiento, el límite se omite y se usa el ancho de banda al completo. Por ejemplo:

`Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond 32MB`

Este problema se produce debido a un problema de interoperabilidad entre la réplica de almacenamiento y SMB. Este problema se solucionó por primera vez en la actualización acumulativa de julio de 2017 de Windows Server 2016 y en Windows Server, versión 1709.

## <a name="event-1241-warning-repeated-during-initial-sync"></a>La advertencia de evento 1241 se repite durante la sincronización inicial
Cuando se especifica que una asociación de replicación es asincrónica, el equipo de origen registra repetidamente la advertencia de evento 1241 en el canal de administración de la réplica de almacenamiento. Por ejemplo:

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

El destino asincrónico está actualmente desconectado. El objetivo de punto de recuperación (RPO) puede estar disponible una vez se restaure la conexión.

    The asynchronous destination is unable to keep pace with the source such that the most recent destination log record is no longer present in the source log. The destination will start block copying. The RPO should become available after block copying completes.

Este es el comportamiento que se espera durante la sincronización inicial y se puede pasar por alto sin problema. Este comportamiento puede cambiar en una versión posterior. Si ves este comportamiento durante la replicación asincrónica en curso, échale un vistazo a la asociación para determinar por qué la replicación se retrasa más allá del RPO configurado (30 segundos de manera predeterminada).

## <a name="event-4004-warning-repeated-after-rebooting-a-replicated-node"></a>La advertencia de evento 4004 se repite después de reiniciar un nodo replicado
Si reinicias un servidor que se encuentre en una asociación, puede ocurrir que la replicación falle y el nodo reiniciado registre el evento de advertencia 4007 con un error de acceso denegado; recuerda que esto solo ocurre en circunstancias excepcionales y normalmente irreproducibles.

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

Anota `Status: "{Access Denied}"` y el mensaje `A process has requested access to an object, but has not been granted those access rights.` Se trata de un problema conocido en la Réplica de almacenamiento y se solucionó en la actualización de calidad del 12 de septiembre de 2017: KB4038782 (compilación del sistema operativo 14393.1715) https://support.microsoft.com/es-es/help/4038782/windows-10-update-kb4038782 

## <a name="error-failed-to-bring-the-resource-cluster-disk-x-online-with-a-stretch-cluster"></a>Error "No se pudo conectar el recurso "Disco de clúster x" en línea". con un clúster extendido
Al intentar conectar un disco de clúster después de realizar una conmutación por error correcta en la que intentas que el sitio de origen original vuelva a ser el sitio primario, recibes un error en el Administrador de clústeres de conmutación por error. Por ejemplo:

    Error
    The operation has failed.
    Failed to bring the resource 'Cluster Disk 2' online.
    
    Error Code: 0x80071397
    The operation failed because either the specified cluster node is not the owner of the resource, or the node is not a possible owner of the resource.
    
Si intentas mover el disco o CSV de forma manual, verás un error adicional. Por ejemplo:

    Error
    The operation has failed.
    The action 'Move' did not complete.
    
    Error Code: 0x8007138d
    A cluster node is not available for this operation

Este problema lo causa uno o más discos sin inicializar y que se han adjuntado a uno o más nodos de clúster. Para resolver este problema, debes inicializar todas las interfaces de almacenamiento adjuntas mediante DiskMgmt.msc, DISKPART.EXE o el cmdlet de PowerShell Initialize-Disk.

Estamos trabajando para proporcionarte una actualización que te permita resolver este problema de forma permanente. Si quieres echarnos una mano y tienes un contrato de soporte técnico Premier de Microsoft, envía un correo electrónico a SRFEED@microsoft.com para que podamos ayudarte a presentar una solicitud de corrección compatible.

## <a name="gpt-error-when-attempting-to-create-a-new-sr-partnership"></a>Error GPT al intentar crear una nueva asociación de SR

Cuando se ejecuta New-SRPartnership, se produce el siguiente error: 

    Disk layout type for volume \\?\Volume{GUID}\ is not a valid GPT style layout.
    New-SRPartnership : Unable to create replication group SRG01, detailed reason: Disk layout type for volume
    \\?\Volume{GUID}\ is not a valid GPT style layout.
    At line:1 char:1
    + New-SRPartnership -SourceComputerName nodesrc01 -SourceRGName SRG01 ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (MSFT_WvrAdminTasks:root/Microsoft/...T_WvrAdminTasks) [New-SRPartnership]
    , CimException
    + FullyQualifiedErrorId : Windows System Error 5078,New-SRPartnership

En la interfaz gráfica de usuario (GUI) del Administrador de clústeres de conmutación por error, no hay ninguna opción para configurar la replicación del disco.

Cuando se ejecuta Test-SRTopology, se produce el siguiente error: 

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

Esto se debe a que el nivel funcional del clúster aún se establece en Windows Server 2012 R2 (por ejemplo, FL 8). La réplica de almacenamiento debe devolver un error específico aquí, pero en su lugar, devuelve una asignación de error incorrecta.

Ejecuta Get-Cluster | fl * en todos los nodos.

Si ClusterFunctionalLevel = 9, quiere decir que es la versión de Windows 2016 ClusterFunctionalLevel necesaria para implementar la réplica de almacenamiento en este nodo.
Si ClusterFunctionalLevel no es 9, debes actualizar ClusterFunctionalLevel para poder implementar la réplica de almacenamiento en este nodo.

Para resolver el problema, ejecuta el cmdlet de PowerShell para generar el nivel funcional del clúster: Update-ClusterFunctionalLevel https://technet.microsoft.com/itpro/powershell/windows/failoverclusters/update-clusterfunctionallevel

## <a name="small-unknown-partition-listed-in-diskmgmt-for-each-replicated-volume"></a>Pequeña partición desconocida mostrada en DISKMGMT por cada volumen replicado

Al ejecutar ejecuta el complemento de administración de discos (DISKMGMT.MSC), se observa que aparecen uno o más volúmenes sin etiqueta o con la letra de unidad y 1MB de tamaño. Es posible que puedas eliminar el volumen desconocido o puede que recibas:

    "An Unexpected Error has Occurred"  

Este comportamiento es así por diseño. No s trata de un volumen, sino de una partición. La Réplica de almacenamiento crea una partición de 512KB como un espacio de base de datos para las operaciones de replicación (la herramienta DiskMgmt.msc heredada redondea al MB más cercano). Disponer de una partición así para cada volumen replicado es normal y deseable. Cuando ya no se use, eres libre de eliminar esta partición de512 KB; las que están en uso no se pueden eliminar. La partición nunca aumenta ni se reduce. Si recreas una replicación, se recomienda dejar la partición, ya que la Réplica de almacenamiento reclamará las que no se usen.

Para ver los detalles, usa la herramienta DISKPART o el cmdlet Get-Partition. Estas particiones tendrán un tipo de GPT de `558d43c5-a1ac-43c0-aac8-d1472b2923d1`. 

## <a name="see-also"></a>Consulta también  
- [Réplica de almacenamiento](storage-replica-overview.md)  
- [Replicación de clúster extendido con almacenamiento compartido](stretch-cluster-replication-using-shared-storage.md)  
- [Replicación de almacenamiento de servidor a servidor](server-to-server-storage-replication.md)  
- [Replicación de almacenamiento de clúster a clúster](cluster-to-cluster-storage-replication.md)  
- [Réplica de almacenamiento: preguntas frecuentes](storage-replica-frequently-asked-questions.md)  
- [Espacios de almacenamiento directos](../storage-spaces/storage-spaces-direct-overview.md)  
