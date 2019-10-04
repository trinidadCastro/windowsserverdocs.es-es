---
title: Comprensión e implementación de memoria persistente
description: Información detallada sobre qué es la memoria persistente y cómo configurarla con espacios de almacenamiento directo en Windows Server 2019.
keywords: Espacios de almacenamiento directo, memoria persistente, pmem, almacenamiento, S2D
ms.assetid: ''
ms.prod: ''
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 3/26/2019
ms.localizationpriority: ''
ms.openlocfilehash: 549cc6dbeec3d414e886f6ebf32315ae13627812
ms.sourcegitcommit: de71970be7d81b95610a0977c12d456c3917c331
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2019
ms.locfileid: "71940805"
---
---
# <a name="understand-and-deploy-persistent-memory"></a>Comprensión e implementación de memoria persistente

>Se aplica a: Windows Server 2019

La memoria persistente (o PMem) es un nuevo tipo de tecnología de memoria que proporciona una combinación única de capacidad y persistencia asequibles. En este tema se proporciona información general sobre PMem y los pasos para implementarlo con Windows Server 2019 con Espacios de almacenamiento directo.

## <a name="background"></a>Background

PMem es un tipo de RAM no volátil (NVDIMM) que conserva su contenido a través de ciclos de energía. El contenido de la memoria permanece aunque la alimentación del sistema deje de funcionar en caso de que se produzca una pérdida de energía inesperada, el cierre Iniciado por el usuario, el bloqueo del sistema, etc. Esta característica única significa que también puede usar PMem como almacenamiento, que es el motivo por el que puede oír que se hace referencia a PMem como "memoria de clase de almacenamiento".

Para ver algunas de estas ventajas, echemos un vistazo a esta demostración de Microsoft encendido 2018:

