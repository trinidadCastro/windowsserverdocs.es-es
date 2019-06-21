---
title: Comprender e implementar memoria persistente
description: Información detallada sobre qué memoria persistente y cómo configurarlo con espacios de almacenamiento directo en Windows Server 2019.
keywords: Espacios de almacenamiento directo, memoria persistente, pmem, almacenamiento, S2D
ms.assetid: ''
ms.prod: ''
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 3/26/2019
ms.localizationpriority: ''
ms.openlocfilehash: ed4b2669ad35a2ce0f818c65f7024ce905d9e4d6
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280043"
---
---
# <a name="understand-and-deploy-persistent-memory"></a>Comprender e implementar memoria persistente

>Se aplica a: Windows Server 2019

Memoria persistente (o PMem) es un nuevo tipo de tecnología de memoria que ofrece una combinación única de gran capacidad asequible y persistencia. En este tema se proporciona en segundo plano en PMem y los pasos para implementarlo con Windows Server 2019 con espacios de almacenamiento directo.

## <a name="background"></a>Background

PMem es un tipo de no volátil DRAM (NVDIMM) que tiene la velocidad de DRAM, pero conserva su contenido de la memoria a través de ciclos de alimentación (el contenido de la memoria permanece incluso cuando la energía del sistema deja de funcionar si se produce una pérdida de alimentación inesperado, el apagado iniciado por el usuario, el bloqueo del sistema, etcetera.). Por este motivo, reanudar desde donde lo dejó es significativamente más rápida, ya que no necesita volver a cargar el contenido de la memoria RAM. Otra característica única que es PMem bytes direccionable, lo que significa que también se puede utilizar como almacenamiento (razón por la cual es posible que oiga PMem que se conoce como la memoria de clase de almacenamiento).


Para ver algunas de estas ventajas, echemos un vistazo a esta demostración de 2018 de Microsoft Ignite:

