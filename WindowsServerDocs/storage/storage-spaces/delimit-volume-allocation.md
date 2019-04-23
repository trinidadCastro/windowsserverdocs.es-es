---
title: Delimitar la asignación de volúmenes en espacios de almacenamiento directo
ms.author: cosmosdarwin
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/29/2018
ms.openlocfilehash: c93cbf4ba418f6702cf8747508605952d993508d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889056"
---
# <a name="delimit-the-allocation-of-volumes-in-storage-spaces-direct"></a>Delimitar la asignación de volúmenes en espacios de almacenamiento directo
> Se aplica a: Windows Server Insider Preview, compilación 17093 y versiones posterior

Windows Server Insider Preview incluye una opción para delimitar manualmente la asignación de volúmenes en espacios de almacenamiento directo. Si lo hace por lo que puede aumentar considerablemente la tolerancia a errores en determinadas condiciones, pero impone algunas consideraciones sobre la administración se ha agregado y la complejidad. En este tema se explica cómo funciona y ofrece ejemplos de PowerShell.

   > [!IMPORTANT]
   > Esta característica es nueva en Windows Server Insider Preview, compilación 17093 y versiones posterior. No está disponible en Windows Server 2016. Le invitamos a unirse a los profesionales de TI la [Windows Server Insider programa](https://aka.ms/serverinsider) para proporcionarnos sus comentarios sobre lo que podemos hacer para que Windows Server funcione mejor para su organización.

## <a name="prerequisites"></a>Requisitos previos

### <a name="green-checkmark-iconmediadelimit-volume-allocationsupportedpng-consider-using-this-option-if"></a>![Icono de marca de verificación verde.](media/delimit-volume-allocation/supported.png) Considere el uso de esta opción si:

- El clúster tiene seis o más servidores; y
- El clúster solo usa [triple](storage-spaces-fault-tolerance.md#mirroring) resistencia

### <a name="red-x-iconmediadelimit-volume-allocationunsupportedpng-do-not-use-this-option-if"></a>![Icono X rojo.](media/delimit-volume-allocation/unsupported.png) No use esta opción si:

- El clúster tiene menos de seis servidores; o
- El clúster usa [paridad](storage-spaces-fault-tolerance.md#parity) o [acelerada reflejado paridad](storage-spaces-fault-tolerance.md#mirror-accelerated-parity) resistencia

## <a name="understand"></a>Comprender

### <a name="review-regular-allocation"></a>Revisión: asignación normal

Con reflejo triple regular, el volumen se divide en muchos pequeños "bloques" que se copian tres veces y se distribuyen uniformemente en cada unidad en todos los servidores del clúster. Para obtener más información, lea [esta entrada de blog de profundización](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/).

![Diagrama que muestra el volumen que se divide en tres pilas de bloques y distribuyen uniformemente entre todos los servidores.](media/delimit-volume-allocation/regular-allocation.png)

Esta asignación predeterminada maximiza paralelo lee y escribe, dando lugar a un mejor rendimiento y es atractiva en su simplicidad: cada servidor está ocupado por igual, cada unidad es igualmente completa y todos los volúmenes permanecen en línea o se desconectan entre sí. Se garantiza que todos los volúmenes para sobrevivir a un máximo de dos errores simultáneos, como [estos ejemplos](storage-spaces-fault-tolerance.md#examples) ilustrar.

Sin embargo, con esta asignación, los volúmenes no pueden sobrevivir tres errores simultáneos. Si tres servidores, se producen errores al mismo tiempo, o si las unidades en tres servidores producirá un error al mismo tiempo, los volúmenes inaccesible porque al menos algunos bloques (con una probabilidad muy alta) asignados al exactamente tres unidades o los servidores que no se pudo.

En el ejemplo siguiente, los servidores de 1, 3 y 5 producirá un error al mismo tiempo. Aunque muchos bloques tienen sobrevivir copias, otros no:

![Diagrama que muestra tres de seis servidores resaltados en rojo y el volumen global está en rojo.](media/delimit-volume-allocation/regular-does-not-survive.png)

El volumen se queda sin conexión y deja de estar accesible hasta que se recuperan los servidores.

### <a name="new-delimited-allocation"></a>Novedad: delimitado por asignación

Con la asignación delimitada, especifique un subconjunto de servidores para usar (mínimo tres para reflejo triple). El volumen se divide en bloques que se copian tres veces, como antes, pero en lugar de asignar a través de todos los servidores, **los bloques se asignan sólo al subconjunto de servidores que especifique**.

![Diagrama que muestra el volumen que se divide en tres pilas de bloques y distribuido solo a tres de seis servidores.](media/delimit-volume-allocation/delimited-allocation.png)

#### <a name="advantages"></a>Ventajas

Con esta asignación, el volumen es probable que sobreviven a tres errores simultáneos: de hecho, la probabilidad de supervivencia aumenta de 0% (con asignación normal) al 95% (con asignación delimitado) en este caso. Forma intuitiva, esto es porque no depende de los servidores de 4, 5 o 6 por lo que no se ve afectada por los errores.

En el ejemplo anterior, los servidores de 1, 3 y 5 producirá un error al mismo tiempo. Dado que delimitado asignación garantiza que el servidor 2 contiene una copia de cada cemento, cada sección tiene una copia que permanece y el volumen permanece en línea y accesibles:

![Diagrama que muestra tres de seis servidores resaltados en rojo, sin embargo, el volumen global es verde.](media/delimit-volume-allocation/delimited-does-survive.png)

Probabilidad de supervivencia depende del número de servidores y otros factores – vea [Analysis](#analysis) para obtener más información.

#### <a name="disadvantages"></a>Desventajas

Asignación delimitado impone algunas consideraciones sobre la administración se ha agregado y la complejidad:

1. El administrador es responsable de delimitación de la asignación de cada volumen para equilibrar el uso del almacenamiento en servidores y mantener la alta probabilidad de supervivencia, como se describe en el [procedimientos recomendados](#best-practices) sección.

2. Con la asignación delimitada, reservar el equivalente de **unidad de capacidad de una por servidor (con ningún máximo)**. Esto es más que [publicado la recomendación](plan-volumes.md#choosing-the-size-of-volumes) para la asignación normal, que usa al máximo la capacidad de cuatro unidades en total.

3. Si un servidor se produce un error y debe reemplazarse, como se describe en [quitar un servidor y sus unidades](remove-servers.md#remove-a-server-and-its-drives), el administrador es responsable de actualizar la delimitación de los volúmenes afectados al agregar el nuevo servidor y quitar el error: ejemplo a continuación.

## <a name="usage-in-powershell"></a>Uso en PowerShell

Puede usar el `New-Volume` para crear volúmenes en espacios de almacenamiento directo.

Por ejemplo, para crear un volumen normal de triple:

```PowerShell
New-Volume -FriendlyName "MyRegularVolume" -Size 100GB
```

### <a name="create-a-volume-and-delimit-its-allocation"></a>Crear un volumen y delimitar su asignación

Para crear un volumen reflejado de tres vías y delimitar la asignación:

1. Asignar a los servidores del clúster a la variable `$Servers`:

    ```PowerShell
    $Servers = Get-StorageFaultDomain -Type StorageScaleUnit | Sort FriendlyName
    ```

   > [!TIP]
   > Espacios de almacenamiento directo, el término 'Unidad de escalado de almacenamiento' hace referencia a todo el almacenamiento sin formato asociado a un servidor, incluidas unidades de conexión directa y conexión directa alojamientos externos con unidades. En este contexto, es igual que 'server'.

2. Especificar qué servidores para usar con el nuevo `-StorageFaultDomainsToUse` parámetro y mediante la indización en `$Servers`. Por ejemplo, para delimitar la asignación a la primera, segunda y terceros servidores (índices 0, 1 y 2):

    ```PowerShell
    New-Volume -FriendlyName "MyVolume" -Size 100GB -StorageFaultDomainsToUse $Servers[0,1,2]
    ```

### <a name="see-a-delimited-allocation"></a>Vea una asignación delimitada

Para ver cómo *MyVolume* está asignado, use el `Get-VirtualDiskFootprintBySSU.ps1` crear scripts de [apéndice](#appendix):

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         100 GB  100 GB  100 GB  0       0       0      
```

Tenga en cuenta que solo Servidor1, servidor2 y Server3 contiene bloques de *MyVolume*.

### <a name="change-a-delimited-allocation"></a>Cambiar una asignación delimitada

Use la nueva `Add-StorageFaultDomain` y `Remove-StorageFaultDomain` para cambiar cómo se delimita la asignación.

Por ejemplo, para mover *MyVolume* un servidor de la tecla TAB:

1. Especificar que el servidor cuarto **puede** almacenar bloques de *MyVolume*:

    ```PowerShell
    Get-VirtualDisk MyVolume | Add-StorageFaultDomain -StorageFaultDomains $Servers[3]
    ```

2. Especificar que el primer servidor **no** almacenar bloques de *MyVolume*:

    ```PowerShell
    Get-VirtualDisk MyVolume | Remove-StorageFaultDomain -StorageFaultDomains $Servers[0]
    ```

3. Volver a equilibrar el bloque de almacenamiento para que el cambio surta efecto:

    ```PowerShell
    Get-StoragePool S2D* | Optimize-StoragePool
    ```

![Diagrama que muestra que los bloques de migración en masa de los servidores de 1, 2 y 3 servidores 2, 3 y 4.](media/delimit-volume-allocation/move.gif)

Puede supervisar el progreso de la reequilibrio con `Get-StorageJob`.

Una vez completado, compruebe que *MyVolume* mediante la ejecución se ha movido `Get-VirtualDiskFootprintBySSU.ps1` nuevo.

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         0       100 GB  100 GB  100 GB  0       0      
```

Tenga en cuenta que no contienen Server1 desbastes planos de *MyVolume* ya: en su lugar, Server04.

## <a name="best-practices"></a>Procedimiento recomendado

Estos son los procedimientos recomendados para seguir al usar delimitados por la asignación de volúmenes:

### <a name="choose-three-servers"></a>Elija los tres servidores

Delimitar cada volumen triple para los tres servidores, no más.

### <a name="balance-storage"></a>Equilibrar el almacenamiento

Equilibrar la cantidad de almacenamiento se asigna a cada servidor, teniendo en cuenta el tamaño de volumen.

### <a name="every-delimited-allocation-unique"></a>Cada asignación delimitado único

Para maximizar la tolerancia a errores, asegúrese de asignación de cada volumen único, lo que significa no comparte *todas* sus servidores con otro volumen (cierta superposición es correcto). Con servidores de N, hay combinaciones únicas de "N Elija 3": aquí es lo que significa para algunos tamaños de clúster comunes:

| Número de servidores (N) | Número del único delimitado asignaciones (N Elija 3) |
|-----------------------|-----------------------------------------------------|
| 6                     | 20                                                  |
| 8                     | 56                                                  |
| 12                    | 220                                                 |
| 16                    | 560                                                 |

   > [!TIP]
   > Tenga en cuenta esta revisión útil de [combinatorics y elija notación](https://betterexplained.com/articles/easy-permutations-and-combinations/).

Este es un ejemplo que maximiza la tolerancia a errores, cada volumen tiene una única asignación delimitada:

![unique-allocation](media/delimit-volume-allocation/unique-allocation.png)

Por el contrario, en el ejemplo siguiente, los tres primeros volúmenes usan la misma asignación delimitada (para servidores de 1, 2 y 3) y los últimos tres volúmenes usan la misma asignación delimitada (para servidores de 4, 5 y 6). Esto no maximizar la tolerancia a errores: si se producen errores en tres servidores, varios volúmenes se quedan sin conexión y dejan de estar accesibles a la vez.

![no único-asignación](media/delimit-volume-allocation/non-unique-allocation.png)

## <a name="analysis"></a>Análisis

En esta sección se deriva la probabilidad matemática que un volumen permanece en línea y accesibles (o forma equivalente, la fracción esperada de almacenamiento general que permanece en línea y accesibles) como una función del número de errores y el tamaño del clúster.

   > [!NOTE]
   > Esta sección es opcional de lectura. Si está ansioso por ver las matemáticas, siga leyendo. Pero si no, no se preocupe: [Uso de PowerShell](#usage-in-powershell) y [procedimientos recomendados](#best-practices) es todo lo que necesita para implementar la asignación delimitado correctamente.

### <a name="up-to-two-failures-is-always-okay"></a>Hasta dos errores siempre es correcto

Todos los volúmenes reflejados tridireccionales pueden sobrevivir a fallos de hasta dos al mismo tiempo, como [estos ejemplos](storage-spaces-fault-tolerance.md#examples) ilustrar, independientemente de su asignación. Si se producirá un error en dos unidades, producirá un error de dos servidores o uno de cada, cada volumen triple permanece en línea y accesibles, incluso con la asignación normal.

### <a name="more-than-half-the-cluster-failing-is-never-okay"></a>Error de más de la mitad del clúster nunca es correcto

Por el contrario, en el caso extremo que más de la mitad de los servidores o las unidades en el clúster al mismo tiempo, no [se perdió el quórum](understand-quorum.md) y todos los volúmenes reflejados tridireccionales se queda sin conexión y deja de estar accesible, independientemente de su asignación.

### <a name="what-about-in-between"></a>¿Qué sucede entre?

Si se producen errores de tres o más a la vez, pero al menos la mitad de los servidores y las unidades están aún activos, los volúmenes con asignación delimitado pueden permanecer en línea y accesibles, dependiendo de qué servidores tienen errores. Vamos a ejecutar los números para determinar las probabilidades precisas.

Por motivos de simplicidad, suponen los volúmenes son por separado y distribuido de forma idéntica (IID) según las recomendaciones anteriores y ese combinaciones únicas suficientemente están disponibles para la asignación de cada volumen que sea único. La probabilidad de que sobrevive cualquier volumen determinado es también la fracción de almacenamiento general que sobrevive a linealidad de expectativa esperada. 

Dado **N** servidores de los cuales **F** tienen errores, un volumen asignado a **3** de ellos se queda sin conexión if-y-solo-if todas **3** se encuentran entre la  **F** con errores. Hay **(N Elija F)** formas para **F** errores que se produzca, de los cuales **(F elegir 3)** como resultado el volumen está quedándose sin conexión y se está convirtiendo en inaccesible. La probabilidad se puede expresar como:

![P_offline = Fc3 / NcF](media/delimit-volume-allocation/probability-volume-offline.png)

En todos los demás casos, el volumen permanece en línea y accesibles:

![P_online = 1 – (Fc3 / NcF)](media/delimit-volume-allocation/probability-volume-online.png)

Las siguientes tablas evalúan la probabilidad para algunos tamaños de clúster comunes y un máximo de 5 errores, revela que la asignación delimitado aumenta tolerancia a errores en comparación con una asignación normal en todos los casos considera.

### <a name="with-6-servers"></a>Con 6 servidores

| Asignación                           | Probabilidad de sobrevivir a 1 error | Probabilidad de superación de 2 errores | Probabilidad de superación de 3 errores | Probabilidad de superación de 4 errores | Probabilidad de superación de 5 errores |
|--------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Regular, se reparten entre 6 todos los servidores | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| Delimitado en solo 3 servidores          | 100%                               | 100%                                | 95.0%                               | 0%                                  | 0%                                  |

   > [!NOTE]
   > Después de más de 3 errores fuera de 6 total de servidores, el clúster pierde el quórum.

### <a name="with-8-servers"></a>Con 8 servidores

| Asignación                           | Probabilidad de sobrevivir a 1 error | Probabilidad de superación de 2 errores | Probabilidad de superación de 3 errores | Probabilidad de superación de 4 errores | Probabilidad de superación de 5 errores |
|--------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Regular, se reparten entre todos los servidores de 8 | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| Delimitado en solo 3 servidores          | 100%                               | 100%                                | 98.2%                               | 94.3%                               | 0%                                  |

   > [!NOTE]
   > Después de más de 4 errores fuera 8 servidores total, el clúster pierde el quórum.

### <a name="with-12-servers"></a>Con 12 servidores

| Asignación                            | Probabilidad de sobrevivir a 1 error | Probabilidad de superación de 2 errores | Probabilidad de superación de 3 errores | Probabilidad de superación de 4 errores | Probabilidad de superación de 5 errores |
|---------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Regular, se reparten entre 12 todos los servidores | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| Delimitado en solo 3 servidores           | 100%                               | 100%                                | 99.5%                               | 99.2%                               | 98.7%                               |

### <a name="with-16-servers"></a>Con 16 servidores

| Asignación                            | Probabilidad de sobrevivir a 1 error | Probabilidad de superación de 2 errores | Probabilidad de superación de 3 errores | Probabilidad de superación de 4 errores | Probabilidad de superación de 5 errores |
|---------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Regular, se reparten entre 16 todos los servidores | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| Delimitado en solo 3 servidores           | 100%                               | 100%                                | 99.8%                               | 99.8%                               | 99.8%                               |

## <a name="frequently-asked-questions"></a>Preguntas frecuentes

### <a name="can-i-delimit-some-volumes-but-not-others"></a>¿Se puede delimitar algunos volúmenes pero no para otras?

Sí. Puede elegir por volumen si se deben delimitar la asignación.

### <a name="does-delimited-allocation-change-how-drive-replacement-works"></a>¿Asignación delimitado cambia cómo funciona la sustitución de la unidad?

No, es el mismo que con la asignación normal.

## <a name="see-also"></a>Vea también

- [Información general de espacios directo de almacenamiento](storage-spaces-direct-overview.md)
- [Tolerancia a errores en los espacios de almacenamiento directo](storage-spaces-fault-tolerance.md)

## <a name="appendix"></a>Apéndice

Este script le permite ver cómo se asignan los volúmenes.

Para usarla como se describió anteriormente, copiar y pegar y guardar como `Get-VirtualDiskFootprintBySSU.ps1`.

```PowerShell
Function ConvertTo-PrettyCapacity {
    Param (
        [Parameter(
            Mandatory = $True,
            ValueFromPipeline = $True
            )
        ]
    [Int64]$Bytes,
    [Int64]$RoundTo = 0
    )
    If ($Bytes -Gt 0) {
        $Base = 1024
        $Labels = ("bytes", "KB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB")
        $Order = [Math]::Floor( [Math]::Log($Bytes, $Base) )
        $Rounded = [Math]::Round($Bytes/( [Math]::Pow($Base, $Order) ), $RoundTo)
        [String]($Rounded) + " " + $Labels[$Order]
    }
    Else {
        "0"
    }
    Return
}

Function Get-VirtualDiskFootprintByStorageFaultDomain {

    ################################################
    ### Step 1: Gather Configuration Information ###
    ################################################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Gathering configuration information..." -Status "Step 1/4" -PercentComplete 00

    $ErrorCannotGetCluster = "Cannot proceed because 'Get-Cluster' failed."
    $ErrorNotS2DEnabled = "Cannot proceed because the cluster is not running Storage Spaces Direct."
    $ErrorCannotGetClusterNode = "Cannot proceed because 'Get-ClusterNode' failed."
    $ErrorClusterNodeDown = "Cannot proceed because one or more cluster nodes is not Up."
    $ErrorCannotGetStoragePool = "Cannot proceed because 'Get-StoragePool' failed."
    $ErrorPhysicalDiskFaultDomainAwareness = "Cannot proceed because the storage pool is set to 'PhysicalDisk' fault domain awareness. This cmdlet only supports 'StorageScaleUnit', 'StorageChassis', or 'StorageRack' fault domain awareness."

    Try  {
        $GetCluster = Get-Cluster -ErrorAction Stop
    }
    Catch {
        throw $ErrorCannotGetCluster
    }

    If ($GetCluster.S2DEnabled -Ne 1) {
        throw $ErrorNotS2DEnabled
    }

    Try  {
        $GetClusterNode = Get-ClusterNode -ErrorAction Stop
    }
    Catch {
        throw $ErrorCannotGetClusterNode
    }

    If ($GetClusterNode | Where State -Ne Up) {
        throw $ErrorClusterNodeDown
    }

    Try {
        $GetStoragePool = Get-StoragePool -IsPrimordial $False -ErrorAction Stop
    }
    Catch {
        throw $ErrorCannotGetStoragePool
    }

    If ($GetStoragePool.FaultDomainAwarenessDefault -Eq "PhysicalDisk") {
        throw $ErrorPhysicalDiskFaultDomainAwareness
    }

    ###########################################################
    ### Step 2: Create SfdList[] and PhysicalDiskToSfdMap{} ###
    ###########################################################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Analyzing physical disk information..." -Status "Step 2/4" -PercentComplete 25

    $SfdList = Get-StorageFaultDomain -Type ($GetStoragePool.FaultDomainAwarenessDefault) | Sort FriendlyName # StorageScaleUnit, StorageChassis, or StorageRack

    $PhysicalDiskToSfdMap = @{} # Map of PhysicalDisk.UniqueId -> StorageFaultDomain.FriendlyName
    $SfdList | ForEach {
        $StorageFaultDomain = $_
        $_ | Get-StorageFaultDomain -Type PhysicalDisk | ForEach {
            $PhysicalDiskToSfdMap[$_.UniqueId] = $StorageFaultDomain.FriendlyName
        }
    }

    ##################################################################################################
    ### Step 3: Create VirtualDisk.FriendlyName -> { StorageFaultDomain.FriendlyName -> Size } Map ###
    ##################################################################################################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Analyzing virtual disk information..." -Status "Step 3/4" -PercentComplete 50

    $GetVirtualDisk = Get-VirtualDisk | Sort FriendlyName

    $VirtualDiskMap = @{}

    $GetVirtualDisk | ForEach {
        # Map of PhysicalDisk.UniqueId -> Size for THIS virtual disk
        $PhysicalDiskToSizeMap = @{}
        $_ | Get-PhysicalExtent | ForEach {
            $PhysicalDiskToSizeMap[$_.PhysicalDiskUniqueId] += $_.Size
        }
        # Map of StorageFaultDomain.FriendlyName -> Size for THIS virtual disk
        $SfdToSizeMap = @{}
        $PhysicalDiskToSizeMap.keys | ForEach {
            $SfdToSizeMap[$PhysicalDiskToSfdMap[$_]] += $PhysicalDiskToSizeMap[$_]
        }
        # Store
        $VirtualDiskMap[$_.FriendlyName] = $SfdToSizeMap
    }

    #########################
    ### Step 4: Write-Out ###
    #########################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Formatting output..." -Status "Step 4/4" -PercentComplete 75

    $Output = $GetVirtualDisk | ForEach {
        $Row = [PsCustomObject]@{}

        $VirtualDiskFriendlyName = $_.FriendlyName
        $Row | Add-Member -MemberType NoteProperty "VirtualDiskFriendlyName" $VirtualDiskFriendlyName

        $TotalFootprint = $_.FootprintOnPool | ConvertTo-PrettyCapacity
        $Row | Add-Member -MemberType NoteProperty "TotalFootprint" $TotalFootprint

        $SfdList | ForEach {
            $Size = $VirtualDiskMap[$VirtualDiskFriendlyName][$_.FriendlyName] | ConvertTo-PrettyCapacity
            $Row | Add-Member -MemberType NoteProperty $_.FriendlyName $Size
        }

        $Row
    }

    # Calculate width, in characters, required to Format-Table
    $RequiredWindowWidth = ("TotalFootprint").length + 1 + ("VirtualDiskFriendlyName").length + 1
    $SfdList | ForEach {
        $RequiredWindowWidth += $_.FriendlyName.Length + 1
    }

    $ActualWindowWidth = (Get-Host).UI.RawUI.WindowSize.Width

    If (!($ActualWindowWidth)) {
        # Cannot get window width, probably ISE, Format-List
        Write-Warning "Could not determine window width. For the best experience, use a Powershell window instead of ISE"
        $Output | Format-Table
    }
    ElseIf ($ActualWindowWidth -Lt $RequiredWindowWidth) {
        # Narrower window, Format-List
        Write-Warning "For the best experience, try making your PowerShell window at least $RequiredWindowWidth characters wide. Current width is $ActualWindowWidth characters."
        $Output | Format-List
    }
    Else {
        # Wider window, Format-Table
        $Output | Format-Table
    }
}

Get-VirtualDiskFootprintByStorageFaultDomain
```
