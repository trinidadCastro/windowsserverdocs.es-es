---
title: Comprensión e implementación de memoria persistente
description: Información detallada sobre qué es la memoria persistente y cómo configurarla con espacios de almacenamiento directo en Windows Server 2019.
ms.prod: windows-server
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 1/27/2020
ms.localizationpriority: medium
ms.openlocfilehash: 43268986f0ef42aabc218062ac19f1d98f27be6d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861038"
---
# <a name="understand-and-deploy-persistent-memory"></a>Comprensión e implementación de memoria persistente

> Se aplica a: Windows Server 2019

La memoria persistente (o PMem) es un nuevo tipo de tecnología de memoria que proporciona una combinación única de capacidad y persistencia asequibles. En este artículo se proporciona información general sobre PMem y los pasos para implementarlo en Windows Server 2019 mediante Espacios de almacenamiento directo.

## <a name="background"></a>Fondo

PMem es un tipo de RAM no volátil (NVDIMM) que conserva su contenido a través de ciclos de energía. El contenido de la memoria permanece incluso cuando se interrumpe la alimentación del sistema en caso de que se produzca una pérdida de energía inesperada, el cierre Iniciado por el usuario, el bloqueo del sistema, etc. Esta característica única significa que también puede usar PMem como almacenamiento. Este es el motivo por el que puede oír que los usuarios hacen referencia a PMem como "memoria de clase de almacenamiento".

Para ver algunas de estas ventajas, echemos un vistazo a la demostración siguiente de Microsoft encendido 2018.