[![Demostración de Pmem de Microsoft encendido 2018](http://img.youtube.com/vi/8WMXkMLJORc/0.jpg)](http://www.youtube.com/watch?v=8WMXkMLJORc)

Cualquier sistema de almacenamiento que proporcione tolerancia a errores realiza necesariamente copias distribuidas de escrituras, que deben atravesar la red e incurre en la amplificación de escritura de back-end. Por esta razón, los números de banco de pruebas de IOPS más grandes se suelen alcanzar con las lecturas, especialmente si el sistema de almacenamiento tiene optimizaciones comunes para leer de la copia local siempre que sea posible, lo que Espacios de almacenamiento directo.

**Con un 100% de lecturas, el clúster ofrece 13.798.674 IOPS.**

![captura de pantalla de registro de IOPS de 13.7 m](media/deploy-pmem/iops-record.png)

Si ve el vídeo en estrecha profundidad, observará que la eliminación de thatwhat aún más Jaw es la latencia: incluso a más de 13,7 millones de IOPS, el sistema de archivos de Windows está informando de una latencia que es siempre inferior a 40 μs. (Es decir, el símbolo de microsegundos, una-millonésimas de segundo). Se trata de un orden de magnitud más rápido que los típicos proveedores de Flash que se anuncian en la actualidad.

Juntos, Espacios de almacenamiento directo en Windows Server 2019 e Intel® Optane™ memoria persistente de DC ofrecen un rendimiento innovador. Este benchmark de HCl líder del sector de más de 13.7 M e/s por segundo, con una latencia predecible y extremadamente baja, es más que el doble de la prueba comparativa anterior líder del sector de 6,7 M e/s por segundo. Lo que es más, esta vez necesitábamos solo 12 nodos de servidor, un 25% menos de hace dos años.

![Ventajas de IOPS](media/deploy-pmem/iops-gains.png)

El hardware que se usa era un clúster de 12 servidores con volúmenes ReFS de creación de reflejo triple y delimitados, **12** x Intel® S2600WFT, **384 GIB** de memoria, 2 x 28-Core "CASCADELAKE", **1,5 TB** Intel® Optane™ DC persistente Memory as cache, **32 TB** NVMe ( 4 x 8 TB Intel® DC P4510) como capacidad, **2** x Mellanox connectx-4 25 Gbps

En la tabla siguiente se muestran los números de rendimiento completos: 

| Analizar                   | Rendimiento         |
|-----------------------------|---------------------|
| Lectura aleatoria de 4K 100%         | 13,8 millones IOPS   |
| 4K 90/10% de lectura/escritura aleatoria | 9.450.000 IOPS   |
| Lectura secuencial de 2 MB         | Rendimiento de 549 GB/s |

### <a name="supported-hardware"></a>Hardware compatible

En la tabla siguiente se muestra el hardware de memoria persistente compatible para Windows Server 2019 y Windows Server 2016. Tenga en cuenta que Intel Optane admite tanto memoria (es decir, volátil) como aplicación directa (es decir, persistentes).

| Tecnología de memoria persistente                                      | Windows Server 2016 | Windows Server 2019 |
|-------------------------------------------------------------------|--------------------------|--------------------------|
| **NVDIMM-N** en modo persistente                                  | Se admite                | Se admite                |
| **Memoria persistente de DC de™ de Intel Optane** en modo de aplicación directa             | No se admite            | Se admite                |
| **Memoria persistente de DC de™ de Intel Optane** en modo de memoria | Se admite            | Se admite                |

Ahora, vamos a profundizar en cómo configurar la memoria persistente.

## <a name="interleave-sets"></a>Conjuntos de intercalación

### <a name="understanding-interleave-sets"></a>Descripción de los conjuntos de intercalación

Recuerde que un NVDIMM reside en una ranura DIMM estándar (memoria), colocando los datos más cerca del procesador (por lo tanto, se reduce la latencia y se obtiene un mejor rendimiento). Para compilar en este caso, un conjunto de intercalación es cuando dos o más NVDIMMs crean un conjunto de intercalación N-Way para proporcionar operaciones de lectura y escritura de franjas para aumentar el rendimiento. Las configuraciones más comunes son intercalados bidireccionales o bidireccionales.

A menudo, se pueden crear conjuntos intercalados en el BIOS de una plataforma para que aparezcan varios dispositivos de memoria persistentes como un único disco lógico en Windows Server. Cada disco lógico de memoria persistente contiene un conjunto intercalado de dispositivos físicos mediante la ejecución de:

```PowerShell
Get-PmemDisk
```

A continuación se muestra un resultado de ejemplo:

```
DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

Podemos ver que el disco lógico pmem #2 tiene dispositivos físicos de Id20 y Id120 y el disco físico pmem #3 tiene dispositivos físicos de Id1020 y Id1120. También podemos alimentar un disco pmem específico a get-PmemPhysicalDevice para obtener todos sus NVDIMMs físicos en el conjunto de intercalación como se indica a continuación.


```PowerShell
(Get-PmemDisk)[0] | Get-PmemPhysicalDevice
```

A continuación se muestra un resultado de ejemplo:

```
DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
20       Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_C1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
```

### <a name="configuring-interleave-sets"></a>Configuración de conjuntos de intercalación

Para configurar un conjunto de intercalación, ejecute el siguiente cmdlet de PowerShell:

```PowerShell
Get-PmemUnusedRegion

RegionId TotalSizeInBytes DeviceId
-------- ---------------- --------
       1     270582939648 {20, 120}
       3     270582939648 {1020, 1120}
```

Esto muestra todas las regiones de memoria persistentes que no están asignadas a un disco lógico de memoria persistente en el sistema.

Para ver toda la información de los dispositivos de memoria persistentes en el sistema, incluidos el tipo de dispositivo, la ubicación, el estado operativo y de mantenimiento, etc. puede ejecutar el siguiente cmdlet en el servidor local:

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

Como hemos disponible la región de pmem sin usar, podemos crear nuevos discos de memoria persistentes. Podemos crear varios discos de memoria persistentes mediante las regiones no utilizadas:

```PowerShell
Get-PmemUnusedRegion | New-PmemDisk
Creating new persistent memory disk. This may take a few moments.
```

Después, podemos ver los resultados mediante la ejecución de:

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

Merece la pena mencionar que podríamos haber ejecutado **Get-PhysicalDisk | Donde MediaType-EQ SCM** en lugar de **Get-PmemDisk** para obtener los mismos resultados. El disco de memoria persistente recién creado 1:1 corresponde a las unidades que aparecen en el centro de administración de Windows y PowerShell.

### <a name="using-persistent-memory-for-cache-or-capacity"></a>Uso de memoria persistente para caché o capacidad

Espacios de almacenamiento directo en Windows Server 2019 admite el uso de memoria persistente como unidad de caché o capacidad. Consulte esta [documentación](understand-the-cache.md) para obtener más detalles sobre la configuración de unidades de memoria caché y capacidad.

## <a name="creating-a-dax-volume"></a>Creación de un volumen DAX

### <a name="understanding-dax"></a>Descripción de DAX

Hay dos métodos para tener acceso a la memoria persistente. Estas sobrecargas son:

1. **Acceso directo (Dax)** , que funciona como la memoria para obtener la latencia más baja. La aplicación modifica directamente la memoria persistente, omitiendo la pila. Tenga en cuenta que esto solo se puede usar con NTFS.
2. **Bloquear el acceso**, que funciona como el almacenamiento para la compatibilidad de aplicaciones. Los datos fluyen a través de la pila en esta configuración y se pueden usar con NTFS y ReFS.

A continuación se muestra un ejemplo de esto:

![Pila DAX](media/deploy-pmem/dax.png)

### <a name="configuring-dax"></a>Configuración de DAX

Tendremos que usar los cmdlets de PowerShell para crear un volumen DAX en una memoria persistente. Con el modificador **-IsDax** , se puede dar formato a un volumen para que esté habilitado para DAX.

```PowerShell
Format-Volume -IsDax:$true
```

El siguiente fragmento de código le ayudará a crear un volumen DAX en un disco de memoria persistente.

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

1. La memoria persistente no crea contadores de rendimiento de disco físico, por lo que no verá si aparecen en los gráficos del centro de administración de Windows.
2. La memoria persistente no crea datos de Storport 505, por lo que no obtendrá la detección de valores atípicos proactivos.

Aparte de eso, la experiencia de supervisión es la misma que cualquier otro disco físico. Puede consultar el estado de un disco de memoria persistente mediante la ejecución de:

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

**HealthStatus** muestra si el disco de memoria persistente está en buen estado o no. **UnsafeshutdownCount** realiza un seguimiento del número de apagados que pueden provocar la pérdida de datos en este disco lógico. Es la suma de los recuentos de cierres no seguros de todos los dispositivos de memoria persistentes subyacentes de este disco. También podemos usar los comandos siguientes para consultar la información de estado. **OperationalStatus** y **OperationalDetails** proporcionan más información sobre el estado de mantenimiento.

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

Esto muestra qué dispositivo de memoria persistente está en mal estado. El dispositivo incorrecto (**DeviceId**) 20 coincide con el caso del ejemplo anterior. El **PhysicalLocation** del BIOS puede ayudar a identificar qué dispositivo de memoria persistente está en estado defectuoso.

## <a name="replacing-persistent-memory"></a>Reemplazar la memoria persistente

Anteriormente se describe cómo ver el estado de mantenimiento de la memoria persistente. Si necesita reemplazar un módulo con errores, tendrá que volver a aprovisionar el disco de memoria persistente (consulte los pasos descritos anteriormente).

A la hora de solucionar problemas, es posible que deba usar **Remove-PmemDisk**, que quita un disco de memoria persistente específico. Podemos quitar todos los discos persistentes actuales:

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

Es importante tener en cuenta que la eliminación de un disco de memoria persistente producirá una pérdida de datos en ese disco.

Otro cmdlet que puede necesitar es **Initialize-PmemPhysicalDevice**, que inicializará el área de almacenamiento de etiqueta en los dispositivos de memoria persistente física. Se puede usar para borrar información de almacenamiento de etiquetas dañadas en los dispositivos de memoria persistentes.

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

Es importante tener en cuenta que este comando debe usarse como último recurso para corregir problemas relacionados con la memoria persistente. Se producirá una pérdida de datos en la memoria persistente.

## <a name="see-also"></a>Vea también

- [Información general de Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
- [Administración de estado de memoria de clase de almacenamiento (NVDIMM-N) en Windows](storage-class-memory-health.md)
- [Descripción de la memoria caché](understand-the-cache.md)
