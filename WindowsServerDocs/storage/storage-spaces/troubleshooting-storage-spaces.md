---
title: Solucionar problemas de espacios directo almacenamiento
description: Obtenga información sobre cómo solucionar problemas de implementación de espacios de almacenamiento directo.
keywords: Espacios de almacenamiento
ms.prod: windows-server-threshold
ms.author: ''
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: 44bcf48f3e4a3b4b49ff027d3aa3e5704865e7b5
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447881"
---
# <a name="troubleshoot-storage-spaces-direct"></a>Solución de problemas de espacios de almacenamiento directo

> Se aplica a: Windows Server 2019, Windows Server 2016

Utilice la siguiente información para solucionar problemas de implementación de espacios de almacenamiento directo.

En general, comience con los pasos siguientes:

1. Confirme que la marca y modelo de SSD está certificado para Windows Server 2016 y Windows Server 2019 mediante el catálogo de Windows Server. Confirme con el proveedor que se admiten las unidades de disco para espacios de almacenamiento directo.
2. Inspeccionar el almacenamiento para cualquier unidad defectuosa. Usar software de administración de almacenamiento para comprobar el estado de las unidades de disco. Si alguna de las unidades de disco defectuoso, hable con su proveedor. 
3. Actualice el almacenamiento y firmware de la unidad si es necesario.
   Asegúrese de que las actualizaciones más recientes de Windows están instaladas en todos los nodos. Puede obtener las actualizaciones más recientes para Windows Server 2016 desde [historial de actualizaciones de Windows 10 y Windows Server 2016](https://aka.ms/update2016) y para Windows Server 2019 desde [historial de actualizaciones de Windows 10 y Windows Server 2019](https://support.microsoft.com/help/4464619).
4. Actualización de firmware y los controladores de adaptador de red.
5. Ejecute la validación de clúster y revise la sección de espacio de almacenamiento directo, asegúrese de que las unidades que se va a usar para la memoria caché se notifican correctamente y sin errores.

Si sigue teniendo problemas, revise los siguientes escenarios.

## <a name="virtual-disk-resources-are-in-no-redundancy-state"></a>Recursos de disco virtual están en estado de redundancia de n
Los nodos de un sistema de espacios de almacenamiento directo reiniciarse de forma inesperada debido a un error de alimentación o de bloqueo. A continuación, uno o varios de los discos virtuales no pueden ponerse en línea, y vea la descripción "no hay suficiente información de la redundancia".

|FriendlyName|ResiliencySettingName| OperationalStatus| HealthStatus| IsManualAttach|Tamaño| PSComputerName|
|------------|---------------------| -----------------| ------------| --------------|-----| --------------|
|DISK4| Reflejada| Aceptar|  Correcto| True|  10 TB|  Nodo-01.conto...|
|Disk3         |Reflejada                 |Aceptar                          |Correcto       |True            |10 TB | Nodo-01.conto...|
|Disk2         |Reflejada                 |No hay redundancia               |Incorrecto     |True            |10 TB | Nodo-01.conto...|
|Disco1         |Reflejada                 |{No hay redundancia, EnTaller}  |Incorrecto     |True            |10 TB | Nodo-01.conto...| 

Además, después de un intento de poner en línea el disco virtual, la siguiente información se registra en el registro de clúster (DiskRecoveryAction).  

```
[Verbose] 00002904.00001040::YYYY/MM/DD-12:03:44.891 INFO [RES] Physical Disk <DiskName>: OnlineThread: SuGetSpace returned 0.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 WARN [RES] Physical Disk < DiskName>: Underlying virtual disk is in 'no redundancy' state; its volume(s) may fail to mount.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 ERR [RES] Physical Disk <DiskName>: Failing online due to virtual disk in 'no redundancy' state. If you would like to attempt to online the disk anyway, first set this resource's private property 'DiskRecoveryAction' to 1. We will try to bring the disk online for recovery, but even if successful, its volume(s) or CSV may be unavailable. 
``` 

El **estado operativo de redundancia No** puede ocurrir si un disco no se pudo o si el sistema no puede acceder a los datos en el disco virtual. Este problema puede producirse si se produce un reinicio en un nodo durante el mantenimiento en los nodos.

Para corregir este problema, siga estos pasos:

1. Quite los discos virtuales afectados de CSV. Esto coloca en el grupo "Available storage" en el clúster y empezar a mostrar como un tipo de recurso de disco físico"."

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. En el nodo que posee el grupo de almacenamiento disponible, ejecute el siguiente comando en todos los discos que se encuentra en un estado No redundancia. Para identificar qué nodo es el grupo "Available Storage" en el puede ejecutar el comando siguiente.

   ```powershell
   Get-ClusterGroup
   ```
3. Establezca la acción de recuperación de disco y, a continuación, inicie los discos.
   ```powershell
   Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 1
   Start-ClusterResource -Name "VdiskName"
   ```
4. Una reparación debe iniciarse automáticamente. Espere a que la reparación Finalizar. Puede entrar en un estado suspendido y vuelva a iniciar. Para supervisar el progreso: 
    - Ejecute **Get-StorageJob** para supervisar el estado de la reparación y ver cuando el proceso finalice.
    - Ejecute **Get-VirtualDisk** y compruebe que el espacio devuelve HealthStatus correcto.
5. Después de la reparación finaliza y los discos virtuales son correcto, cambie los parámetros de disco Virtual de nuevo.

   ```powershell
    Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 0
   ```
6. Tome los discos sin conexión y, a continuación, en línea nuevo para que el DiskRecoveryAction surtan efecto:

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ``` 
7. Agregue los discos virtuales afectados a CSV.

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```

**DiskRecoveryAction** es un modificador de invalidación que permite adjuntar el volumen de espacio en el modo de lectura y escritura sin ninguna comprobación. La propiedad le permite realizar diagnósticos en ¿por qué no conectará un volumen. Es muy similar al modo de mantenimiento, pero se puede invocar en un recurso en un estado de error. También permite tener acceso a los datos, que pueden resultar útiles en situaciones como la "Redundancia n", donde puede obtener acceso a los datos puede y cópielo. La propiedad DiskRecoveryAction se agregó en el 22 de febrero de 2018, actualización, 4077525 KB.


## <a name="detached-status-in-a-cluster"></a>Estado desasociado en un clúster 

Al ejecutar el **Get-VirtualDisk** cmdlet, el OperationalStatus uno o varios espacios de almacenamiento directo se desasocia los discos virtuales. Sin embargo, el HealthStatus notificados por el **Get-PhysicalDisk** cmdlet indica que todos los discos físicos están en un estado correcto.

El siguiente es un ejemplo del resultado de la **Get-VirtualDisk** cmdlet.

|FriendlyName|  ResiliencySettingName|  OperationalStatus|   HealthStatus|  IsManualAttach|  Tamaño|   PSComputerName|
|-|-|-|-|-|-|-|
|DISK4|         Reflejada|                 Aceptar|                  Correcto|       True|            10 TB|  Nodo-01.conto...|
|Disk3|         Reflejada|                 Aceptar|                  Correcto|       True|            10 TB|  Nodo-01.conto...|
|Disk2|         Reflejada|                 Desasociado|            Unknown|       True|            10 TB|  Nodo-01.conto...|
|Disco1|         Reflejada|                 Desasociado|            Unknown|       True|            10 TB|  Nodo-01.conto...| 


Además, es posible que se registran los siguientes eventos en los nodos:

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

El **estado operativo desasociado** puede producirse si la región de datos sucia seguimiento (obsoletas DRT) el registro está lleno. Espacios de almacenamiento utiliza regiones obsoletas (DRT) de seguimiento para los espacios reflejados para asegurarse de que cuando se produce un error de alimentación, las actualizaciones en curso de los metadatos se registran para asegurarse de que el espacio de almacenamiento puede rehacer o deshacer operaciones para devolver el espacio de almacenamiento a una flexible y un estado coherente cuando se restaure la energía y el sistema vuelve a activarse. Si el registro DRT está lleno, el disco virtual no se puede poner en línea hasta que se sincronizan y se vacían los metadatos DRT. Este proceso requiere ejecutar un examen completo, que puede tardar varias horas en completarse.

Para corregir este problema, siga estos pasos:
1. Quite los discos virtuales afectados de CSV.

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. Ejecute los siguientes comandos en todos los discos que no entra en línea. 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 7
   Start-ClusterResource -Name "VdiskName"
   ``` 
3. Ejecute el siguiente comando en cada nodo en el que está en línea el volumen separado. 

   ```powershell
   Get-ScheduledTask -TaskName "Data Integrity Scan for Crash Recovery" | Start-ScheduledTask 
   ```
   Esta tarea se debe iniciar en todos los nodos en el que está en línea el volumen separado. Una reparación debe iniciarse automáticamente. Espere a que la reparación Finalizar. Puede entrar en un estado suspendido y vuelva a iniciar. Para supervisar el progreso: 
   - Ejecute **Get-StorageJob** para supervisar el estado de la reparación y ver cuando el proceso finalice.
   - Ejecute **Get-VirtualDisk** y compruebe el espacio devuelve HealthStatus correcto.
     - Los "datos integridad Scan para recuperación tras bloqueo" es una tarea que no se muestre como un trabajo de almacenamiento, y no hay ningún indicador de progreso. Si la tarea se muestra como en ejecución, se está ejecutando. Cuando se complete, se mostrará completada.

       Además, puede ver el estado de una tarea de programación en ejecución mediante el siguiente cmdlet: 
       ```powershell
       Get-ScheduledTask | ? State -eq running
       ``` 
4. Tan pronto como finalice la "datos integridad análisis para la recuperación tras bloqueo", finalice la reparación y los discos virtuales son correcto, cambie los parámetros de disco Virtual de nuevo. 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 0 
   ```
5. Agregue los discos virtuales afectados a CSV.    

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```  
   **Valor DiskRunChkdsk 7** se utiliza para adjuntar el volumen de espacio y tener la partición en modo de solo lectura. Esto permite espacios para detectar automáticamente y reparar automáticamente mediante el desencadenamiento de una reparación. La reparación se ejecutará automáticamente una vez montado. También permite acceder a los datos, que pueden ser útiles para obtener acceso a cualquier dato que se puede utilizar para copiar. Para algunas condiciones de error, como un registro lleno DRT, deberá ejecutar el análisis de integridad de datos para la tarea programada de recuperación tras bloqueo.

**Análisis de integridad de datos para la tarea de recuperación tras bloqueo** se utiliza para sincronizar y borrar el registro de seguimiento (DRT) completa de regiones. Esta tarea puede tardar varias horas en completarse. Los "datos integridad Scan para recuperación tras bloqueo" es una tarea que no se muestre como un trabajo de almacenamiento, y no hay ningún indicador de progreso. Si la tarea se muestra como en ejecución, se está ejecutando. Cuando se complete, se mostrará como completados. Si cancela la tarea o reiniciar un nodo mientras se ejecuta esta tarea, la tarea deberá empezar de nuevo desde el principio.

Para obtener más información, consulte [de estado de solución de problemas de espacios de almacenamiento directo y estados operativos](storage-spaces-states.md).

## <a name="event-5120-with-statusiotimeout-c00000b5"></a>Evento 5120 con STATUS_IO_TIMEOUT c00000b5 

> [!Important]
> **Para Windows Server 2016:** Para reducir la posibilidad de que experimentan estos síntomas al aplicar la actualización con la corrección, se recomienda utilizar el siguiente procedimiento de modo de mantenimiento de almacenamiento para instalar el [18 de octubre de 2018, la actualización acumulativa para Windows Server 2016](https://support.microsoft.com/help/4462928)o una versión posterior cuando los nodos actualmente han instalado una actualización acumulativa de Windows Server 2016 que se publicó desde [8 de mayo de 2018](https://support.microsoft.com/help/4103723) a [9 de octubre de 2018](https://support.microsoft.com/help/KB4462917).

Podría obtener eventos 5120 con STATUS_IO_TIMEOUT c00000b5 después de reiniciar un nodo en Windows Server 2016 con la actualización acumulativa que se publicaron desde [8 de mayo de 2018 KB 4103723](https://support.microsoft.com/help/4103723) a [9 de octubre de 2018 KB 4462917](https://support.microsoft.com/help/4462917)instalado.

Cuando se reinicia el nodo, 5120 de evento se registra en el registro de eventos del sistema e incluye uno de los códigos de error siguiente:

```
Event Source: Microsoft-Windows-FailoverClustering
Event ID: 5120
Description:    Cluster Shared Volume 'CSVName' ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_IO_TIMEOUT(c00000b5)'. All I/O will temporarily be queued until a path to the volume is reestablished. 

Cluster Shared Volume ‘CSVName’ ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_CONNECTION_DISCONNECTED(c000020c)'. All I/O will temporarily be queued until a path to the volume is reestablished.    
```

Cuando se registra un evento de 5120, se genera un volcado de memoria en vivo para recopilar información de depuración que puede tener un efecto de rendimiento o provocar síntomas adicionales. Generar el volcado de memoria en vivo, crea una breve pausa para habilitar la toma una instantánea de memoria para escribir el archivo de volcado. Los sistemas que tienen una gran cantidad de memoria y en situaciones de estrés pueden causar nodos retirarse pertenencia al clúster y provocar también el siguiente 1135 eventos se registrarán.

```
Event source: Microsoft-Windows-FailoverClustering
Event ID: 1135  
Description: Cluster node 'NODENAME'was removed from the active failover cluster membership. The Cluster service on this node may have stopped. This could also be due to the node having lost communication with other active nodes in the failover cluster. Run the Validate a Configuration wizard to check your network configuration. If the condition persists, check for hardware or software errors related to the network adapters on this node. Also check for failures in any other network components to which the node is connected such as hubs, switches, or bridges.
```

Un cambio introducido en 8 de mayo de 2018 a Windows Server 2016, lo que era una actualización acumulativa para agregar controla resistente de SMB para las sesiones de red SMB de espacios de almacenamiento directo dentro del clúster. Esto se hace para mejorar la resistencia a errores de red transitorios y mejorar cómo controla la congestión de la red RoCE. Estas mejoras también accidentalmente aumentan los tiempos de espera cuando las conexiones SMB intentan volver a conectarse y espera hasta el tiempo de espera cuando se reinicia un nodo. Estos problemas pueden afectar a un sistema que está bajo presión. Durante el tiempo de inactividad imprevisto, también se han observado pausas de E/S de hasta 60 segundos mientras espera a que el sistema para las conexiones con el tiempo de espera. Para corregir este problema, instale el [18 de octubre de 2018, la actualización acumulativa para Windows Server 2016](https://support.microsoft.com/help/4462928) o una versión posterior.

*Tenga en cuenta* esta actualización alinea los tiempos de espera CSV con tiempos de espera de conexión de SMB para corregir este problema. No implementa los cambios para deshabilitar la generación de volcado de memoria en vivo se menciona en la sección solución alternativa.

### <a name="shutdown-process-flow"></a>Flujo del proceso de apagado:

1. Ejecute el cmdlet Get-VirtualDisk y asegúrese de que el valor de HealthStatus es correcto.
2. Purga el nodo, ejecute el siguiente cmdlet:

   ```powershell
   Suspend-ClusterNode -Drain
   ```
3. Colocar los discos en ese nodo en modo de mantenimiento de almacenamiento, ejecute el siguiente cmdlet: 

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Enable-StorageMaintenanceMode
   ```
4. Ejecute el **Get-PhysicalDisk** cmdlet y asegúrese de que el valor OperationalStatus está en modo de mantenimiento.
5. Ejecute el **Restart-Computer** cmdlet para reiniciar el nodo.
6. Cuando se reinicia un nodo, quite los discos en ese nodo desde el modo de mantenimiento de almacenamiento, ejecute el siguiente cmdlet:

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Disable-StorageMaintenanceMode
   ```
7. Reanudar el nodo, ejecute el siguiente cmdlet:

   ```powershell
   Resume-ClusterNode
   ```
8. Compruebe el estado de los trabajos de resincronización, ejecute el siguiente cmdlet:

   ```powershell
   Get-StorageJob
   ```

### <a name="disabling-live-dumps"></a>Deshabilitación de volcados de memoria en vivo
Para mitigar el efecto de generación de volcado de memoria en vivo en los sistemas que tienen una gran cantidad de memoria y en situaciones de estrés, además es posible que desee deshabilitar la generación de volcado de memoria en vivo. A continuación se proporcionan tres opciones.

>[!Caution]
>Este procedimiento puede impedir la recopilación de información de diagnóstico que puede necesitar Microsoft Support para investigar este problema. Puede tener un agente de soporte técnico solicitar que vuelva a habilitar la generación de volcado de memoria en vivo basada en escenarios de solución de problemas específicos.

Hay dos métodos para deshabilitar los volcados de memoria en vivo, como se describe a continuación.

#### <a name="method-1-recommended-in-this-scenario"></a>Método 1 (recomendado en este escenario)
Para deshabilitar completamente todos los volcados de memoria, incluidos los volcados de memoria en vivo todo el sistema, siga estos pasos:

1. Cree la siguiente clave del registro: HKLM\System\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled
2. En la nueva **ForceDumpsDisabled** clave, cree una propiedad REG_DWORD como GuardedHost y, a continuación, establezca su valor en 0 x 10000000.
3. Aplicar la nueva clave del registro para cada nodo del clúster.

>[!NOTE]
>Tendrá que reiniciar el equipo para que surta efecto el cambio de NLA.

Después de esta clave del registro está establecido, en vivo creación del volcado de memoria se producirá un error y generar un error de "STATUS_NOT_SUPPORTED".

#### <a name="method-2"></a>Método 2
De forma predeterminada, Windows Error Reporting permitirá solo LiveDump por tipo de informe y 7 días y solo 1 LiveDump por máquina por 5 días. Se puede cambiar estableciendo las siguientes claves del registro para permitir únicamente uno LiveDump en la máquina para siempre.
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v SystemThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v ComponentThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```

*Tenga en cuenta* tendrá que reiniciar el equipo para que el cambio surta efecto.

### <a name="method-3"></a>Método 3
Para deshabilitar la generación de clúster de volcados de memoria en vivo (por ejemplo, cuando se registra un evento de 5120), ejecute el siguiente cmdlet:

```powershell
(Get-Cluster).DumpPolicy = ((Get-Cluster).DumpPolicy -band 0xFFFFFFFFFFFFFFFE)
```
Este cmdlet tiene un efecto inmediato en todos los nodos del clúster sin reiniciar el equipo.

## <a name="slow-io-performance"></a>Rendimiento lento de E/S

Si observa un rendimiento de E/S lento, compruebe si la memoria caché está habilitada en la configuración de espacios de almacenamiento directo. 

Hay dos maneras de comprobar: 


1. Con el registro de clúster. Abra el registro de clúster en el editor de texto preferido y busque "[=== SBL discos ===]." Se trata de una lista del disco en el nodo que se generó el registro en. 

     Caché habilitada discos ejemplo: Tenga en cuenta aquí que el estado es CacheDiskStateInitializedAndBound y hay un GUID que se encuentra aquí. 

   ```
   [=== SBL Disks ===]
    {26e2e40f-a243-1196-49e3-8522f987df76},3,false,true,1,48,{1ff348f1-d10d-7a1a-d781-4734f4440481},CacheDiskStateInitializedAndBound,1,8087,54,false,false,HGST    ,HUH721010AL4200 ,        7PG3N2ER,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Memoria caché no está habilitado: Aquí podemos ver ningún GUID está presente y el estado es CacheDiskStateNonHybrid. 
    ```
   [=== SBL Disks ===]
    {426f7f04-e975-fc9d-28fd-72a32f811b7d},12,false,true,1,24,{00000000-0000-0000-0000-000000000000},CacheDiskStateNonHybrid,0,0,0,false,false,HGST    ,HUH721010AL4200 ,        7PGXXG6C,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Memoria caché no está habilitado: Cuando todos los discos son del mismo caso de tipo no está habilitado de forma predeterminada. Aquí podemos ver ningún GUID está presente y el estado es CacheDiskStateIneligibleDataPartition. 
    ```
    {d543f90c-798b-d2fe-7f0a-cb226c77eeed},10,false,false,1,20,{00000000-0000-0000-0000-000000000000},CacheDiskStateIneligibleDataPartition,0,0,0,false,false,NVMe    ,INTEL SSDPE7KX02,  PHLF7330004V2P0LGN,0170,{79b4d631-976f-4c94-a783-df950389fd38},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0], 
    ```  
2. Uso de Get-PhysicalDisk.xml desde el SDDCDiagnosticInfo
    1. Abra el archivo XML con "$d = GetPhysicalDisk.XML Import-Clixml"
    2. Ejecute "ipmo almacenamiento"
    3. Ejecute "$d". Tenga en cuenta que uso es la selección automática, no diario, verá un resultado similar al siguiente: 

   |FriendlyName|  SerialNumber| MediaType| CanPool| OperationalStatus| HealthStatus| Uso| Tamaño|
   |-----------|------------|---------| -------| -----------------| ------------| -----| ----|
   |NVMe INTEL SSDPE7KX02| PHLF733000372P0LGN| SSD| False|   Aceptar|                Correcto|      Selección automática 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504008J2P0LGN| SSD|  False|    Aceptar|                Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504005F2P0LGN| SSD|  False|  Aceptar|                Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002A2P0LGN| SSD| False| Aceptar|    Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504004T2P0LGN |SSD| False|Aceptar|       Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002E2P0LGN| SSD| False| Aceptar|      Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330002Z2P0LGN| SSD| False| Aceptar|      Correcto|Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF733000272P0LGN |SSD| False| Aceptar|  Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330001J2P0LGN |SSD| False| Aceptar| Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF733000302P0LGN |SSD| False| Aceptar|Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7330004D2P0LGN |SSD| False| Aceptar| Correcto| Selección automática |1,82 TB|

## <a name="how-to-destroy-an-existing-cluster-so-you-can-use-the-same-disks-again"></a>Cómo destruir un clúster existente para que pueda usar los mismos discos nuevo

En un clúster de espacios de almacenamiento directo, una vez que se deshabilite espacios de almacenamiento directo y usar el proceso de limpieza se describe en [limpiar unidades](deploy-storage-spaces-direct.md#step-31-clean-drives), sigue siendo el bloque de almacenamiento en clúster en un estado sin conexión y se quitará el servicio de mantenimiento clúster.

El siguiente paso es quitar el grupo de almacenamiento ficticia: 
   ```powershell
   Get-ClusterResource -Name "Cluster Pool 1" | Remove-ClusterResource
   ```

Ahora, si ejecuta **Get-PhysicalDisk** en cualquiera de los nodos, verá todos los discos que se encontraban en el grupo. Por ejemplo, en un laboratorio con un clúster de 4 nodos con 4 discos SAS, de 100 GB cada uno presentada a cada nodo. En ese caso, después de espacio de almacenamiento directo está deshabilitado, que quita el SBL (capa de Bus de almacenamiento), pero deja el filtro, si ejecuta **Get-PhysicalDisk**, debe indicar que hay 4 discos, excepto el disco del sistema operativo local. En su lugar notifica 16 en su lugar. Este es el mismo para todos los nodos del clúster. Al ejecutar un **Get-Disk** comando, verá los discos conectados localmente numerados como 0, 1, 2 y así sucesivamente, tal como se muestra en esta salida de ejemplo:

|Número| Nombre descriptivo| Número de serie|HealthStatus|OperationalStatus|Tamaño total| Estilo de partición|
|-|-|-|-|-|-|-|-|
|0|Msft Virtu...  ||Correcto | Online|  127 GB| GPT|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| SIN PROCESAR|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| SIN PROCESAR|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| SIN PROCESAR|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| SIN PROCESAR|
|1|Msft Virtu...||Correcto| Desconectado| 100 GB| SIN PROCESAR|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| SIN PROCESAR|
|2|Msft Virtu...||Correcto| Desconectado| 100 GB| SIN PROCESAR|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| SIN PROCESAR|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| SIN PROCESAR|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| SIN PROCESAR|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| SIN PROCESAR|
|4|Msft Virtu...||Correcto| Desconectado| 100 GB| SIN PROCESAR|
|3|Msft Virtu...||Correcto| Desconectado| 100 GB| SIN PROCESAR|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| SIN PROCESAR|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| SIN PROCESAR|
||Msft Virtu... ||Correcto| Desconectado| 100 GB| SIN PROCESAR|


## <a name="error-message-about-unsupported-media-type-when-you-create-an-storage-spaces-direct-cluster-using-enable-clusters2d"></a>Mensaje de error sobre "tipo de medio no compatible" al crear un clúster de espacios de almacenamiento directo con Enable-ClusterS2D  

Es posible que vea errores similares al siguiente al ejecutar el **Enable-ClusterS2D** cmdlet:

![Mensaje de error de escenario 6](media/troubleshooting/scenario-error-message.png)

Para corregir este problema, asegúrese de que el adaptador HBA está configurado en modo HBA. Ningún HBA debe configurarse en modo RAID.  

## <a name="enable-clusterstoragespacesdirect-hangs-at-waiting-until-sbl-disks-are-surfaced-or-at-27"></a>Enable-ClusterStorageSpacesDirect se bloquea en 'Esperar a que SBL discos aparecen' o en 27%

Verá la siguiente información en el informe de validación:

    Disk <identifier> connected to node <nodename> returned a SCSI Port Association and the corresponding enclosure device could not be found. The hardware is not compatible with Storage Spaces Direct (S2D), contact the hardware vendor to verify support for SCSI Enclosure Services (SES). 


El problema está relacionado con la tarjeta de expansión HPE SAS que se encuentra entre los discos y la tarjeta HBA. El Ampliador SAS, crea un Id. duplicado entre la primera unidad conectada en el botón de expansión y el botón de expansión.  Esto se ha resuelto en [HPE inteligente matriz controladores SAS expansor Firmware: 4.02](https://support.hpe.com/hpsc/swd/public/detail?sp4ts.oid=7304566&swItemId=MTX_ef8d0bf4006542e194854eea6a&swEnvOid=4184#tab3).

## <a name="intel-ssd-dc-p4600-series-has-a-non-unique-nguid"></a>Serie de Intel SSD DC P4600 tiene un NGUID no único
Es posible que vea un problema donde un dispositivo de la serie Intel SSD DC P4600 parece ser reporting similar 16 bytes NGUID para varios espacios de nombres como 0100000001000000E4D25C000014E214 o 0100000001000000E4D25C0000EEE214 en el ejemplo siguiente.


|               Identificador único               | deviceid | MediaType | BusType |               serialnumber               |      size      | canpool | FriendlyName | OperationalStatus |
|--------------------------------------|----------|-----------|---------|------------------------------------------|----------------|---------|--------------|-------------------|
|           5000CCA251D12E30           |    0     |    HDD    |   SAS   |                 7PKR197G                 | 10000831348736 |  False  |     HGST     |  HUH721010AL4200  |
| eui.0100000001000000E4D25C000014E214 |    4     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_0014_E214. | 1600321314816  |  True   |    INTEL     |   SSDPE2KE016T7   |
| eui.0100000001000000E4D25C000014E214 |    5     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_0014_E214. | 1600321314816  |  True   |    INTEL     |   SSDPE2KE016T7   |
| eui.0100000001000000E4D25C0000EEE214 |    6     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_00EE_E214. | 1600321314816  |  True   |    INTEL     |   SSDPE2KE016T7   |
| eui.0100000001000000E4D25C0000EEE214 |    7     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_00EE_E214. | 1600321314816  |  True   |    INTEL     |   SSDPE2KE016T7   |

Para corregir este problema, actualice el firmware en las unidades de Intel para la versión más reciente.  Versión de firmware QDV101B1 desde mayo de 2018 se conoce para resolver este problema.

El [versión de mayo de 2018 de la herramienta de centro de datos de Intel SSD](https://downloadmirror.intel.com/27778/eng/Intel_SSD_Data_Center_Tool_3_0_12_Release_Notes_330715-026.pdf) incluye una actualización de firmware, QDV101B1, para la serie de Intel SSD DC P4600.


## <a name="physical-disk-healthy-and-operational-status-is-removing-from-pool"></a>"Correcto" disco físico y estado operativo es "Quitar de grupo" 

En un clúster de espacios de almacenamiento de Windows Server 2016 directo, es posible que vea el HealthStatus de discos físicos de más de un como "Correcto", mientras el OperationalStatus es "(quitar del grupo, Aceptar)." 

"Quitar de grupo" es un intento de establecer cuando **Remove-PhysicalDisk** se llama pero almacenados en el estado para mantener el estado y permitir la recuperación si se produce un error en la operación de eliminación. Puede cambiar manualmente el OperationalStatus a correcto con uno de los métodos siguientes:

- Quite el disco físico del grupo y, a continuación, vuelva a agregarlo.
- Ejecute el [Clear PhysicalDiskHealthData.ps1 script](https://go.microsoft.com/fwlink/?linkid=2034205) para borrar la intención. (Disponible para su descarga como un. Archivo TXT. Tendrá que guardarlo como un. Archivo PS1 antes de que se pueda ejecutar.)

Estos son algunos ejemplos que muestran cómo ejecutar la secuencia de comandos:

- Use la **SerialNumber** parámetro para especificar el disco que necesite establecer al estado correcto. Puede obtener el número de serie **WMI MSFT_PhysicalDisk** o **Get-PhysicalDIsk**. (Simplemente usamos 0s para el número de serie.)

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -SerialNumber 000000000000000 -Verbose -Force
    ```

- Use la **UniqueId** parámetro para especificar el disco (nuevo desde **WMI MSFT_PhysicalDisk** o **Get-PhysicalDIsk**).

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -UniqueId 00000000000000000 -Verbose -Force
   ```

## <a name="file-copy-is-slow"></a>Copia de archivos es lenta

Es posible que visto un problema con el Explorador de archivos para copiar un VHD de gran tamaño en el disco virtual: la copia de archivos está tardando más de lo esperado.

Mediante el Explorador de archivos, Robocopy o Xcopy para copiar un VHD de gran tamaño en el disco virtual no es un método recomendado para que esto dará como resultado más lento que el rendimiento esperado. El proceso de copia no pasa por la pila de espacios de almacenamiento directo, que se ubica inferior en la pila de almacenamiento y, en su lugar, actúa como un proceso de copia local.

Si desea probar el rendimiento de espacios de almacenamiento directo, se recomienda usar VMFleet y Diskspd para carga y prueba de esfuerzo de los servidores para obtener una línea de base y establecer las expectativas del rendimiento de espacios de almacenamiento directo.

## <a name="expected-events-that-you-would-see-on-rest-of-the-nodes-during-the-reboot-of-a-node"></a>Espera que los eventos que vería en el resto de los nodos durante el reinicio de un nodo. 

Es seguro omitir estos eventos: 

    Event ID 205: Windows lost communication with physical disk {XXXXXXXXXXXXXXXXXXXX }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

    Event ID 203: Windows lost communication with physical disk {xxxxxxxxxxxxxxxxxxxxxxxx }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

Si ejecutas máquinas virtuales de Azure, puede omitir este evento:

    Event ID 32: The driver detected that the device \Device\Harddisk5\DR5 has its write cache enabled. Data corruption may occur. 

## <a name="slow-performance-or-lost-communication-io-error-detached-or-no-redundancy-errors-for-deployments-that-use-intel-p3x00-nvme-devices"></a>Rendimiento lento o "Pérdida de comunicación," "Error de E/S", "Desconectado", o errores de "Redundancia del n" para las implementaciones que usan dispositivos NVMe P3x00 de Intel

Hemos identificado un problema crítico que afecta a algunos espacios de almacenamiento directo a los usuarios que usen el hardware basado en la familia Intel P3x00 de dispositivos NVM Express (NVMe) con las versiones de firmware antes de "Mantenimiento versión 8." 

>[!NOTE]
> Los OEM individual pueden tener los dispositivos que se basan en la familia Intel P3x00 de dispositivos NVMe con cadenas de versión de firmware único. Para obtener más información de la última versión de firmware, póngase en contacto con su OEM.

Si utiliza hardware de su implementación en función de la familia Intel P3x00 de dispositivos NVMe, se recomienda aplicar inmediatamente el firmware más reciente disponible (al menos 8 de la versión de mantenimiento). Esto [artículo de Microsoft Support](https://support.microsoft.com/help/4052341/slow-performance-or-lost-communication-io-error-detached-or-no-redunda) proporciona información adicional sobre este problema. 