[demostración de ![de Microsoft encendido 2018 Pmem](http://img.youtube.com/vi/8WMXkMLJORc/0.jpg)](http://www.youtube.com/watch?v=8WMXkMLJORc)

Cualquier sistema de almacenamiento que proporcione tolerancia a errores realiza necesariamente copias distribuidas de escrituras. Dichas operaciones deben atravesar la red y amplificar el tráfico de escritura de back-end. Por esta razón, los números de banco de pruebas de IOPS más grandes se suelen lograr mediante la medición de las lecturas únicamente, especialmente si el sistema de almacenamiento tiene optimizaciones comunes para leer de la copia local siempre que sea posible. Espacios de almacenamiento directo está optimizado para ello.

**Cuando se mide usando solo operaciones de lectura, el clúster ofrece 13.798.674 IOPS.**

![captura de pantalla de registro de IOPS de 13.7 m](media/deploy-pmem/iops-record.png)

Si ve el vídeo en estrecha, observará que lo que es incluso más Jaw es la latencia. Incluso a más de 13,7 millones de IOPS, el sistema de archivos de Windows está informando de una latencia que es constantemente inferior a 40 μs. (Es decir, el símbolo de microsegundos, una-millonésimas de segundo). Esta velocidad es un orden de magnitud más rápido que la típica que los proveedores All-Flash anuncian hoy en día.

Juntos, Espacios de almacenamiento directo en Windows Server 2019 e Intel&reg; Optane&trade; memoria persistente de DC ofrecen un rendimiento innovador. Este benchmark de HCl líder del sector de más de 13.7 M e/s por segundo, acompañado por una latencia predecible y extremadamente baja, es más que el doble de la prueba comparativa anterior líder del sector de 6,7 M IOPS. Lo que es más, esta vez necesitamos solo 12 nodos de servidor&mdash;25 por ciento menos de hace dos años.

![Ventajas de IOPS](media/deploy-pmem/iops-gains.png)

El hardware de prueba era un clúster de 12 servidores que se configuró para usar volúmenes de creación de reflejo triple y de ReFS delimitados. **12** x Intel&reg; S2600WFT, **384 GIB** Memory, 2 x 28-Core "CASCADELAKE", **1,5 TB** Intel&reg; Optane&trade; dc persistente Memory as cache, **32 TB** NVME (4 x 8 TB Intel&reg; DC P4510) como capacidad, **2** x Mellanox ConnectX-4 25 Gbps.

En la tabla siguiente se muestran los números de rendimiento completos.  

| Analizar                   | Rendimiento         |
|-----------------------------|---------------------|
| lectura aleatoria de 4K 100%         | 13,8 millones IOPS   |
| 4K 90/10% de lectura/escritura aleatoria | 9.450.000 IOPS   |
| lectura secuencial de 2 MB         | rendimiento de 549 GB/s |

### <a name="supported-hardware"></a>Hardware compatible

En la tabla siguiente se muestra el hardware de memoria persistente compatible para Windows Server 2019 y Windows Server 2016.  

| Tecnología de memoria persistente                                      | Windows Server 2016 | Windows Server 2019 |
|-------------------------------------------------------------------|--------------------------|--------------------------|
| **NVDIMM-N** en modo persistente                                  | Se admite                | Se admite                |
| **Memoria persistente de DC de&trade; de Intel Optane** en modo de aplicación directa             | No se admite            | Se admite                |
| **Memoria persistente de DC de&trade; de Intel Optane** en modo de memoria | Se admite            | Se admite                |

> [!NOTE]  
> Intel Optane admite los modos de *memoria* (volátil) y *aplicación directa* (persistente).
   
> [!NOTE]  
> Cuando se reinicia un sistema que tiene varios módulos de Intel&reg; Optane&trade; PMem en modo de aplicación directa que se dividen en varios espacios de nombres, es posible que se pierda el acceso a algunos o a todos los discos de almacenamiento lógicos relacionados. Este problema se produce en las versiones de Windows Server 2019 anteriores a la versión 1903.
>   
> Esta pérdida de acceso se produce porque un módulo PMem no está entrenado o produce un error cuando se inicia el sistema. En tal caso, se produce un error en todos los espacios de nombres de almacenamiento de cualquier módulo de PMem en el sistema, incluidos los espacios de nombres que no se asignan físicamente al módulo con errores.
>   
> Para restaurar el acceso a todos los espacios de nombres, reemplace el módulo con error.
>   
> Si se produce un error en un módulo en Windows Server 2019 versión 1903 o versiones más recientes, se pierde el acceso solo a los espacios de nombres que se asignan físicamente al módulo afectado. Otros espacios de nombres no se ven afectados.

Ahora, vamos a profundizar en cómo configurar la memoria persistente.

## <a name="interleaved-sets"></a>Conjuntos intercalados

### <a name="understanding-interleaved-sets"></a>Descripción de los conjuntos intercalados

Recuerde que un NVDIMM reside en una ranura DIMM estándar (memoria), que coloca los datos más cerca del procesador. Esta configuración reduce la latencia y mejora el rendimiento de la captura. Para aumentar aún más el rendimiento, dos o más NVDIMMs crean un conjunto de intercalación n-Way para seccionar las operaciones de lectura y escritura. Las configuraciones más comunes son intercalados bidireccionales o bidireccionales. Un conjunto intercalado también hace que aparezcan varios dispositivos de memoria persistentes como un único disco lógico en Windows Server. Puede usar el cmdlet **Get-PmemDisk** de Windows PowerShell para revisar la configuración de estos discos lógicos, como se indica a continuación:

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

Podemos ver que el disco lógico PMem #2 usa los dispositivos físicos Id20 y Id120 y el disco físico PMem #3 usa los dispositivos físicos Id1020 y Id1120.  

Para recuperar más información sobre el conjunto intercalado que usa una unidad lógica, ejecute el cmdlet **Get-PmemPhysicalDevice** :

```PowerShell
(Get-PmemDisk)[0] | Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
20       Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_C1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
```

### <a name="configuring-interleaved-sets"></a>Configuración de conjuntos intercalados

Para configurar un conjunto intercalado, empiece por revisar todas las regiones de memoria persistentes que no están asignadas a un disco PMem lógico en el sistema. Para ello, ejecute el siguiente cmdlet de PowerShell:

```PowerShell
Get-PmemUnusedRegion

RegionId TotalSizeInBytes DeviceId
-------- ---------------- --------
       1     270582939648 {20, 120}
       3     270582939648 {1020, 1120}
```

Para ver toda la información del dispositivo PMem en el sistema, incluidos el tipo de dispositivo, la ubicación, el estado de mantenimiento y operativo, etc., ejecute el siguiente cmdlet en el servidor local:

```PowerShell
Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile
                                                                                                                      memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------
1020     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_C1     102005310        126 GB                 0 GB
1120     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_F1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
20       Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_C1     102005310        126 GB                 0 GB
```

Como tenemos una región de PMem disponible sin usar, podemos crear nuevos discos de memoria persistentes. Podemos usar la región sin usar para crear varios discos de memoria persistentes mediante la ejecución de los siguientes cmdlets:

```PowerShell
Get-PmemUnusedRegion | New-PmemDisk
Creating new persistent memory disk. This may take a few moments.
```

Una vez hecho esto, podemos ver los resultados mediante la ejecución de:

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

Merece la pena mencionar que se puede ejecutar **Get-PhysicalDisk | Donde MediaType-EQ SCM** en lugar de **Get-PmemDisk** para obtener los mismos resultados. El disco PMem recién creado se corresponde uno a uno con las unidades que aparecen en PowerShell y en el centro de administración de Windows.

### <a name="using-persistent-memory-for-cache-or-capacity"></a>Uso de memoria persistente para caché o capacidad

Espacios de almacenamiento directo en Windows Server 2019 admite el uso de memoria persistente como una unidad de memoria caché o de capacidad. Para obtener más información acerca de cómo configurar la memoria caché y las unidades de capacidad, consulte [Descripción de la memoria caché en espacios de almacenamiento directo](understand-the-cache.md).

## <a name="creating-a-dax-volume"></a>Creación de un volumen DAX

### <a name="understanding-dax"></a>Descripción de DAX

Hay dos métodos para tener acceso a la memoria persistente. Son:

1. **Acceso directo (Dax)** , que funciona como la memoria para obtener la latencia más baja. La aplicación modifica directamente la memoria persistente, omitiendo la pila. Tenga en cuenta que solo puede usar DAX en combinación con NTFS.
1. **Bloquear el acceso**, que funciona como el almacenamiento para la compatibilidad de aplicaciones. En este configuración, los datos fluyen a través de la pila. Puede usar esta configuración en combinación con NTFS y ReFS.

En la ilustración siguiente se muestra un ejemplo de una configuración de DAX:

![Pila DAX](media/deploy-pmem/dax.png)

### <a name="configuring-dax"></a>Configuración de DAX

Tenemos que usar cmdlets de PowerShell para crear un volumen DAX en un disco de memoria persistente. Con el modificador **-IsDax** , se puede dar formato a un volumen para que esté habilitado para DAX.

```PowerShell
Format-Volume -IsDax:$true
```

El siguiente fragmento de código le ayuda a crear un volumen DAX en un disco de memoria persistente.

```PowerShell
# Here we use the first pmem disk to create the volume as an example
$disk = (Get-PmemDisk)[0] | Get-PhysicalDisk | Get-Disk
# Initialize the disk to GPT if it is not initialized
If ($disk.partitionstyle -eq "RAW") {$disk | Initialize-Disk -PartitionStyle GPT}
# Create a partition with drive letter 'S' (can use any available drive letter)
$disk | New-Partition -DriveLetter S -UseMaximumSize


   DiskPath: \\?\scmld#ven_8980&dev_097a&subsys_89804151&rev_0018#3&1b1819f6&0&03018089fb63494db728d8418b3cbbf549997891#{53f56307-b6
bf-11d0-94f2-00a0c91efb8b}

PartitionNumber  DriveLetter Offset                                               Size Type
---------------  ----------- ------                                               ---- ----
2                S           16777216                                        251.98 GB Basic

# Format the volume with drive letter 'S' to DAX Volume
Format-Volume -FileSystem NTFS -IsDax:$true -DriveLetter S

DriveLetter FriendlyName FileSystemType DriveType HealthStatus OperationalStatus SizeRemaining      Size
----------- ------------ -------------- --------- ------------ ----------------- -------------      ----
S                        NTFS           Fixed     Healthy      OK                    251.91 GB 251.98 GB

# Verify the volume is DAX enabled
Get-Partition -DriveLetter S | fl


UniqueId             : {00000000-0000-0000-0000-000100000000}SCMLD\VEN_8980&DEV_097A&SUBSYS_89804151&REV_0018\3&1B1819F6&0&03018089F
                       B63494DB728D8418B3CBBF549997891:WIN-8KGI228ULGA
AccessPaths          : {S:\, \\?\Volume{cf468ffa-ae17-4139-a575-717547d4df09}\}
DiskNumber           : 2
DiskPath             : \\?\scmld#ven_8980&dev_097a&subsys_89804151&rev_0018#3&1b1819f6&0&03018089fb63494db728d8418b3cbbf549997891#{5
                       3f56307-b6bf-11d0-94f2-00a0c91efb8b}
DriveLetter          : S
Guid                 : {cf468ffa-ae17-4139-a575-717547d4df09}
IsActive             : False
IsBoot               : False
IsHidden             : False
IsOffline            : False
IsReadOnly           : False
IsShadowCopy         : False
IsDAX                : True                   # <- True: DAX enabled
IsSystem             : False
NoDefaultDriveLetter : False
Offset               : 16777216
OperationalStatus    : Online
PartitionNumber      : 2
Size                 : 251.98 GB
Type                 : Basic
```

## <a name="monitoring-health"></a>Supervisión del estado

Cuando se usa la memoria persistente, existen algunas diferencias en la experiencia de supervisión:

- La memoria persistente no crea contadores de rendimiento de disco físico, por lo que no verá que aparecen en los gráficos del centro de administración de Windows.
- La memoria persistente no crea datos de Storport 505, por lo que no obtendrá la detección de valores atípicos proactivos.

Aparte de eso, la experiencia de supervisión es la misma que para cualquier otro disco físico. Puede consultar el estado de un disco de memoria persistente mediante la ejecución de los siguientes cmdlets:

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Unhealthy    None          True         {20, 120}         2
3          252 GB Healthy      None          True         {1020, 1120}      0

Get-PmemDisk | Get-PhysicalDisk | select SerialNumber, HealthStatus, OperationalStatus, OperationalDetails

SerialNumber               HealthStatus OperationalStatus  OperationalDetails
------------               ------------ ------------------ ------------------
802c-01-1602-117cb5fc      Healthy      OK
802c-01-1602-117cb64f      Warning      Predictive Failure {Threshold Exceeded,NVDIMM_N Error}
```

**HealthStatus** muestra si el disco PMem está en buen estado.  

El valor **UnsafeshutdownCount** realiza un seguimiento del número de apagados que pueden provocar la pérdida de datos en este disco lógico. Es la suma de los recuentos de cierres no seguros de todos los dispositivos PMem subyacentes de este disco. Para obtener más información sobre el estado de mantenimiento, use el cmdlet **Get-PmemPhysicalDevice** para buscar información como **OperationalStatus**.

```PowerShell
Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
1020     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_C1     102005310        126 GB                 0 GB
1120     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_F1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
20       Intel INVDIMM device Unhealthy    {HardwareError}   CPU1_DIMM_C1     102005310        126 GB                 0 GB
```

Este cmdlet muestra qué dispositivo de memoria persistente tiene un estado incorrecto. El dispositivo incorrecto (**DeviceId** 20) coincide con el caso del ejemplo anterior. El **PhysicalLocation** en BIOS puede ayudar a identificar qué dispositivo de memoria persistente está en estado defectuoso.

## <a name="replacing-persistent-memory"></a>Reemplazar la memoria persistente

En este artículo se describe cómo ver el estado de mantenimiento de la memoria persistente. Si tiene que reemplazar un módulo con errores, tendrá que volver a aprovisionar el disco PMem (consulte los pasos descritos anteriormente).

Al solucionar el problema, es posible que tenga que usar **Remove-PmemDisk**. Este cmdlet quita un disco de memoria persistente específico. Podemos quitar todos los discos PMem actuales mediante la ejecución de los siguientes cmdlets:

```PowerShell
Get-PmemDisk | Remove-PmemDisk

cmdlet Remove-PmemDisk at command pipeline position 1
Supply values for the following parameters:
DiskNumber: 2

This will remove the persistent memory disk(s) from the system and will result in data loss.
Remove the persistent memory disk(s)?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): Y
Removing the persistent memory disk. This may take a few moments.
```

> [!IMPORTANT]  
> Al quitar un disco de memoria persistente, se produce una pérdida de datos en ese disco.

Otro cmdlet que podría necesitar es **Initialize-PmemPhysicalDevice**. Este cmdlet inicializa las áreas de almacenamiento de etiquetas en los dispositivos de memoria persistentes física y puede borrar información de almacenamiento de etiquetas dañadas en los dispositivos PMem.

```PowerShell
Get-PmemPhysicalDevice | Initialize-PmemPhysicalDevice

This will initialize the label storage area on the physical persistent memory device(s) and will result in data loss.
Initializes the physical persistent memory device(s)?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): A
Initializing the physical persistent memory device. This may take a few moments.
Initializing the physical persistent memory device. This may take a few moments.
Initializing the physical persistent memory device. This may take a few moments.
Initializing the physical persistent memory device. This may take a few moments.
```

> [!IMPORTANT]  
> **Initialize-PmemPhysicalDevice provoca la** pérdida de datos en la memoria persistente. Úselo como último recurso para corregir problemas persistentes relacionados con la memoria.

## <a name="see-also"></a>Vea también

- [Información general de Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
- [Administración de estado de memoria de clase de almacenamiento (NVDIMM-N) en Windows](storage-class-memory-health.md)
- [Descripción de la memoria caché](understand-the-cache.md)
