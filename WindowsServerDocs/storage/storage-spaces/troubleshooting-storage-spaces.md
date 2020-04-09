---
title: Solución de problemas de Espacios de almacenamiento directo
description: Obtenga información acerca de cómo solucionar problemas de la implementación de Espacios de almacenamiento directo.
ms.prod: windows-server
ms.author: ''
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: 429eddf30fddf6bfd035d1f928196a3b66d14646
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820948"
---
# <a name="troubleshoot-storage-spaces-direct"></a>Solucionar problemas Espacios de almacenamiento directo

> Se aplica a: Windows Server 2019, Windows Server 2016

Use la siguiente información para solucionar los problemas de la implementación de Espacios de almacenamiento directo.

En general, empiece con los pasos siguientes:

1. Confirme que la marca o el modelo de SSD está certificado para Windows Server 2016 y Windows Server 2019 mediante el catálogo de Windows Server. Confirme con el proveedor que las unidades son compatibles con Espacios de almacenamiento directo.
2. Inspeccione el almacenamiento de las unidades defectuosas. Use software de administración de almacenamiento para comprobar el estado de las unidades. Si alguna de las unidades es defectuosa, trabaje con el proveedor. 
3. Actualice el firmware de la unidad y el almacenamiento si es necesario.
   Asegúrese de que las actualizaciones más recientes de Windows están instaladas en todos los nodos. Puede obtener las actualizaciones más recientes de Windows Server 2016 desde el historial de actualizaciones de Windows [10 y Windows server 2016](https://aka.ms/update2016) y para windows Server 2019 desde el historial de actualizaciones de Windows [10 y Windows Server 2019](https://support.microsoft.com/help/4464619).
4. Actualice el firmware y los controladores del adaptador de red.
5. Ejecute la validación del clúster y revise la sección espacio de almacenamiento directo, asegúrese de que las unidades que se usarán para la memoria caché se notifiquen correctamente y que no hay errores.

Si sigue teniendo problemas, revise los escenarios siguientes.

## <a name="virtual-disk-resources-are-in-no-redundancy-state"></a>Los recursos de disco virtual no están en estado de redundancia
Los nodos de un Espacios de almacenamiento directo reiniciar el sistema de forma inesperada debido a un error de alimentación o de bloqueo. Después, es posible que uno o varios de los discos virtuales no se conecten y vea la descripción "no hay suficiente información de redundancia".

|FriendlyName|ResiliencySettingName| OperationalStatus| HealthStatus| IsManualAttach|Tamaño| PSComputerName|
|------------|---------------------| -----------------| ------------| --------------|-----| --------------|
|Disk4| Crear reflejo| ACEPTAR|  Correcto| True|  10 TB|  Node-01. cont...|
|Disk3         |Crear reflejo                 |ACEPTAR                          |Correcto       |True            |10 TB | Node-01. cont...|
|Disk2         |Crear reflejo                 |Sin redundancia               |Incorrecto     |True            |10 TB | Node-01. cont...|
|Disk1         |Crear reflejo                 |{Sin redundancia, inservicio}  |Incorrecto     |True            |10 TB | Node-01. cont...| 

Además, después de intentar conectar el disco virtual, se registra la siguiente información en el registro del clúster (DiskRecoveryAction).  

```
[Verbose] 00002904.00001040::YYYY/MM/DD-12:03:44.891 INFO [RES] Physical Disk <DiskName>: OnlineThread: SuGetSpace returned 0.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 WARN [RES] Physical Disk < DiskName>: Underlying virtual disk is in 'no redundancy' state; its volume(s) may fail to mount.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 ERR [RES] Physical Disk <DiskName>: Failing online due to virtual disk in 'no redundancy' state. If you would like to attempt to online the disk anyway, first set this resource's private property 'DiskRecoveryAction' to 1. We will try to bring the disk online for recovery, but even if successful, its volume(s) or CSV may be unavailable. 
``` 

El **estado operativo sin redundancia** puede producirse si se produce un error en un disco o si el sistema no puede obtener acceso a los datos del disco virtual. Este problema puede producirse si se produce un reinicio en un nodo durante el mantenimiento de los nodos.

Para corregir este problema, siga estos pasos:

1. Quite los discos virtuales afectados de CSV. Esto los colocará en el grupo "almacenamiento disponible" del clúster y empezará a mostrarse como ResourceType de "disco físico".

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. En el nodo que posee el grupo de almacenamiento disponible, ejecute el siguiente comando en todos los discos que no tengan redundancia. Para identificar el nodo en el que está el grupo "almacenamiento disponible", puede ejecutar el siguiente comando.

   ```powershell
   Get-ClusterGroup
   ```
3. Establezca la acción de recuperación de disco y, a continuación, inicie los discos.
   ```powershell
   Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 1
   Start-ClusterResource -Name "VdiskName"
   ```
4. Una reparación debe iniciarse automáticamente. Espere a que finalice la reparación. Puede entrar en un estado suspendido y volver a iniciarse. Para supervisar el progreso: 
    - Ejecute **Get-StorageJob** para supervisar el estado de la reparación y ver cuándo se ha completado.
    - Ejecute **Get-VirtualDisk** y compruebe que el espacio devuelve un HealthStatus de correcto.
5. Una vez finalizada la reparación y los discos virtuales sean correctos, vuelva a cambiar los parámetros del disco virtual.

   ```powershell
    Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 0
   ```
6. Desconectar los discos y volver a ponerlos en línea para que los DiskRecoveryAction surtan efecto:

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ``` 
7. Vuelva a agregar los discos virtuales afectados a CSV.

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```

**DiskRecoveryAction** es un modificador de invalidación que permite adjuntar el volumen de espacio en modo de lectura y escritura sin ninguna comprobación. La propiedad le permite realizar diagnósticos en el motivo por el que un volumen no se conectará. Es muy similar al modo de mantenimiento, pero se puede invocar en un recurso en un estado de error. También permite tener acceso a los datos, lo que puede resultar útil en situaciones como "sin redundancia", donde puede obtener acceso a los datos que pueda y copiar. La propiedad DiskRecoveryAction se agregó en el 22 de febrero de 2018, actualización, KB 4077525.


## <a name="detached-status-in-a-cluster"></a>Estado desasociado en un clúster 

Al ejecutar el cmdlet **Get-VirtualDisk** , el OperationalStatus de uno o más discos virtuales espacios de almacenamiento directo está separado. Sin embargo, el HealthStatus indicado por el cmdlet **Get-PhysicalDisk** indica que todos los discos físicos están en un estado correcto.

A continuación se muestra un ejemplo de la salida del cmdlet **Get-VirtualDisk** .

|FriendlyName|  ResiliencySettingName|  OperationalStatus|   HealthStatus|  IsManualAttach|  Tamaño|   PSComputerName|
|-|-|-|-|-|-|-|
|Disk4|         Crear reflejo|                 ACEPTAR|                  Correcto|       True|            10 TB|  Node-01. cont...|
|Disk3|         Crear reflejo|                 ACEPTAR|                  Correcto|       True|            10 TB|  Node-01. cont...|
|Disk2|         Crear reflejo|                 Desasociado|            Desconocida|       True|            10 TB|  Node-01. cont...|
|Disk1|         Crear reflejo|                 Desasociado|            Desconocida|       True|            10 TB|  Node-01. cont...| 


Además, los siguientes eventos se pueden registrar en los nodos:

```
Log Name: Microsoft-Windows-StorageSpaces-Driver/Operational
Source: Microsoft-Windows-StorageSpaces-Driver 
Event ID: 311 
Level: Error
User: SYSTEM 
Computer: Node#.contoso.local 
Description: Virtual disk {GUID} requires a data integrity scan.  

Data on the disk is out-of-sync and a data integrity scan is required. 

To start the scan, run the following command:   
Get-ScheduledTask -TaskName "Data Integrity Scan for Crash Recovery" | Start-ScheduledTask                

Once you have resolved the condition listed above, you can online the disk by using the following commands in PowerShell:   

Get-VirtualDisk | ?{ $_.ObjectId -Match "{GUID}" } | Get-Disk | Set-Disk -IsReadOnly $false 
Get-VirtualDisk | ?{ $_.ObjectId -Match "{GUID}" } | Get-Disk | Set-Disk -IsOffline  $false
------------------------------------------------------------

Log Name: System
Source: Microsoft-Windows-ReFS 
Event ID: 134
Level: Error 
User: SYSTEM
Computer: Node#.contoso.local 
Description: The file system was unable to write metadata to the media backing volume <VolumeId>. A write failed with status "A device which does not exist was specified." ReFS will take the volume offline. It may be mounted again automatically.
------------------------------------------------------------
Log Name: Microsoft-Windows-ReFS/Operational
Source: Microsoft-Windows-ReFS 
Event ID: 5 
Level: Error 
User: SYSTEM 
Computer: Node#.contoso.local 
Description: ReFS failed to mount the volume. 
Context: 0xffffbb89f53f4180 
Error: A device which does not exist was specified.
Volume GUID:{00000000-0000-0000-0000-000000000000} 
DeviceName: 
Volume Name:
``` 

El **estado operativo desasociado** puede producirse si el registro de seguimiento de la región DESFASADA (DRT) está lleno. Los espacios de almacenamiento usan el seguimiento de regiones desfasadas (DRT) para los espacios reflejados para asegurarse de que cuando se produce un error de alimentación, se registran las actualizaciones en curso en los metadatos para asegurarse de que el espacio de almacenamiento puede rehacer o deshacer las operaciones para volver a poner el espacio de almacenamiento en un estado flexible y coherente cuando se restaure la energía y el sistema Si el registro de DRT está lleno, el disco virtual no se puede poner en línea hasta que los metadatos de DRT se sincronicen y se vacíen. Este proceso requiere ejecutar un examen completo, que puede tardar varias horas en completarse.

Para corregir este problema, siga estos pasos:
1. Quite los discos virtuales afectados de CSV.

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. Ejecute los siguientes comandos en todos los discos que no estén en línea. 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 7
   Start-ClusterResource -Name "VdiskName"
   ``` 
3. Ejecute el siguiente comando en todos los nodos en los que el volumen desasociado esté en línea. 

   ```powershell
   Get-ScheduledTask -TaskName "Data Integrity Scan for Crash Recovery" | Start-ScheduledTask 
   ```
   Esta tarea debe iniciarse en todos los nodos en los que el volumen desasociado esté en línea. Una reparación debe iniciarse automáticamente. Espere a que finalice la reparación. Puede entrar en un estado suspendido y volver a iniciarse. Para supervisar el progreso: 
   - Ejecute **Get-StorageJob** para supervisar el estado de la reparación y ver cuándo se ha completado.
   - Ejecute **Get-VirtualDisk** y compruebe que el espacio devuelve un HealthStatus de correcto.
     - La "exploración de integridad de datos para la recuperación de bloqueos" es una tarea que no se muestra como un trabajo de almacenamiento y no hay ningún indicador de progreso. Si la tarea se muestra como en ejecución, se está ejecutando. Cuando finalice, se mostrará completado.

       Además, puede ver el estado de una tarea de programación en ejecución mediante el siguiente cmdlet: 
       ```powershell
       Get-ScheduledTask | ? State -eq running
       ``` 
4. En cuanto finalice el "examen de integridad de datos para la recuperación de bloqueos", la reparación finaliza y los discos virtuales son correctos, vuelva a cambiar los parámetros del disco virtual. 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 0 
   ```

5. Desconectar los discos y volver a ponerlos en línea para que los DiskRecoveryAction surtan efecto:

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ``` 

6. Vuelva a agregar los discos virtuales afectados a CSV.    

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```  
   El **valor de DiskRunChkdsk 7** se usa para adjuntar el volumen de espacio y la partición en modo de solo lectura. Esto permite que los espacios se detecten automáticamente y se solucionen mediante la activación de una reparación. La reparación se ejecutará automáticamente una vez montado. También permite tener acceso a los datos, lo que puede resultar útil para obtener acceso a los datos que se pueden copiar. En el caso de algunas condiciones de error, como un registro de DRT completo, debe ejecutar la tarea programada examen de integridad de datos para recuperación de bloqueos.

La **tarea análisis de integridad de datos para recuperación de bloqueos** se usa para sincronizar y borrar un registro de seguimiento de regiones DESFASADAS (DRT) completo. Esta tarea puede tardar varias horas en completarse. La "exploración de integridad de datos para la recuperación de bloqueos" es una tarea que no se muestra como un trabajo de almacenamiento y no hay ningún indicador de progreso. Si la tarea se muestra como en ejecución, se está ejecutando. Cuando finalice, se mostrará como completado. Si cancela la tarea o reinicia un nodo mientras esta tarea se está ejecutando, la tarea deberá empezar de nuevo desde el principio.

Para obtener más información, consulte [solución de problemas de estado de espacios de almacenamiento directo y Estados operativos](storage-spaces-states.md).

## <a name="event-5120-with-status_io_timeout-c00000b5"></a>Evento 5120 con STATUS_IO_TIMEOUT c00000b5 

> [!Important]
> **Para Windows Server 2016:** Para reducir la posibilidad de experimentar estos síntomas mientras se aplica la actualización con la corrección, se recomienda usar el procedimiento de modo de mantenimiento del almacenamiento que se indica a continuación para instalar la [actualización acumulativa del 18 de octubre de 2018,](https://support.microsoft.com/help/4462928) que se ha lanzado desde 2016 el [2018 8 de mayo](https://support.microsoft.com/help/4103723) de 2016 al [9 de octubre de 2018](https://support.microsoft.com/help/KB4462917).

Es posible que reciba el evento 5120 con STATUS_IO_TIMEOUT c00000b5 después de reiniciar un nodo en Windows Server 2016 con la actualización acumulativa publicada desde el [8 de mayo de 2018 kb 4103723](https://support.microsoft.com/help/4103723) hasta el [9 de octubre de 2018 KB 4462917](https://support.microsoft.com/help/4462917) instalado.

Al reiniciar el nodo, el evento 5120 se registra en el registro de eventos del sistema e incluye uno de los siguientes códigos de error:

```
Event Source: Microsoft-Windows-FailoverClustering
Event ID: 5120
Description:    Cluster Shared Volume 'CSVName' ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_IO_TIMEOUT(c00000b5)'. All I/O will temporarily be queued until a path to the volume is reestablished. 

Cluster Shared Volume 'CSVName' ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_CONNECTION_DISCONNECTED(c000020c)'. All I/O will temporarily be queued until a path to the volume is reestablished.    
```

Cuando se registra un evento 5120, se genera un volcado de memoria en directo para recopilar información de depuración que puede provocar síntomas adicionales o tener un efecto en el rendimiento. La generación del volcado en vivo crea una breve pausa para habilitar la toma de una instantánea de la memoria para escribir el archivo de volcado. Los sistemas que tienen una gran cantidad de memoria y que están sobrecargados pueden provocar que los nodos salgan de la pertenencia al clúster y que también se registre el siguiente evento 1135.

```
Event source: Microsoft-Windows-FailoverClustering
Event ID: 1135  
Description: Cluster node 'NODENAME'was removed from the active failover cluster membership. The Cluster service on this node may have stopped. This could also be due to the node having lost communication with other active nodes in the failover cluster. Run the Validate a Configuration wizard to check your network configuration. If the condition persists, check for hardware or software errors related to the network adapters on this node. Also check for failures in any other network components to which the node is connected such as hubs, switches, or bridges.
```

Un cambio introducido en el 8 de mayo de 2018 a Windows Server 2016, que era una actualización acumulativa para agregar identificadores resistentes de SMB para las sesiones de red SMB de Espacios de almacenamiento directo dentro del clúster. Esto se hizo para mejorar la resistencia a los errores de red transitorios y mejorar el modo en que RoCE controla la congestión de la red. Estas mejoras también han aumentado involuntariamente los tiempos de espera cuando las conexiones SMB intentan volver a conectarse y esperan a que se agote el tiempo de espera cuando se reinicia un nodo. Estos problemas pueden afectar a un sistema que está bajo esfuerzo. Durante el tiempo de inactividad no planeado, también se han observado las pausas de e/s de hasta 60 segundos mientras el sistema espera a que se agoten las conexiones. Para corregir este problema, instale la [actualización acumulativa del 18 de octubre de 2018 para Windows Server 2016](https://support.microsoft.com/help/4462928) o una versión posterior.

*Nota:* Esta actualización alinea los tiempos de espera de CSV con los tiempos de espera de conexión SMB para corregir este problema. No implementa los cambios para deshabilitar la generación de volcado en directo que se menciona en la sección solución alternativa.

### <a name="shutdown-process-flow"></a>Flujo del proceso de apagado:

1. Ejecute el cmdlet Get-VirtualDisk y asegúrese de que el valor de HealthStatus es correcto.
2. Vacíe el nodo mediante la ejecución del siguiente cmdlet:

   ```powershell
   Suspend-ClusterNode -Drain
   ```
3. Coloque los discos en ese nodo en modo de mantenimiento de almacenamiento ejecutando el siguiente cmdlet: 

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Enable-StorageMaintenanceMode
   ```
4. Ejecute el cmdlet **Get-PhysicalDisk** y asegúrese de que el valor de OperationalStatus está en modo de mantenimiento.
5. Ejecute el cmdlet **restart-Computer** para reiniciar el nodo.
6. Después de reiniciar el nodo, quite los discos de ese nodo del modo de mantenimiento de almacenamiento mediante la ejecución del siguiente cmdlet:

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Disable-StorageMaintenanceMode
   ```
7. Para reanudar el nodo, ejecute el siguiente cmdlet:

   ```powershell
   Resume-ClusterNode
   ```
8. Compruebe el estado de los trabajos de resincronización mediante la ejecución del siguiente cmdlet:

   ```powershell
   Get-StorageJob
   ```

### <a name="disabling-live-dumps"></a>Deshabilitar volcados en directo
Para mitigar el efecto de la generación de un volcado de memoria en directo en sistemas que tienen una gran cantidad de memoria y están bajo esfuerzo, puede que desee deshabilitar la generación de volcado en vivo. A continuación se proporcionan tres opciones.

>[!Caution]
>Este procedimiento puede impedir la recopilación de información de diagnóstico que Soporte técnico de Microsoft puede necesitar para investigar este problema. Un agente de soporte técnico puede tener que pedirle que vuelva a habilitar la generación de volcados de memoria en vivo en función de escenarios de solución de problemas específicos.

Hay dos métodos para deshabilitar los volcados en directo, como se describe a continuación.

#### <a name="method-1-recommended-in-this-scenario"></a>Método 1 (recomendado en este escenario)
Para deshabilitar completamente todos los volcados de memoria, incluidos los volcados en vivo en todo el sistema, siga estos pasos:

1. Cree la siguiente clave del registro: HKLM\System\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled
2. En la nueva clave **ForceDumpsDisabled** , cree una propiedad REG_DWORD como GuardedHost y, a continuación, establezca su valor en 0x10000000.
3. Aplique la nueva clave del registro a cada nodo del clúster.

>[!NOTE]
>Tendrá que reiniciar el equipo para que el cambio de NLA surta efecto.

Después de establecer esta clave del registro, se producirá un error en la creación del volcado en vivo y se generará un error "STATUS_NOT_SUPPORTED".

#### <a name="method-2"></a>Método 2
De forma predeterminada, Informe de errores de Windows solo permitirá un LiveDump por cada tipo de informe en 7 días y solo 1 LiveDump por equipo cada 5 días. Puede cambiarlo estableciendo las siguientes claves del registro para que solo se permita un LiveDump en la máquina de forma indefinida.
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v SystemThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v ComponentThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```

*Nota:* Tendrá que reiniciar el equipo para que el cambio surta efecto.

### <a name="method-3"></a>Método 3
Para deshabilitar la generación de clústeres de volcados en vivo (por ejemplo, cuando se registra un evento 5120), ejecute el siguiente cmdlet:

```powershell
(Get-Cluster).DumpPolicy = ((Get-Cluster).DumpPolicy -band 0xFFFFFFFFFFFFFFFE)
```
Este cmdlet tiene un efecto inmediato en todos los nodos del clúster sin reiniciar el equipo.

## <a name="slow-io-performance"></a>Rendimiento de e/s lento

Si ve un rendimiento de e/s lento, compruebe si la memoria caché está habilitada en la configuración de Espacios de almacenamiento directo. 

Hay dos maneras de comprobar: 


1. Usar el registro de clúster. Abra el registro del clúster en el editor de texto que prefiera y busque "[= = = SBL Disks = = =]". Se trata de una lista del disco en el nodo en el que se generó el registro. 

     Ejemplos de discos habilitados para caché: tenga en cuenta que el estado es CacheDiskStateInitializedAndBound y aquí hay un GUID. 

   ```
   [=== SBL Disks ===]
    {26e2e40f-a243-1196-49e3-8522f987df76},3,false,true,1,48,{1ff348f1-d10d-7a1a-d781-4734f4440481},CacheDiskStateInitializedAndBound,1,8087,54,false,false,HGST    ,HUH721010AL4200 ,        7PG3N2ER,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Caché no habilitada: aquí podemos ver que no hay ningún GUID presente y el estado es CacheDiskStateNonHybrid. 
    ```
   [=== SBL Disks ===]
    {426f7f04-e975-fc9d-28fd-72a32f811b7d},12,false,true,1,24,{00000000-0000-0000-0000-000000000000},CacheDiskStateNonHybrid,0,0,0,false,false,HGST    ,HUH721010AL4200 ,        7PGXXG6C,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Caché no habilitada: cuando todos los discos son del mismo tipo, no está habilitado de forma predeterminada. Aquí podemos ver que no hay ningún GUID presente y el estado es CacheDiskStateIneligibleDataPartition. 
    ```
    {d543f90c-798b-d2fe-7f0a-cb226c77eeed},10,false,false,1,20,{00000000-0000-0000-0000-000000000000},CacheDiskStateIneligibleDataPartition,0,0,0,false,false,NVMe    ,INTEL SSDPE7KX02,  PHLF7330004V2P0LGN,0170,{79b4d631-976f-4c94-a783-df950389fd38},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0], 
    ```  
2. Usar Get-PhysicalDisk. XML de SDDCDiagnosticInfo
    1. Abra el archivo XML mediante "$d = Import-Clixml GetPhysicalDisk. XML".
    2. Ejecutar "almacenamiento de IPMO"
    3. Ejecute "$d". Tenga en cuenta que el uso se realiza de forma automática, no del diario. verá un resultado similar al siguiente: 

   |FriendlyName|  SerialNumber| MediaType| CanPool| OperationalStatus| HealthStatus| Uso| Tamaño|
   |-----------|------------|---------| -------| -----------------| ------------| -----| ----|
   |NVMe INTEL SSDPE7KX02| PHLF733000372P0LGN| SSD| False|   ACEPTAR|                Correcto|      Selección automática 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504008J2P0LGN| SSD|  False|    ACEPTAR|                Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504005F2P0LGN| SSD|  False|  ACEPTAR|                Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002A2P0LGN| SSD| False| ACEPTAR|    Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504004T2P0LGN |SSD| False|ACEPTAR|       Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002E2P0LGN| SSD| False| ACEPTAR|      Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330002Z2P0LGN| SSD| False| ACEPTAR|      Correcto|Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF733000272P0LGN |SSD| False| ACEPTAR|  Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330001J2P0LGN |SSD| False| ACEPTAR| Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF733000302P0LGN |SSD| False| ACEPTAR|Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7330004D2P0LGN |SSD| False| ACEPTAR| Correcto| Selección automática |1,82 TB|

## <a name="how-to-destroy-an-existing-cluster-so-you-can-use-the-same-disks-again"></a>Cómo destruir un clúster existente para que pueda volver a usar los mismos discos

En un clúster de Espacios de almacenamiento directo, una vez que deshabilite Espacios de almacenamiento directo y use el proceso de limpieza descrito en [limpiar unidades](deploy-storage-spaces-direct.md#step-31-clean-drives), el grupo de almacenamiento en clúster permanece en un estado sin conexión y el servicio de mantenimiento se quita del clúster.

El siguiente paso consiste en quitar el grupo de almacenamiento fantasma: 
   ```powershell
   Get-ClusterResource -Name "Cluster Pool 1" | Remove-ClusterResource
   ```

Ahora, si ejecuta **Get-PhysicalDisk** en cualquiera de los nodos, verá todos los discos que se encontraban en el grupo. Por ejemplo, en un laboratorio con un clúster de 4 nodos con 4 discos SAS, 100 GB cada uno se presentó a cada nodo. En ese caso, después de que el espacio de almacenamiento directo esté deshabilitado, lo que quita el SBL (capa de bus de almacenamiento) pero deja el filtro, si ejecuta **Get-PhysicalDisk**, debe notificar 4 discos, excepto el disco del sistema operativo local. En su lugar, se ha indicado 16 en su lugar. Esto es lo mismo para todos los nodos del clúster. Al ejecutar un comando **Get-Disk** , verá los discos conectados localmente numerados como 0, 1, 2, etc., tal como se muestra en este resultado de ejemplo:

|Número| Nombre descriptivo| Número de serie|HealthStatus|OperationalStatus|Tamaño total| Estilo de partición|
|-|-|-|-|-|-|-|-|
|0|Msft Virtu...  ||Correcto | En línea|  127 GB| GPT|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| RAW|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| RAW|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| RAW|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| RAW|
|1|Msft Virtu...||Correcto| Desconectado| 100 GB| RAW|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| RAW|
|2|Msft Virtu...||Correcto| Desconectado| 100 GB| RAW|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| RAW|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| RAW|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| RAW|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| RAW|
|4|Msft Virtu...||Correcto| Desconectado| 100 GB| RAW|
|3|Msft Virtu...||Correcto| Desconectado| 100 GB| RAW|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| RAW|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| RAW|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| RAW|


## <a name="error-message-about-unsupported-media-type-when-you-create-an-storage-spaces-direct-cluster-using-enable-clusters2d"></a>Mensaje de error sobre "tipo de medio no admitido" al crear un clúster de Espacios de almacenamiento directo con enable-ClusterS2D  

Es posible que vea errores similares a este al ejecutar el cmdlet **enable-ClusterS2D** :

![Mensaje de error del escenario 6](media/troubleshooting/scenario-error-message.png)

Para corregir este problema, asegúrese de que el adaptador de HBA está configurado en el modo HBA. No se debe configurar ningún HBA en modo RAID.  

## <a name="enable-clusterstoragespacesdirect-hangs-at-waiting-until-sbl-disks-are-surfaced-or-at-27"></a>Enable-ClusterStorageSpacesDirect se bloquea en ' en espera hasta que se muestran discos SBL ' o en un 27%

Verá la siguiente información en el informe de validación:

    Disk <identifier> connected to node <nodename> returned a SCSI Port Association and the corresponding enclosure device could not be found. The hardware is not compatible with Storage Spaces Direct (S2D), contact the hardware vendor to verify support for SCSI Enclosure Services (SES). 


El problema es con la tarjeta de expansión de SAS de HPE que se encuentra entre los discos y la tarjeta HBA. El ampliador SAS crea un identificador duplicado entre la primera unidad conectada al expansor y el propio expansor.  Esto se ha resuelto en el [firmware del ampliador SAS del controlador de HPE Smart Array controllers: 4,02](https://support.hpe.com/hpsc/swd/public/detail?sp4ts.oid=7304566&swItemId=MTX_ef8d0bf4006542e194854eea6a&swEnvOid=4184#tab3).

## <a name="intel-ssd-dc-p4600-series-has-a-non-unique-nguid"></a>Intel SSD DC P4600 series tiene un NGUID no único
Es posible que aparezca un problema en el que un dispositivo de la serie P4600 de DC de Intel SSD está informando de un NGUID de 16 bytes similar para varios espacios de nombres como 0100000001000000E4D25C000014E214 o 0100000001000000E4D25C0000EEE214 en el ejemplo siguiente.


|               UniqueID               | ID | MediaType | BusType |               SerialNumber               |      size      | canpool | FriendlyName | OperationalStatus |
|--------------------------------------|----------|-----------|---------|------------------------------------------|----------------|---------|--------------|-------------------|
|           5000CCA251D12E30           |    0     |    HDD    |   SAS   |                 7PKR197G                 | 10000831348736 |  False  |     HGST     |  HUH721010AL4200  |
| EUI. 0100000001000000E4D25C000014E214 |    4     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_0014_E214. | 1600321314816  |  True   |    PROCESADOR     |   SSDPE2KE016T7   |
| EUI. 0100000001000000E4D25C000014E214 |    5     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_0014_E214. | 1600321314816  |  True   |    PROCESADOR     |   SSDPE2KE016T7   |
| EUI. 0100000001000000E4D25C0000EEE214 |    6     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_00EE_E214. | 1600321314816  |  True   |    PROCESADOR     |   SSDPE2KE016T7   |
| EUI. 0100000001000000E4D25C0000EEE214 |    7     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_00EE_E214. | 1600321314816  |  True   |    PROCESADOR     |   SSDPE2KE016T7   |

Para corregir este problema, actualice el firmware de las unidades de Intel a la versión más reciente.  Se sabe que la versión de firmware QDV101B1 de mayo de 2018 resuelve este problema.

La [versión 2018 de mayo de la herramienta centro de datos SSD de Intel](https://downloadmirror.intel.com/27778/eng/Intel_SSD_Data_Center_Tool_3_0_12_Release_Notes_330715-026.pdf) incluye una actualización de firmware, QDV101B1, para la serie P4600 de DC de Intel SSD.


## <a name="physical-disk-healthy-and-operational-status-is-removing-from-pool"></a>El disco físico "correcto" y el estado operativo es "quitando del grupo" 

En un clúster de Espacios de almacenamiento directo de Windows Server 2016, podría ver el HealthStatus de uno o varios discos físicos como "correcto", mientras que el OperationalStatus es "(quitando del grupo, correcto)". 

"Quitar del grupo" es un intento establecido cuando se llama a **Remove-PhysicalDisk** pero se almacena en estado para mantener el estado y permitir la recuperación si se produce un error en la operación de eliminación. Puede cambiar manualmente el OperationalStatus a correcto con uno de los métodos siguientes:

- Quite el disco físico del grupo y, a continuación, agréguelo de nuevo.
- Ejecute el [script Clear-PhysicalDiskHealthData. PS1](https://go.microsoft.com/fwlink/?linkid=2034205) para borrar el intento. (Disponible para su descarga como un. Archivo TXT. Tendrá que guardarlo como un. Archivo PS1 antes de poder ejecutarlo).

Estos son algunos ejemplos en los que se muestra cómo ejecutar el script:

- Use el parámetro **SerialNumber** para especificar el disco que necesita establecer en correcto. Puede obtener el número de serie de **WMI MSFT_PhysicalDisk** o **Get-PhysicalDisk**. (Vamos a usar solo 0s para el número de serie que aparece a continuación).

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -SerialNumber 000000000000000 -Verbose -Force
    ```

- Use el parámetro **UniqueID** para especificar el disco (de nuevo desde **MSFT_PhysicalDisk de WMI** o **Get-PhysicalDisk**).

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -UniqueId 00000000000000000 -Verbose -Force
   ```

## <a name="file-copy-is-slow"></a>La copia de archivos es lenta

Es posible que se haya producido un problema al usar el explorador de archivos para copiar un VHD grande en el disco virtual: la copia del archivo está tardando más de lo esperado.

Con el explorador de archivos, Robocopy o xcopy para copiar un VHD grande en el disco virtual no es un método recomendado para, ya que esto dará lugar a un rendimiento más lento del esperado. El proceso de copia no pasa por la pila de Espacios de almacenamiento directo, que se encuentra más abajo en la pila de almacenamiento y, en su lugar, actúa como un proceso de copia local.

Si desea probar el rendimiento de Espacios de almacenamiento directo, se recomienda usar VMFleet y Diskspd para cargar y realizar pruebas de esfuerzo de los servidores con el fin de obtener una línea base y establecer las expectativas del rendimiento del Espacios de almacenamiento directo.

## <a name="expected-events-that-you-would-see-on-rest-of-the-nodes-during-the-reboot-of-a-node"></a>Eventos esperados que se verán en el resto de los nodos durante el reinicio de un nodo. 

Es seguro omitir estos eventos: 

    Event ID 205: Windows lost communication with physical disk {XXXXXXXXXXXXXXXXXXXX }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

    Event ID 203: Windows lost communication with physical disk {xxxxxxxxxxxxxxxxxxxxxxxx }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

Si está ejecutando máquinas virtuales de Azure, puede omitir este evento:

    Event ID 32: The driver detected that the device \Device\Harddisk5\DR5 has its write cache enabled. Data corruption may occur. 

## <a name="slow-performance-or-lost-communication-io-error-detached-or-no-redundancy-errors-for-deployments-that-use-intel-p3x00-nvme-devices"></a>Rendimiento lento o "pérdida de comunicación", "error de e/s", "" desconectado "o" sin redundancia "para implementaciones que usan dispositivos Intel P3x00 NVMe

Hemos identificado un problema crítico que afecta a algunos Espacios de almacenamiento directo usuarios que usan Hardware basado en la familia Intel P3x00 de dispositivos NVM Express (NVMe) con versiones de firmware anteriores a la versión de mantenimiento 8. 

>[!NOTE]
> Los OEM individuales pueden tener dispositivos basados en la familia Intel P3x00 de dispositivos NVMe con cadenas de versión de firmware únicas. Póngase en contacto con su OEM para obtener más información sobre la versión más reciente del firmware.

Si usa hardware en su implementación en función de la familia Intel P3x00 de dispositivos NVMe, se recomienda que aplique inmediatamente el firmware disponible más reciente (por lo menos la versión de mantenimiento 8). En este [soporte técnico de Microsoft artículo](https://support.microsoft.com/help/4052341/slow-performance-or-lost-communication-io-error-detached-or-no-redunda) se proporciona información adicional acerca de este problema. 
