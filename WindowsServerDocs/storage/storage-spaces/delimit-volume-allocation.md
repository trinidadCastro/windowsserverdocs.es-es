---
title: Delimite la asignación de volúmenes en Espacios de almacenamiento directo
ms.author: cosmosdarwin
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/29/2018
ms.openlocfilehash: faf9547833764e9075e86515d1f486a5a3f61ff8
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872081"
---
# <a name="delimit-the-allocation-of-volumes-in-storage-spaces-direct"></a>Delimite la asignación de volúmenes en Espacios de almacenamiento directo
> Se aplica a: Windows Server 2019

Windows Server 2019 presenta una opción para delimitar manualmente la asignación de volúmenes en Espacios de almacenamiento directo. Esto puede aumentar significativamente la tolerancia a errores en determinadas condiciones, pero impone algunas consideraciones y complejidad de administración agregadas. En este tema se explica cómo funciona y se proporcionan ejemplos de PowerShell.

   > [!IMPORTANT]
   > Esta característica es nueva en Windows Server 2019. No está disponible en Windows Server 2016. 

## <a name="prerequisites"></a>Requisitos previos

### <a name="green-checkmark-iconmediadelimit-volume-allocationsupportedpng-consider-using-this-option-if"></a>![Icono de marca de verificación verde.](media/delimit-volume-allocation/supported.png) Considere la posibilidad de usar esta opción si:

