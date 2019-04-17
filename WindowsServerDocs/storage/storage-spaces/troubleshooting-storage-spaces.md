---
title: Solucionar problemas de espacios de almacenamiento
description: Obtén información sobre cómo solucionar problemas de la implementación de espacios de almacenamiento directo.
keywords: Espacios de almacenamiento
ms.prod: windows-server-threshold
ms.author: ''
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: c7c9573732ff1cf5a998588b1aec81915c227ee2
ms.sourcegitcommit: 5d55c3ebd1ceee7229ea9a9989c69cf93757ed88
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "9256938"
---
# Solucionar problemas de espacios de almacenamiento directo

Usa la siguiente información para solucionar problemas de la implementación de espacios de almacenamiento directo.

En general, se inicia con los siguientes pasos:

1. Confirma que la marca y modelo de SSD está certificada para Windows Server 2016 mediante el catálogo de Windows Server. Confirma que con el proveedor que son compatibles con las unidades de espacios de almacenamiento directo.
2. Inspeccionar el almacenamiento de las unidades defectuosos. Usar software de administración de almacenamiento para comprobar el estado de las unidades. Si cualquiera de las unidades son incorrectas, funcionan con tu proveedor. 
3. Actualización de almacenamiento y firmware de la unidad si es necesario.
   Asegúrate de que se instalen las actualizaciones de Windows más recientes en todos los nodos. Puedes obtener las últimas actualizaciones de Windows Server 2016 desde [https://aka.ms/update2016](https://aka.ms/update2016).
4. Actualizar controladores de adaptador de red y el firmware.
5. Ejecutar la validación de clúster y revisa la sección de espacio de almacenamiento directo, asegúrate de que las unidades que se usan para la memoria caché se notifican correctamente y sin errores.

Si aún tienes problemas, revisa los escenarios siguientes.

## Recursos del disco virtual que se encuentran en el estado de redundancia n
Los nodos de un sistema de espacios de almacenamiento directo se reinicien inesperadamente debido a un error de alimentación o bloquearse. A continuación, uno o varios de los discos virtuales no aparezcan en línea y ver la descripción "no hay suficiente información de redundancia".
    
|FriendlyName|ResiliencySettingName| OperationalStatus| HealthStatus| IsManualAttach|Tamaño| PSComputerName|
|------------|---------------------| -----------------| ------------| --------------|-----| --------------|
|DISK4| Mirror| OK|  Correcto| Verdadero|  10 TB|  Nodo-01.conto …|
|Disco 3         |Mirror                 |OK                          |Correcto       |Verdadero            |10 TB | Nodo-01.conto …|
|Disco 2         |Mirror                 |No hay redundancia               |Incorrecto     |Verdadero            |10 TB | Nodo-01.conto …|
|Disco 1         |Mirror                 |{Sin redundancia, EnTaller}  |Incorrecto     |Verdadero            |10 TB | Nodo-01.conto …| 

Además, después de un intento de conectar el disco virtual, se registra la siguiente información en el registro de clúster (DiskRecoveryAction).  

```
[Verbose] 00002904.00001040::YYYY/MM/DD-12:03:44.891 INFO [RES] Physical Disk <DiskName>: OnlineThread: SuGetSpace returned 0.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 WARN [RES] Physical Disk < DiskName>: Underlying virtual disk is in 'no redundancy' state; its volume(s) may fail to mount.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 ERR [RES] Physical Disk <DiskName>: Failing online due to virtual disk in 'no redundancy' state. If you would like to attempt to online the disk anyway, first set this resource's private property 'DiskRecoveryAction' to 1. We will try to bring the disk online for recovery, but even if successful, its volume(s) or CSV may be unavailable. 
``` 

El **Estado operativo de redundancia No** puede producirse si un disco dañado o si el sistema no puede acceder a datos en el disco virtual. Este problema puede ocurrir si se produce un reinicio en un nodo durante el mantenimiento en los nodos.
    
Para corregir este problema, sigue estos pasos:

1. Quitar los discos virtuales afectados de CSV. Esto se colocarlos en el grupo de "Almacenamiento disponible" en el clúster y se iniciará mostrando como un tipo de recurso de "Disco físico".

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. En el nodo propietario del grupo de almacenamiento disponible, ejecute el siguiente comando en todos los discos que se encuentra en un estado No redundancia. Para identificar qué nodo el grupo de "Almacenamiento disponible" está en el ejecutar el comando siguiente.

   ```powershell
   Get-ClusterGroup
   ```
3. Establece la acción de recuperación de disco y, a continuación, inicia los discos.
   ```powershell
   Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 1
   Start-ClusterResource -Name "VdiskName"
   ```
4. Una reparación debe iniciarse automáticamente. Esperar a que la reparación de finalizar. Puede entrar en un estado suspendido y empezar de nuevo. Para supervisar el progreso: 
    - Ejecuta **Get-StorageJob** para supervisar el estado de la reparación y para ver cuando se complete.
    - Ejecuta **Get-VirtualDisk** y comprueba que el espacio devuelve un valor de buen estado.
5. Después de la reparación finaliza y los discos virtuales son correcto, los parámetros de disco Virtual volver a cambiar.

   ```powershell
    Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 0
   ```
6. Tomar los discos sin conexión y, a continuación, en línea para que la DiskRecoveryAction surta efecto:

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ``` 
7. Agrega los discos virtuales afectados a CSV.

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```

**DiskRecoveryAction** es un conmutador de invalidación que permite asociar el volumen de espacio en modo de lectura y escritura sin ninguna comprobación. La propiedad te permite hacer diagnósticos en por qué no conectará un volumen. Es muy similar al modo de mantenimiento, pero se puede invocar en un recurso en un estado de error. También te permite acceder a los datos, que pueden ser útiles en situaciones como "Redundancia n", donde puedes obtener acceso a cualquier dato puede y copiarlo. La propiedad DiskRecoveryAction se agregó en la 22 de febrero de 2018, actualización, 4077525 KB.
    
    
## Estado desconectado en un clúster 

Cuando se ejecuta el cmdlet **Get-VirtualDisk** , se desconecta la OperationalStatus para uno o varios discos virtuales espacios de almacenamiento directo. Sin embargo, el valor notificado por el cmdlet **Get-PhysicalDisk** indica que todos los discos físicos en buen estado.

El siguiente es un ejemplo de la salida del cmdlet **Get-VirtualDisk** .

|FriendlyName|  ResiliencySettingName|  OperationalStatus|   HealthStatus|  IsManualAttach|  Tamaño|   PSComputerName|
|-|-|-|-|-|-|-|
|DISK4|         Mirror|                 OK|                  Correcto|       Verdadero|            10 TB|  Nodo-01.conto …|
|Disco 3|         Mirror|                 OK|                  Correcto|       Verdadero|            10 TB|  Nodo-01.conto …|
|Disco 2|         Mirror|                 Oculto|            Unknown|       Verdadero|            10 TB|  Nodo-01.conto …|
|Disco 1|         Mirror|                 Oculto|            Unknown|       Verdadero|            10 TB|  Nodo-01.conto …| 


Además, los siguientes eventos se pueden grabar en los nodos:

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

El **Estado operativo desconectado** puede producirse si la región obsoleta seguimiento (DRT) el registro está lleno. Espacios de almacenamiento utiliza región obsoleta (DRT) de seguimiento para espacios de reflejadas para asegurarse de que, cuando se produce un error de alimentación, las actualizaciones en curso a los metadatos se registran para asegurarte de que el espacio de almacenamiento puede rehacer o deshacer las operaciones para poner el espacio de almacenamiento en un flexible y el estado coherente cuando se restaura la energía y el sistema vuelve a activarse. Si se completa el registro DRT, el disco virtual no se puede poner en línea hasta que se sincronizan y se vacían los metadatos DRT. Este proceso requiere que se ejecute un examen completo, lo que puede tardar varias horas en Finalizar.
    
Para corregir este problema, sigue estos pasos:
1. Quitar los discos virtuales afectados de CSV.

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. Ejecute los siguientes comandos en todos los discos que no estará disponible en línea. 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 7
   Start-ClusterResource -Name "VdiskName"
   ``` 
3. Ejecuta el siguiente comando en cada nodo en el que está conectado el volumen desconectado. 

   ```powershell
   Get-ScheduledTask -TaskName "Data Integrity Scan for Crash Recovery" | Start-ScheduledTask 
   ```
   Esta tarea debe iniciarse en todos los nodos en el que está conectado el volumen desconectado. Una reparación debe iniciarse automáticamente. Esperar a que la reparación de finalizar. Puede entrar en un estado suspendido y empezar de nuevo. Para supervisar el progreso: 
   - Ejecuta **Get-StorageJob** para supervisar el estado de la reparación y para ver cuando se complete.
   -  Ejecuta **Get-VirtualDisk** y comprueba que el espacio devuelve un valor de buen estado.
    - El "integridad Examinar para bloquear recuperación de datos" es una tarea que no se muestra como un trabajo de almacenamiento y no hay ningún indicador de progreso. Si se muestra la tarea de ejecución, se está ejecutando. Cuando se completa, se mostrarán completada.
    
       Además, puedes ver el estado de una tarea de programación de ejecución mediante el cmdlet siguiente: 
       ```powershell
       Get-ScheduledTask | ? State -eq running
       ``` 
4. Tan pronto como el "integridad Examinar para bloquear recuperación de datos" finalice, los termina de reparación y los discos virtuales son correcto, los parámetros de disco Virtual volver a cambiar. 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 0 
   ```
5. Agrega los discos virtuales afectados a CSV.    

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```  
**Valor DiskRunChkdsk 7** se usa para asociar el volumen de espacio y tener la partición en modo de solo lectura. Esto permite espacios para detectar automáticamente y repararse a sí mismo al desencadenar una reparación. Se ejecutará reparar automáticamente una vez montados. También permite tener acceso a los datos, que pueden resultar útiles obtener acceso a cualquier puede para copiar los datos. Para algunas condiciones de error, como un registro DRT completo, debes ejecutar el análisis de la integridad de datos para la tarea programada en la recuperación de bloqueo.
    
**Análisis de datos integridad de tarea de recuperación de bloqueo** se usa para sincronizar y borrar un registro de seguimiento (DRT) de la región obsoleta completo. Esta tarea puede tardar varias horas en completarse. El "integridad Examinar para bloquear recuperación de datos" es una tarea que no se muestra como un trabajo de almacenamiento y no hay ningún indicador de progreso. Si se muestra la tarea de ejecución, se está ejecutando. Cuando se completa, se mostrará como completado. Si se cancela la tarea o reinicia un nodo mientras se está ejecutando esta tarea, la tarea que empezar desde el principio.

Para obtener más información, consulta la [solución de problemas de espacios de almacenamiento directo de mantenimiento y estados operativos](storage-spaces-states.md).
    
## Evento 5120 con STATUS_IO_TIMEOUT c00000b5 

> [!Important]
> Para reducir la probabilidad de que experimenten estos síntomas al aplicar la actualización con la corrección, se recomienda usar el siguiente procedimiento de modo de mantenimiento de almacenamiento para instalar la [actualización acumulativa de 18 de octubre de 2018 de Windows Server 2016](https://support.microsoft.com/help/4462928) o una versión posterior Cuando los nodos actualmente tener instalada una actualización acumulativa de Windows Server 2016 que se publicó desde [el 8 de mayo de 2018](https://support.microsoft.com/help/4103723) a [9 de octubre de 2018](https://support.microsoft.com/help/KB4462917).

Es posible que obtengas evento 5120 con STATUS_IO_TIMEOUT c00000b5 después de reiniciar un nodo en Windows Server 2016 con la actualización acumulativa que se publicaron desde [puede 8 KB 2018 4103723](https://support.microsoft.com/help/4103723) a [9 de octubre de 2018 KB 4462917](https://support.microsoft.com/help/4462917) instalado.

Cuando se reinicia el nodo, 5120 de eventos se registran en el registro de eventos del sistema e incluye uno de los siguientes códigos de error:

```
Event Source: Microsoft-Windows-FailoverClustering
Event ID: 5120
Description:    Cluster Shared Volume 'CSVName' ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_IO_TIMEOUT(c00000b5)'. All I/O will temporarily be queued until a path to the volume is reestablished. 

Cluster Shared Volume ‘CSVName’ ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_CONNECTION_DISCONNECTED(c000020c)'. All I/O will temporarily be queued until a path to the volume is reestablished.    
```

Cuando se registra un evento de 5120, se genera un volcado de memoria dinámica para recopilar información de depuración que puede tener un efecto de rendimiento o provocar síntomas adicionales. Generar el volcado de memoria dinámica, crea una breve pausa para habilitar la toma una instantánea de memoria para escribir en el archivo de volcado de memoria. Los sistemas que tienen una gran cantidad de memoria y en condiciones de carga pueden provocar nodos desaparecerán de pertenencia al clúster y provocará la siguiente 1135 de evento se registrara.

```
Event source: Microsoft-Windows-FailoverClustering
Event ID: 1135  
Description: Cluster node 'NODENAME'was removed from the active failover cluster membership. The Cluster service on this node may have stopped. This could also be due to the node having lost communication with other active nodes in the failover cluster. Run the Validate a Configuration wizard to check your network configuration. If the condition persists, check for hardware or software errors related to the network adapters on this node. Also check for failures in any other network components to which the node is connected such as hubs, switches, or bridges.
```

Se introdujo un cambio en la actualización acumulativa de 8 de mayo de 2018, agregar controla resistente de SMB para las sesiones de red SMB de espacios de almacenamiento directo dentro del clúster. Esto se hace para mejorar la resistencia a errores de red transitorio y mejorar cómo controla la congestión de red RoCE.

Estas mejoras también accidentalmente aumentan espera tiempo de espera y tiempos de espera cuando conexiones SMB intenta volver a conectar cuando se reinicia un nodo. Estos problemas pueden afectar a un sistema que esté en condiciones de carga. Durante el tiempo de inactividad imprevista, también se ha observado pausas de E/S de hasta 60 segundos mientras espera a que el sistema para las conexiones con el tiempo de espera.

Para corregir este problema, instale el [18 de octubre de 2018, de actualización acumulativa para Windows Server 2016](https://support.microsoft.com/help/4462928) o una versión posterior.

*Nota* Esta actualización alinea los tiempos de espera CSV con tiempos de espera de conexión de SMB para corregir este problema. No implementa los cambios para deshabilitar la generación de volcado de memoria dinámica mencionada en la sección de solución alternativa.
    
### Flujo del proceso de apagado:

1. Ejecuta el cmdlet Get-VirtualDisk y asegúrate de que el valor de HealthStatus es correcto.
2. Purgar el nodo ejecutando el siguiente cmdlet:

   ```powershell
   Suspend-ClusterNode -Drain
   ```
3. Coloca los discos en ese nodo en modo de mantenimiento de almacenamiento, ejecuta el siguiente cmdlet: 

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Enable-StorageMaintenanceMode
   ```
4. Ejecuta el cmdlet **Get-PhysicalDisk** y asegúrate de que el valor de OperationalStatus se encuentra en el modo de mantenimiento.
5. Ejecuta el cmdlet **Restart-Computer** para reiniciar el nodo.
6. Una vez reiniciado el nodo, quitar los discos en ese nodo desde el modo de mantenimiento de almacenamiento, ejecuta el siguiente cmdlet:

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Disable-StorageMaintenanceMode
   ```
7. Reanudar el nodo ejecutando el siguiente cmdlet:

   ```powershell
   Resume-ClusterNode
   ```
8. Comprobar el estado de los trabajos de resincronización ejecutando el siguiente cmdlet:

   ```powershell
   Get-StorageJob
   ```

### Deshabilitar volcados de memoria dinámicas
Para mitigar el efecto de la generación de volcado de memoria dinámica en sistemas que tienen una gran cantidad de memoria y en condiciones de carga, además puedes deshabilitar la generación de volcado de memoria dinámica. A continuación se proporcionan las tres opciones.

>[!Caution]
>Este procedimiento puede impedir la recopilación de información de diagnóstico que podrían tener Microsoft Support para investigar este problema. Puede tener un agente de soporte técnico pedir que vuelva a habilitar la generación de volcado de memoria dinámica en función de los escenarios de solución de problemas específicos.

Hay dos métodos para deshabilitar los volcados de memoria dinámicas, tal como se describe a continuación.

#### Método 1 (en este escenario se recomienda)
Para deshabilitar completamente todos los volcados de memoria, incluidos los volcados de memoria dinámicas para todo el sistema, sigue estos pasos:

1. Crear la clave del registro siguiente: HKLM\System\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled
2. En la nueva clave **ForceDumpsDisabled** , crear una propiedad REG_DWORD como GuardedHost y, a continuación, establece su valor en 0 x 10000000.
3. La nueva clave del registro se aplican a cada nodo del clúster.

>[!NOTE]
>Tendrás que reiniciar el equipo para que surta efecto el cambio de NLA.

Después de esta clave del registro está establecido, dinámicos generará un error y se generará un error de "STATUS_NOT_SUPPORTED" creación de volcado de memoria.

#### Método 2
De manera predeterminada, el informe de errores de Windows te permitirá solo LiveDump por tipo de informe por cada 7 días y solo 1 LiveDump por máquina por 5 días. Puedes cambiar al establecer las siguientes claves del registro para permitir solamente un LiveDump en la máquina para siempre.
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v SystemThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v ComponentThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```

*Nota* Tendrás que reiniciar el equipo para que el cambio surta efecto.

### Método 3
Para deshabilitar la generación de clúster de volcados de memoria dinámicas (por ejemplo, cuando se registra un evento de 5120), ejecuta el siguiente cmdlet:

```powershell
(Get-Cluster).DumpPolicy = ((Get-Cluster).DumpPolicy -band 0xFFFFFFFFFFFFFFFE)
```
Este cmdlet tiene un efecto inmediato en todos los nodos del clúster sin reiniciar el equipo.
    
## Rendimiento de E/S lento

Si experimentas un rendimiento de E/S lento, comprueba si está habilitada la memoria caché en la configuración de espacios de almacenamiento directo. 

Hay dos maneras de comprobar: 
     
 
1. Usar el registro de clúster. Abre el registro de clúster en el editor de texto de elección y busca "[=== SBL discos ===]." Se trata de una lista del disco en el nodo generado en el registro. 

     Ejemplo de los discos de caché Enabled: Ten en cuenta aquí que el estado es CacheDiskStateInitializedAndBound y hay un GUID presente aquí. 

   ```
   [=== SBL Disks ===]
    {26e2e40f-a243-1196-49e3-8522f987df76},3,false,true,1,48,{1ff348f1-d10d-7a1a-d781-4734f4440481},CacheDiskStateInitializedAndBound,1,8087,54,false,false,HGST    ,HUH721010AL4200 ,        7PG3N2ER,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Caché no habilitada: Aquí podemos ver no hay ningún GUID presente y el estado es CacheDiskStateNonHybrid. 
    ```
   [=== SBL Disks ===]
    {426f7f04-e975-fc9d-28fd-72a32f811b7d},12,false,true,1,24,{00000000-0000-0000-0000-000000000000},CacheDiskStateNonHybrid,0,0,0,false,false,HGST    ,HUH721010AL4200 ,        7PGXXG6C,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Caché no habilitada: Cuando todos los discos son del mismo tipo caso no está habilitada de manera predeterminada. Aquí podemos ver no hay ningún GUID presente y el estado es CacheDiskStateIneligibleDataPartition. 
    ```
    {d543f90c-798b-d2fe-7f0a-cb226c77eeed},10,false,false,1,20,{00000000-0000-0000-0000-000000000000},CacheDiskStateIneligibleDataPartition,0,0,0,false,false,NVMe    ,INTEL SSDPE7KX02,  PHLF7330004V2P0LGN,0170,{79b4d631-976f-4c94-a783-df950389fd38},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0], 
    ```  
2. Usar Get-PhysicalDisk.xml desde el SDDCDiagnosticInfo
    1. Abre el archivo XML mediante "$d = Import-Clixml GetPhysicalDisk.XML"
    2. Ejecutar "ipmo almacenamiento"
    3. ejecuta "$d". Ten en cuenta que uso es selección automática, no diario verás salida como este: 

   |FriendlyName|  SerialNumber| MediaType| CanPool| OperationalStatus| HealthStatus| Usage| Tamaño|
   |-----------|------------|---------| -------| -----------------| ------------| -----| ----|
   |NVMe INTEL SSDPE7KX02| PHLF733000372P0LGN| SSD| Falso|   OK|                Correcto|      Selección automática 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504008J2P0LGN| SSD|  Falso|    OK|                Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504005F2P0LGN| SSD|  Falso|  OK|                Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002A2P0LGN| SSD| Falso| OK|    Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504004T2P0LGN |SSD| Falso|OK|       Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002E2P0LGN| SSD| Falso| OK|      Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330002Z2P0LGN| SSD| Falso| OK|      Correcto|Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF733000272P0LGN |SSD| Falso| OK|  Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330001J2P0LGN |SSD| Falso| OK| Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF733000302P0LGN |SSD| Falso| OK|Correcto| Selección automática| 1,82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7330004D2P0LGN |SSD| Falso| OK| Correcto| Selección automática |1,82 TB|
    
## Cómo destruir un clúster existente para que puedas usar los mismos discos vuelve a

En un clúster de espacios de almacenamiento directo, una vez que se deshabilitar espacios de almacenamiento directo y usa el proceso de limpieza descrito en [unidades limpias](deploy-storage-spaces-direct.md#step-31-clean-drives), sigue siendo el grupo de almacenamiento en clúster en un estado sin conexión y el servicio de mantenimiento se quita del clúster.

El siguiente paso consiste en quitar el grupo de almacenamiento ficticias: 
   ```powershell
   Get-ClusterResource -Name "Cluster Pool 1" | Remove-ClusterResource
   ```

Ahora, si ejecuta **Get-PhysicalDisk** en cualquiera de los nodos, podrás ver todos los discos que estaban en el grupo. Por ejemplo, en un laboratorio con un clúster de 4 nodos con 4 discos SAS, de 100 GB cada presentada a cada nodo. En ese caso, después de espacio de almacenamiento directo está deshabilitado, que quita el SBL (capa de Bus de almacenamiento), pero deja el filtro, si ejecuta **Get-PhysicalDisk**, se deben informar 4 discos excluyendo el disco del sistema operativo local. En su lugar que se notificó 16 en su lugar. Este es el mismo para todos los nodos del clúster. Al ejecutar un comando de **Get-Disk** , verás los discos conectados localmente numerados como 0, 1, 2 y así sucesivamente, tal como se muestra en este resultado de ejemplo:

|Número| Nombre descriptivo| Número de serie|HealthStatus|OperationalStatus|Tamaño total| Estilo de partición|
|-|-|-|-|-|-|-|-|
|0|Msft virtual …  ||Correcto | Online|  127 GB| GPT|
||Msft virtual … ||Correcto| Sin conexión| 100 GB| SIN PROCESAR|
||Msft virtual … ||Correcto| Sin conexión| 100 GB| SIN PROCESAR|
||Msft virtual … ||Correcto| Sin conexión| 100 GB| SIN PROCESAR|
||Msft virtual … ||Correcto| Sin conexión| 100 GB| SIN PROCESAR|
|1|Msft virtual …||Correcto| Sin conexión| 100 GB| SIN PROCESAR|
||Msft virtual … ||Correcto| Sin conexión| 100 GB| SIN PROCESAR|
|2|Msft virtual …||Correcto| Sin conexión| 100 GB| SIN PROCESAR|
||Msft virtual … ||Correcto| Sin conexión| 100 GB| SIN PROCESAR|
||Msft virtual … ||Correcto| Sin conexión| 100 GB| SIN PROCESAR|
||Msft virtual … ||Correcto| Sin conexión| 100 GB| SIN PROCESAR|
||Msft virtual … ||Correcto| Sin conexión| 100 GB| SIN PROCESAR|
|4|Msft virtual …||Correcto| Sin conexión| 100 GB| SIN PROCESAR|
|3|Msft virtual …||Correcto| Sin conexión| 100 GB| SIN PROCESAR|
||Msft virtual … ||Correcto| Sin conexión| 100 GB| SIN PROCESAR|
||Msft virtual … ||Correcto| Sin conexión| 100 GB| SIN PROCESAR|
||Msft virtual … ||Correcto| Sin conexión| 100 GB| SIN PROCESAR|

    
## Mensaje de error "tipo de medio no compatible" al crear un clúster de espacios de almacenamiento directo con Enable-ClusterS2D  

Cuando se ejecuta el cmdlet **Enable-ClusterS2D** , es posible que veas errores similares al siguiente:

![Mensaje de error de escenario 6](media/troubleshooting/scenario-error-message.png)

Para corregir este problema, asegúrate de que el adaptador HBA está configurado en el modo HBA. No hay HBA debe configurarse en el modo RAID.  
    
## Enable-ClusterStorageSpacesDirect se bloquea en 'Esperar hasta que SBL discos se exponen' o 27%

Verás la siguiente información en el informe de validación:

    Disk <identifier> connected to node <nodename> returned a SCSI Port Association and the corresponding enclosure device could not be found. The hardware is not compatible with Storage Spaces Direct (S2D), contact the hardware vendor to verify support for SCSI Enclosure Services (SES). 
    
  
Es el problema con la tarjeta de un ampliador SAS HPE que se encuentra entre los discos y la tarjeta HBA. El botón de expansión SAS crea un identificador duplicado entre la primera unidad conectado a la expansión y el botón de expansión.  Esto se ha solucionado en [HPE inteligente matriz SAS un ampliador Firmware de los controladores: 4.02](https://support.hpe.com/hpsc/swd/public/detail?sp4ts.oid=7304566&swItemId=MTX_ef8d0bf4006542e194854eea6a&swEnvOid=4184#tab3).
    
## Serie de Intel SSD DC P4600 tiene un NGUID no único
Es posible que veas un problema donde un dispositivo de la serie de Intel SSD DC P4600 parece va a informar similar de 16 bytes NGUID para varios espacios de nombres como 0100000001000000E4D25C000014E214 o 0100000001000000E4D25C0000EEE214 en el siguiente ejemplo.

|UniqueID| DeviceID |MediaType| Tipo de bus| número de serie| size|canpool| FriendlyName| OperationalStatus|
|-|-|-|-|-|-|-|-|-
|5000CCA251D12E30| 0| HDD| SAS| 7PKR197G|                  10000831348736 |Falso|HGST| HUH721010AL4200| OK|
|EUI.0100000001000000E4D25C000014E214 |4|SSD| NVMe|   0100_0000_0100_0000_E4D2_5C00_0014_E214.|1600321314816|Verdadero| INTEL| SSDPE2KE016T7|  OK|
|EUI.0100000001000000E4D25C000014E214 |5|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_0014_E214.|  1600321314816|    Verdadero| INTEL| SSDPE2KE016T7|  OK|
|EUI.0100000001000000E4D25C0000EEE214| 6|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_00EE_E214.|  1600321314816|    Verdadero| INTEL| SSDPE2KE016T7|  OK|
|EUI.0100000001000000E4D25C0000EEE214| 7|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_00EE_E214.|  1600321314816|    Verdadero| INTEL| SSDPE2KE016T7|  OK|

Para corregir este problema, actualice el firmware en las unidades de Intel a la versión más reciente.  Versión de firmware se conoce QDV101B1 de mayo de 2018 para resolver este problema.

El [lanzamiento de mayo de 2018 de la herramienta de centro de datos de Intel SSD](https://downloadmirror.intel.com/27778/eng/Intel_SSD_Data_Center_Tool_3_0_12_Release_Notes_330715-026.pdf) incluye una actualización de firmware, QDV101B1, para la serie de Intel SSD DC P4600.

    
## "Correcto" disco físico y el estado operativo es "Quitar de grupo" 

En un clúster de espacios de almacenamiento de Windows Server 2016 directo, es posible que veas el valor de discos físicos de uno o más como "Correcto", mientras que la OperationalStatus es "(quitar del grupo, Aceptar de)." 

"Quitar del grupo de" es una intención establece cuando se llama **Remove-PhysicalDisk** pero se almacena en el estado para mantener el estado y permitir la recuperación si se produce un error en la operación de eliminación. Puedes cambiar manualmente la OperationalStatus a correcto con uno de los siguientes métodos:

- Quitar el disco físico de la agrupación y, a continuación, se vuelve a agregar.
- Ejecutar el [script de borrar PhysicalDiskHealthData.ps1](https://go.microsoft.com/fwlink/?linkid=2034205) para borrar la intención. (Disponible para su descarga como un. Archivo TXT. Tendrás que guardar como un. Archivo PS1 antes de que se puede ejecutar.)

Estos son algunos ejemplos que muestran cómo ejecutar el script:

- Usa el parámetro **SerialNumber** para especificar el disco que es necesario establecer a correcto. Puedes obtener el número de serie de **WMI MSFT_PhysicalDisk** o **Get-PhysicalDIsk**. (Solo usamos 0 para el número de serie.)
   
   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -SerialNumber 000000000000000 -Verbose -Force
    ```

- Usa el parámetro **UniqueId** para especificar el disco (de nuevo de **WMI MSFT_PhysicalDisk** o **Get-PhysicalDIsk**).

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -UniqueId 00000000000000000 -Verbose -Force
   ```

## Copia de archivos es lenta

Es posible que se muestra un problema con el Explorador de archivos para copiar un disco duro virtual de gran tamaño en el disco virtual, la copia de archivos tarda más de lo esperado.

Con el Explorador de archivos, Robocopy o Xcopy para copiar un disco duro virtual de gran tamaño en el disco virtual no es un método recomendado para que esto dará más lentas que rendimiento esperado. El proceso de copia no vaya a través de la pila de espacios de almacenamiento directo, que se encuentra una menor en la pila de almacenamiento y actúa en su lugar como un proceso de copia local.

Si quieres probar el rendimiento de espacios de almacenamiento directo, te recomendamos que uses VMFleet y Diskspd a pruebas de esfuerzo y carga los servidores para obtener una línea de base y las expectativas del rendimiento de los espacios de almacenamiento directo.

## Espera eventos que se vería en el resto de los nodos durante el reinicio de un nodo. 

Es seguro omitir estos eventos: 

    Event ID 205: Windows lost communication with physical disk {XXXXXXXXXXXXXXXXXXXX }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

    Event ID 203: Windows lost communication with physical disk {xxxxxxxxxxxxxxxxxxxxxxxx }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

Si estás ejecutando las VM de Azure, puede omitir este evento:

    Event ID 32: The driver detected that the device \Device\Harddisk5\DR5 has its write cache enabled. Data corruption may occur. 
    
## Rendimiento lento o "Comunicación perdida," "Error de E/S", "Oculto", o los errores "No redundancia" para las implementaciones que usan dispositivos NVMe P3x00 de Intel

Hemos identificado un problema fundamental que afecta a algunos usuarios espacios de almacenamiento directo que usan hardware en función de la familia de Intel P3x00 de dispositivos NVM Express (NVMe) con las versiones de firmware antes de "Mantenimiento Release 8". 

>[!NOTE]
> Los OEM individuales pueden tener dispositivos que se basan en la familia de Intel P3x00 de dispositivos NVMe con cadenas de versión de firmware único. Para obtener más información de la última versión de firmware, ponte en contacto con el OEM.

Si estás usando hardware en la implementación en función de la familia de Intel P3x00 de dispositivos de NVMe, te recomendamos que aplique el firmware más reciente disponible inmediatamente (al menos 8 de la versión de mantenimiento). En este [artículo de soporte técnico de Microsoft](https://support.microsoft.com/en-us/help/4052341/slow-performance-or-lost-communication-io-error-detached-or-no-redunda) proporciona información adicional sobre este problema. 