[![Microsoft Ignite 2018 Pmem demo](http://img.youtube.com/vi/8WMXkMLJORc/0.jpg)](http://www.youtube.com/watch?v=8WMXkMLJORc)

Cualquier sistema de almacenamiento que proporciona tolerancia a errores necesariamente hace copias distribuidas de escrituras, lo que deben atravesar la red e incurre amplificación de escritura de back-end. Por este motivo, los números de banco de pruebas de e/s por segundo mayor absolutos normalmente se realizan con lecturas solo, especialmente si el sistema de almacenamiento tiene las optimizaciones de sentido común para leer desde la copia local siempre que sea posible, qué espacios de almacenamiento directo.

**Con el 100% de lecturas, el clúster ofrece 13,798,674 e/s por segundo.**

![captura de pantalla de registro de e/s por segundo de 13.7 m](media/deploy-pmem/iops-record.png)

Si observa atentamente el vídeo, se dará cuenta aún más asombroso del thatwhat es la latencia: incluso en más 13.7 M e/s por segundo, el sistema de archivos en Windows está informando de latencia de microsiemens por constantemente inferior a 40! (Es el símbolo de microsegundos, millonésima de segundo). Esto es un orden de magnitud más rápido que lo que los proveedores de memoria flash típicos orgullo anuncian hoy en día.

Juntos, espacios de almacenamiento directo en Windows Server 2019 y memoria persistente de DC de Intel® Optane™ ofrecen un rendimiento excepcional. Esta referencia HCI líderes del sector de más 13.7 millones de e/s por segundo, con una latencia muy baja y predecible, es más del doble nuestra anterior líderes del sector prueba comparativa de M 6.7 e/s por segundo. ¿Qué es más, esta vez necesitábamos sólo 12 nodos de servidor, 25% menos de dos años.

![Mejoras de e/s por segundo](media/deploy-pmem/iops-gains.png)

El hardware utilizado era un clúster de 12 y el servidor mediante la creación de reflejo triple y delimitado por volúmenes de ReFS, **12** x Intel® S2600WFT, **GiB 384** memoria, el núcleo de 2 x 28 "CascadeLake" **1,5 TB**Memoria persistente de Intel® Optane™ DC como memoria caché, **32 TB** NVMe (4 x 8 TB Intel® DC P4510) como la capacidad, **2** x Mellanox ConnectX-4 25 Gbps

En la tabla siguiente tiene las cifras de rendimiento completa: 

| Banco de pruebas                   | Rendimiento         |
|-----------------------------|---------------------|
| Lectura de 4K de 100% aleatorios         | 13,8 millones de e/s por segundo   |
| 4 K 90/10% aleatorias de lectura/escritura | 9.45 millones de IOPS   |
| Lectura secuencial de 2MB         | Rendimiento 549 GB/s |

### <a name="supported-hardware"></a>Hardware compatible

La tabla siguiente muestra el hardware de memoria persistente admitidos para 2019 de Windows Server y Windows Server 2016. Tenga en cuenta que Intel Optane específicamente admite los modos de memoria y directos de la aplicación. Windows Server 2019 admite operaciones de modo mixto.

| Tecnología de memoria persistente                                      | Windows Server 2016 | Windows Server 2019 |
|-------------------------------------------------------------------|--------------------------|--------------------------|
| **NVDIMM-N** en modo de aplicación directos                                       | Se admite                | Se admite                |
| **Memoria persistente de Intel Optane™ DC** en modo de aplicación directos             | No se admite            | Se admite                |
| **Memoria persistente de Intel Optane™ DC** en modo de memoria de nivel dos (2LM) | No se admite            | Se admite                |

Ahora, vamos a profundizar en cómo configurar memoria persistente.

## <a name="interleave-sets"></a>Conjuntos de intercalación

### <a name="understanding-interleave-sets"></a>Descripción de intercalación conjuntos

Recuerde que el NVDIMM-N reside en una ranura DIMM (memoria) estándar, colocar los datos más cerca del procesador (por lo tanto, lo que reduce la latencia y obtener un mejor rendimiento). Para sacar provecho de esto, un conjunto de intercalación es cuando dos o más NVDIMMs creación una intercalación de N-Way para proporcionar operaciones de lectura/escritura de franjas para aumentar la capacidad. Las configuraciones más comunes son 2 o 4 direcciones intercalada.

A menudo se pueden crear conjuntos de intercalado en BIOS de la plataforma para que varios dispositivos de memoria persistente aparezca como un único disco lógico a Windows Server. Cada disco lógico de memoria persistente contiene un conjunto intercalado de dispositivos físicos mediante la ejecución:

```PowerShell
Get-PmemDisk
```

A continuación se muestra un ejemplo de salida:

```
DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

Podemos ver que el disco lógico pmem #2 tiene dispositivos físicos de Id20 y Id120 y el disco lógico pmem #3 tiene dispositivos físicos de Id1020 y Id1120. También nos podemos fuente un disco específico pmem a Get-PmemPhysicalDevice para obtener todos sus NVDIMMs físicos en la intercalación establece como sigue.


```PowerShell
(Get-PmemDisk)[0] | Get-PmemPhysicalDevice
```

A continuación se muestra un ejemplo de salida:

```
DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
20       Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_C1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
```

### <a name="configuring-interleave-sets"></a>Configurar conjuntos de intercalación

Para configurar un conjunto de intercalación, ejecute el siguiente cmdlet de PowerShell:

```PowerShell
Get-PmemUnusedRegion

RegionId TotalSizeInBytes DeviceId
-------- ---------------- --------
       1     270582939648 {20, 120}
       3     270582939648 {1020, 1120}
```

Esto muestra todas las regiones de memoria persistente no asignadas a un disco lógico de memoria persistente en el sistema.

Para ver toda la información de dispositivos de memoria persistente en el sistema, incluido el tipo de dispositivo, ubicación, estado y el estado operativo, etc. puede ejecutar el siguiente cmdlet en el servidor local:

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

Puesto que tenemos disponible pmem sin usar región, podemos crear nuevos discos de memoria persistente. Podemos crear varios discos persistentes de memoria utilizando las regiones no usadas por:

```PowerShell
Get-PmemUnusedRegion | New-PmemDisk
Creating new persistent memory disk. This may take a few moments.
```

Después de ello, podemos ver los resultados mediante la ejecución:

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

Es importante destacar que podríamos haber ejecutar **Get-PhysicalDisk | Donde MediaType - Eq SCM** en lugar de **Get PmemDisk** para obtener los mismos resultados. El disco recién creado memoria persistente corresponde a las unidades que aparecen en PowerShell y Windows Admin Center de 1:1.

### <a name="using-persistent-memory-for-cache-or-capacity"></a>Uso de memoria persistente para la memoria caché o la capacidad

Espacios de almacenamiento directo en Windows Server 2019 admite el uso de memoria persistente como una caché o la capacidad de unidad. Vea este [documentación](understand-the-cache.md) para obtener más información sobre cómo configurar las unidades de caché y capacidad.

## <a name="creating-a-dax-volume"></a>Creación de un volumen DAX

### <a name="understanding-dax"></a>Descripción de DAX

Hay dos métodos para tener acceso a memoria persistente. Estas sobrecargas son:

1. **(DAX) acceso directo**, que actúa como la memoria para obtener la latencia más baja. La aplicación modifica directamente la memoria persistente, omitir la pila. Tenga en cuenta que solo se puede usar con NTFS.
2. **Bloquear el acceso**, que actúa como el almacenamiento para la compatibilidad de aplicaciones. Los datos fluyen a través de la pila en esta configuración, y esto se puede usar con NTFS y ReFS.

A continuación, se puede ver un ejemplo de esto:

![Pila DAX](media/deploy-pmem/dax.png)

### <a name="configuring-dax"></a>Configuración de DAX

Deberá usar los cmdlets de PowerShell para crear un volumen DAX en una memoria persistente. Mediante el uso de la **- IsDax** conmutador, nos podemos dar formato a un volumen para que sea DAX habilitado.

```PowerShell
Format-Volume -IsDax:$true
```

El fragmento de código siguiente le ayudará a crear un volumen DAX en un disco de memoria persistente.

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

## <a name="monitoring-health"></a>Supervisar el estado

Cuando se utiliza memoria persistente, hay algunas diferencias en la experiencia de supervisión:

1. Memoria persistente no crea contadores de rendimiento, por lo que no verá si aparecen en los gráficos en Windows Admin Center de disco físico.
2. Memoria persistente no crea datos 505 Storport, por lo que no obtendrá la detección proactiva de valores atípicos.

Aparte de eso, la experiencia de supervisión es igual que cualquier otro disco físico. Puede consultar el estado de un disco de memoria persistente mediante la ejecución:

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

**HealthStatus** muestra si el disco de memoria persistente es correcto o no. El **UnsafeshutdownCount** realiza un seguimiento del número de secuencias de apagado que puede provocar la pérdida de datos en este disco lógico. Es la suma de los recuentos de apagado no seguro de todos los dispositivos de memoria persistente subyacente de este disco. También podemos usar los siguientes comandos para la información de estado de la consulta. El **OperationalStatus** y **OperationalDetails** proporcionan más información sobre el estado de mantenimiento.

Para consultar el estado del dispositivo de memoria persistente:

```PowerShell
Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
1020     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_C1     102005310        126 GB                 0 GB
1120     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_F1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
20       Intel INVDIMM device Unhealthy    {HardwareError}   CPU1_DIMM_C1     102005310        126 GB                 0 GB
```

Esto muestra qué dispositivo de memoria persistente está en mal estado. El dispositivo incorrecto (**DeviceId**) 20 coincide con el caso en el ejemplo anterior. El **PhysicalLocation** de BIOS puede ayudar a identificar qué memoria persistente dispositivo está en estado defectuoso.

## <a name="replacing-persistent-memory"></a>Reemplazo de memoria persistente

Anteriormente, se describe cómo ver el estado de mantenimiento de la memoria persistente. Si necesita reemplazar un módulo con error, deberá volver a aprovisionar el disco de memoria persistente (consulte los pasos que se ha descrito anteriormente).

Para solucionar el problema, es posible que deba usar **Remove-PmemDisk**, que quita un disco de memoria persistente concreta. Podemos quitar todos los discos persistentes actuales por:

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

Es importante tener en cuenta que la eliminación de un disco de memoria persistente producirá pérdida de datos en ese disco.

Es otro cmdlet que puede que necesite **Initialize PmemPhysicalDevice**, responsable de inicializar el área de almacenamiento de la etiqueta en los dispositivos físicos de memoria persistente. Esto puede utilizarse para borrar la información de etiqueta dañado de almacenamiento en los dispositivos de memoria persistente.

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

Es importante tener en cuenta que este comando debe usarse como último recurso para corregir memoria persistente relacionados con problemas. Dará lugar a pérdida de datos a la memoria persistente.

## <a name="see-also"></a>Vea también

- [Información general de espacios directo de almacenamiento](storage-spaces-direct-overview.md)
- [Administración de estado de clase de almacenamiento (NVDIMM-N) de memoria en Windows](storage-class-memory-health.md)
- [Descripción de la memoria caché](understand-the-cache.md)