- El clúster tiene seis o más servidores; etc
- El clúster solo usa resistencia [de reflejo triple](storage-spaces-fault-tolerance.md#mirroring)

### <a name="red-x-iconmediadelimit-volume-allocationunsupportedpng-do-not-use-this-option-if"></a>![Icono X de color rojo.](media/delimit-volume-allocation/unsupported.png) No use esta opción si:

- El clúster tiene menos de seis servidores; de
- El [clúster utiliza la](storage-spaces-fault-tolerance.md#parity) resistencia de paridad de paridad o [de reflejos](storage-spaces-fault-tolerance.md#mirror-accelerated-parity)

## <a name="understand"></a>Comprender

### <a name="review-regular-allocation"></a>Revisión: asignación normal

Con la creación de reflejos tridireccionales normal, el volumen se divide en muchos "bloques" pequeños que se copian tres veces y se distribuyen uniformemente en todas las unidades de cada servidor del clúster. Para obtener más información, lea [este blog en profundidad](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/).

![Diagrama que muestra el volumen que se divide en tres pilas de bloques y se distribuye uniformemente en cada servidor.](media/delimit-volume-allocation/regular-allocation.png)

Esta asignación predeterminada maximiza las lecturas y escrituras en paralelo, lo que conduce a un mejor rendimiento y es atractivo en su simplicidad: cada servidor está igualmente ocupado, cada unidad está igualmente llena y todos los volúmenes permanecen en línea o sin conexión juntos. Se garantiza que todos los volúmenes sobreviven a dos errores simultáneos, como se muestra en [estos ejemplos](storage-spaces-fault-tolerance.md#examples) .

Sin embargo, con esta asignación, los volúmenes no pueden sobrevivir a tres errores simultáneos. Si se produce un error en tres servidores a la vez, o si las unidades de tres servidores producen errores al mismo tiempo, los volúmenes se vuelven inaccesibles porque al menos algunos bloques eran (con una probabilidad muy alta) asignados a las tres unidades o servidores exactos con errores.

En el ejemplo siguiente, los servidores 1, 3 y 5 generan un error al mismo tiempo. Aunque muchos bloques tienen copias supervivientes, algunos no:

![Diagrama que muestra tres de los seis servidores resaltados en rojo y el volumen total es rojo.](media/delimit-volume-allocation/regular-does-not-survive.png)

El volumen se queda sin conexión y deja de estar accesible hasta que se recuperan los servidores.

### <a name="new-delimited-allocation"></a>Nuevo: asignación delimitada

Con la asignación delimitada, se especifica un subconjunto de servidores que se usarán (mínimo tres para reflejo triple). El volumen se divide en bloques que se copian tres veces, como antes, pero en lugar de asignarse a todos los servidores, **los bloques solo se asignan al subconjunto de servidores que especifique**.

![Diagrama que muestra el volumen que se divide en tres pilas de bloques y se distribuye solo a tres de seis servidores.](media/delimit-volume-allocation/delimited-allocation.png)

#### <a name="advantages"></a>Ventajas

Con esta asignación, es probable que el volumen sobreviva a tres errores simultáneos: de hecho, su probabilidad de supervivencia aumenta desde 0% (con asignación normal) hasta 95% (con asignación delimitada) en este caso. De forma intuitiva, esto se debe a que no depende de los servidores 4, 5 o 6, por lo que no se ve afectado por los errores.

En el ejemplo anterior, los servidores 1, 3 y 5 generan un error al mismo tiempo. Dado que la asignación delimitada garantiza que el servidor 2 contiene una copia de cada losa, cada losa tiene una copia superviviente y el volumen permanece en línea y accesible:

![Diagrama que muestra tres de los seis servidores resaltados en rojo, pero el volumen total es verde.](media/delimit-volume-allocation/delimited-does-survive.png)

La probabilidad de supervivencia depende del número de servidores y otros factores; consulte [análisis](#analysis) para obtener más información.

#### <a name="disadvantages"></a>Desventajas

La asignación delimitada impone algunas consideraciones y complejidad de administración agregadas:

1. El administrador es responsable de delimitar la asignación de cada volumen para equilibrar el uso de almacenamiento en los servidores y mantener una probabilidad alta de supervivencia, tal como se describe en la sección de [procedimientos](#best-practices) recomendados.

2. Con la asignación delimitada, Reserve el equivalente de **una unidad de capacidad por servidor (sin máximo)** . Esto es algo más que la [recomendación publicada](plan-volumes.md#choosing-the-size-of-volumes) para la asignación normal, que se llegar al máximo en un total de cuatro unidades de capacidad.

3. Si se produce un error en un servidor y es necesario reemplazarlo, tal y como se describe en [quitar un servidor y sus unidades](remove-servers.md#remove-a-server-and-its-drives), el administrador es responsable de actualizar la delimitación de los volúmenes afectados agregando el nuevo servidor y quitando el error uno de los siguientes: ejemplo.

## <a name="usage-in-powershell"></a>Uso en PowerShell

Puede usar el `New-Volume` cmdlet para crear volúmenes en espacios de almacenamiento directo.

Por ejemplo, para crear un volumen de reflejo triple normal:

```PowerShell
New-Volume -FriendlyName "MyRegularVolume" -Size 100GB
```

### <a name="create-a-volume-and-delimit-its-allocation"></a>Crear un volumen y delimitar su asignación

Para crear un volumen de reflejo triple y delimitar su asignación:

1. En primer lugar, asigne los servidores del clúster a `$Servers`la variable:

    ```PowerShell
    $Servers = Get-StorageFaultDomain -Type StorageScaleUnit | Sort FriendlyName
    ```

   > [!TIP]
   > En Espacios de almacenamiento directo, el término "unidad de escala de almacenamiento" hace referencia a todo el almacenamiento sin procesar conectado a un servidor, incluidas las unidades conectadas directamente y los alojamientos externos con conexión directa con las unidades. En este contexto, es lo mismo que ' Server '.

2. Especifique los servidores que se van a usar `-StorageFaultDomainsToUse` con el nuevo parámetro e indexando en. `$Servers` Por ejemplo, para delimitar la asignación al primer, segundo y tercer servidor (índices 0, 1 y 2):

    ```PowerShell
    New-Volume -FriendlyName "MyVolume" -Size 100GB -StorageFaultDomainsToUse $Servers[0,1,2]
    ```

### <a name="see-a-delimited-allocation"></a>Ver una asignación delimitada

Para ver cómo se asigna el *volumen* , use el `Get-VirtualDiskFootprintBySSU.ps1` script del [Apéndice](#appendix):

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         100 GB  100 GB  100 GB  0       0       0      
```

Tenga en cuenta que solo server1, server2 y Server3 contienen bloques de mi *volumen*.

### <a name="change-a-delimited-allocation"></a>Cambiar una asignación delimitada

Use los cmdlets `Remove-StorageFaultDomain` New `Add-StorageFaultDomain` y para cambiar cómo se delimita la asignación.

Por ejemplo, para subir un *volumen* en un servidor:

1. Especifica que el cuarto servidor **puede** almacenar los bloques de mi *volumen*:

    ```PowerShell
    Get-VirtualDisk MyVolume | Add-StorageFaultDomain -StorageFaultDomains $Servers[3]
    ```

2. Especifica que el primer servidor **no puede** almacenar los bloques de mi *volumen*:

    ```PowerShell
    Get-VirtualDisk MyVolume | Remove-StorageFaultDomain -StorageFaultDomains $Servers[0]
    ```

3. Reequilibrar el grupo de almacenamiento para que el cambio surta efecto:

    ```PowerShell
    Get-StoragePool S2D* | Optimize-StoragePool
    ```

![Diagrama que muestra los bloques migre en masa desde los servidores 1, 2 y 3 a los servidores 2, 3 y 4.](media/delimit-volume-allocation/move.gif)

Puede supervisar el progreso del reequilibrio con `Get-StorageJob`.

Una vez que haya finalizado, Compruebe que se ha deshecho `Get-VirtualDiskFootprintBySSU.ps1` la ejecución de mi volumen.

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         0       100 GB  100 GB  100 GB  0       0      
```

Tenga en cuenta que server1 ya no contiene bloques de mi *volumen* ; en su lugar, Server04 sí.

## <a name="best-practices"></a>Procedimientos recomendados

Estos son los procedimientos recomendados que se deben seguir al usar la asignación de volúmenes delimitados:

### <a name="choose-three-servers"></a>Elegir tres servidores

Delimite cada volumen de reflejo triple en tres servidores, no más.

### <a name="balance-storage"></a>Equilibrar el almacenamiento

Equilibre la cantidad de almacenamiento que se asigna a cada servidor, teniendo en cuenta el tamaño del volumen.

### <a name="every-delimited-allocation-unique"></a>Cada asignación delimitada es única

Para maximizar la tolerancia a errores, haga que la asignación de cada volumen sea única, lo que significa que no comparte *todos* sus servidores con otro volumen (la superposición es correcta). Con N servidores, hay "N elección 3" combinaciones únicas; esto es lo que significa para algunos tamaños de clúster comunes:

| Número de servidores (N) | Número de asignaciones delimitadas únicas (N seleccione 3) |
|-----------------------|-----------------------------------------------------|
| 6                     | 20                                                  |
| 8                     | 56                                                  |
| 12                    | 220                                                 |
| 16                    | 560                                                 |

   > [!TIP]
   > Tenga en cuenta esta revisión útil de [Combinatorics y elija notación](https://betterexplained.com/articles/easy-permutations-and-combinations/).

Este es un ejemplo que maximiza la tolerancia a errores: cada volumen tiene una asignación delimitada única:

![asignación única](media/delimit-volume-allocation/unique-allocation.png)

Por el contrario, en el ejemplo siguiente, los tres primeros volúmenes usan la misma asignación delimitada (a los servidores 1, 2 y 3) y los últimos tres volúmenes usan la misma asignación delimitada (a los servidores 4, 5 y 6). Esto no maximiza la tolerancia a errores: si se produce un error en tres servidores, varios volúmenes podrían desconectarse y dejar de estar accesible a la vez.

![asignación no única](media/delimit-volume-allocation/non-unique-allocation.png)

## <a name="analysis"></a>Analizar

En esta sección se deriva la probabilidad matemática de que un volumen permanezca en línea y accesible (o equivalente, la fracción esperada de almacenamiento general que permanece en línea y accesible) como función del número de errores y del tamaño del clúster.

   > [!NOTE]
   > Esta sección es de lectura opcional. Si le interesa ver las matemáticas, siga leyendo. Pero si no es así, no se preocupe: El [uso de PowerShell](#usage-in-powershell) y los [procedimientos recomendados](#best-practices) es todo lo que necesita para implementar correctamente la asignación delimitada.

### <a name="up-to-two-failures-is-always-okay"></a>Un máximo de dos errores siempre es correcto.

Cada volumen de reflejo triple puede sobrevivir a dos errores al mismo tiempo, como muestran [estos ejemplos](storage-spaces-fault-tolerance.md#examples) , independientemente de su asignación. Si se produce un error en dos unidades, o se produce un error en dos servidores, o uno de cada, cada volumen de reflejo triple permanece en línea y accesible, incluso con la asignación normal.

### <a name="more-than-half-the-cluster-failing-is-never-okay"></a>Más de la mitad del error del clúster nunca está bien

Por el contrario, en el caso extremo en el que se produce un error en una vez más de la mitad de servidores o unidades del clúster, el [cuórum se pierde](understand-quorum.md) y cada volumen de reflejo triple se queda sin conexión y deja de estar accesible, independientemente de su asignación.

### <a name="what-about-in-between"></a>¿Qué ocurre en entre?

Si se producen tres o más errores al mismo tiempo, pero al menos la mitad de los servidores y las unidades de discos siguen funcionando, los volúmenes con asignación delimitada pueden permanecer en línea y accesibles, en función de los servidores que tengan errores. Vamos a ejecutar los números para determinar las probabilidades precisas.

Por motivos de simplicidad, supongamos que los volúmenes se distribuyen de manera independiente y idéntica (IID) según las prácticas recomendadas anteriores, y que hay suficientes combinaciones únicas disponibles para que la asignación de cada volumen sea única. La probabilidad de que un volumen determinado sobreviva sea también la fracción esperada de almacenamiento general que sobrevive por la linealidad de la expectativa. 

Dados **N** servidores de los que **F** tienen errores, un volumen asignado a **3** de ellos se queda sin conexión solo si los **tres** están entre **f** con errores. Hay **(N Choose f)** para que se produzcan errores de **F** , de los cuales **(F Choose 3)** hacen que el volumen quede sin conexión y se vuelva inaccesible. La probabilidad se puede expresar de la siguiente manera:

![P_offline = Fc3/NcF](media/delimit-volume-allocation/probability-volume-offline.png)

En todos los demás casos, el volumen permanece en línea y accesible:

![P_online = 1 – (Fc3/NcF)](media/delimit-volume-allocation/probability-volume-online.png)

En las tablas siguientes se evalúa la probabilidad de que se produzcan algunos tamaños de clúster comunes y hasta cinco errores, lo que revela que la asignación delimitada aumenta la tolerancia a errores en comparación con la asignación normal en todos los casos considerados.

### <a name="with-6-servers"></a>Con 6 servidores

| Asignación                           | Probabilidad de supervivencia 1 error | Probabilidad de supervivencia de 2 errores | Probabilidad de supervivencia de 3 errores | Probabilidad de sobrevivir a 4 errores | Probabilidad de sobrevivir a 5 errores |
|--------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Normal, repartidos por los 6 servidores | 100%                               | 100%                                | 0,1                                  | 0,1                                  | 0,1                                  |
| Solo se delimitan a 3 servidores          | 100%                               | 100%                                | 95,0%                               | 0,1                                  | 0,1                                  |

   > [!NOTE]
   > Después de más de 3 errores de 6 servidores en total, el clúster pierde el cuórum.

### <a name="with-8-servers"></a>Con 8 servidores

| Asignación                           | Probabilidad de supervivencia 1 error | Probabilidad de supervivencia de 2 errores | Probabilidad de supervivencia de 3 errores | Probabilidad de sobrevivir a 4 errores | Probabilidad de sobrevivir a 5 errores |
|--------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Normal, repartidos por los 8 servidores | 100%                               | 100%                                | 0,1                                  | 0,1                                  | 0,1                                  |
| Solo se delimitan a 3 servidores          | 100%                               | 100%                                | 98,2%                               | 94.3%                               | 0,1                                  |

   > [!NOTE]
   > Después de más de 4 errores de 8 servidores en total, el clúster pierde el cuórum.

### <a name="with-12-servers"></a>Con 12 servidores

| Asignación                            | Probabilidad de supervivencia 1 error | Probabilidad de supervivencia de 2 errores | Probabilidad de supervivencia de 3 errores | Probabilidad de sobrevivir a 4 errores | Probabilidad de sobrevivir a 5 errores |
|---------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Normal, repartidos por los 12 servidores | 100%                               | 100%                                | 0,1                                  | 0,1                                  | 0,1                                  |
| Solo se delimitan a 3 servidores           | 100%                               | 100%                                | 99,5%                               | 99,2%                               | 98,7%                               |

### <a name="with-16-servers"></a>Con 16 servidores

| Asignación                            | Probabilidad de supervivencia 1 error | Probabilidad de supervivencia de 2 errores | Probabilidad de supervivencia de 3 errores | Probabilidad de sobrevivir a 4 errores | Probabilidad de sobrevivir a 5 errores |
|---------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Normal, repartidos por los 16 servidores | 100%                               | 100%                                | 0,1                                  | 0,1                                  | 0,1                                  |
| Solo se delimitan a 3 servidores           | 100%                               | 100%                                | 99,8%                               | 99,8%                               | 99,8%                               |

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="can-i-delimit-some-volumes-but-not-others"></a>¿Puedo delimitar algunos volúmenes, pero no otros?

Sí. Puede elegir por volumen si quiere o no delimitar la asignación.

### <a name="does-delimited-allocation-change-how-drive-replacement-works"></a>¿Cambia la asignación delimitada el funcionamiento de la sustitución de la unidad?

No, es lo mismo que con la asignación normal.

## <a name="see-also"></a>Vea también

- [Información general de Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
- [Tolerancia a errores en Espacios de almacenamiento directo](storage-spaces-fault-tolerance.md)

## <a name="appendix"></a>Anexo

Este script le ayuda a ver cómo se asignan los volúmenes.

Para usarlo como se describió anteriormente, Copie/pegue y guarde `Get-VirtualDiskFootprintBySSU.ps1`como.

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
