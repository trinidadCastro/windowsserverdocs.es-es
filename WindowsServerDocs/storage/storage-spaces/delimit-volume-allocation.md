---
title: Delimite la asignación de volúmenes en Espacios de almacenamiento directo
manager: eldenc
ms.topic: article
author: cosmosdarwin
ms.author: cosdar
ms.date: 03/29/2018
ms.openlocfilehash: 394d9dbb41f502fe9be273e97177237dea79fde7
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078500"
---
# <a name="delimit-the-allocation-of-volumes-in-storage-spaces-direct"></a>Delimite la asignación de volúmenes en Espacios de almacenamiento directo
> Se aplica a: Windows Server 2019

Windows Server 2019 presenta una opción para delimitar manualmente la asignación de volúmenes en Espacios de almacenamiento directo. Esto puede aumentar significativamente la tolerancia a errores en determinadas condiciones, pero impone algunas consideraciones y complejidad de administración agregadas. En este tema se explica cómo funciona y se proporcionan ejemplos de PowerShell.

   > [!IMPORTANT]
   > Esta característica es nueva en Windows Server 2019. No está disponible en Windows Server 2016.

## <a name="prerequisites"></a>Prerrequisitos

### <a name="green-checkmark-icon-consider-using-this-option-if"></a>![Icono de marca de verificación verde.](media/delimit-volume-allocation/supported.png) Considere la posibilidad de usar esta opción si:

- El clúster tiene seis o más servidores; etc
- El clúster solo usa resistencia [de reflejo triple](storage-spaces-fault-tolerance.md#mirroring)

### <a name="red-x-icon-do-not-use-this-option-if"></a>![Icono X de color rojo.](media/delimit-volume-allocation/unsupported.png) No use esta opción si:

- El clúster tiene menos de seis servidores; de
- El [clúster utiliza la](storage-spaces-fault-tolerance.md#parity) resistencia de paridad de paridad o [de reflejos](storage-spaces-fault-tolerance.md#mirror-accelerated-parity)

## <a name="understand"></a>Descripción

### <a name="review-regular-allocation"></a>Revisión: asignación normal

Con la creación de reflejos tridireccionales normal, el volumen se divide en muchos "bloques" pequeños que se copian tres veces y se distribuyen uniformemente en todas las unidades de cada servidor del clúster. Para obtener más información, lea [este blog en profundidad](https://techcommunity.microsoft.com/t5/storage-at-microsoft/deep-dive-the-storage-pool-in-storage-spaces-direct/ba-p/425959).

![Diagrama que muestra el volumen que se divide en tres pilas de bloques y se distribuye uniformemente en cada servidor.](media/delimit-volume-allocation/regular-allocation.png)

Esta asignación predeterminada maximiza las lecturas y escrituras en paralelo, lo que conduce a un mejor rendimiento y es atractivo en su simplicidad: cada servidor está igualmente ocupado, cada unidad está igualmente llena y todos los volúmenes permanecen en línea o sin conexión juntos. Se garantiza que todos los volúmenes sobreviven a dos errores simultáneos, como se muestra en [estos ejemplos](storage-spaces-fault-tolerance.md#examples) .

Sin embargo, con esta asignación, los volúmenes no pueden sobrevivir a tres errores simultáneos. Si se produce un error en tres servidores a la vez, o si las unidades de tres servidores producen errores al mismo tiempo, los volúmenes se vuelven inaccesibles porque al menos algunos bloques eran (con una probabilidad muy alta) asignados a las tres unidades o servidores exactos con errores.

En el ejemplo siguiente, los servidores 1, 3 y 5 generan un error al mismo tiempo. Aunque muchos bloques tienen copias supervivientes, algunos no:

![Diagrama que muestra tres de los seis servidores resaltados en rojo y el volumen total es rojo.](media/delimit-volume-allocation/regular-does-not-survive.png)

El volumen se queda sin conexión y deja de estar accesible hasta que se recuperan los servidores.

### <a name="new-delimited-allocation"></a>Nuevo: asignación delimitada

Con la asignación delimitada, se especifica un subconjunto de servidores que se usarán (mínimo cuatro). El volumen se divide en bloques que se copian tres veces, como antes, pero en lugar de asignarse a todos los servidores, **los bloques solo se asignan al subconjunto de servidores que especifique**.

Por ejemplo, si tiene un clúster de 8 nodos (nodos del 1 al 8), puede especificar que un volumen se encuentre solo en los discos de los nodos 1, 2, 3 y 4.
#### <a name="advantages"></a>Ventajas

Con la asignación de ejemplo, es probable que el volumen sobreviva a tres errores simultáneos. Si los nodos 1, 2 y 6 dejan de funcionar, solo 2 de los nodos que contienen las 3 copias de los datos para el volumen están inactivos y el volumen permanecerá en línea.

La probabilidad de supervivencia depende del número de servidores y otros factores; consulte [análisis](#analysis) para obtener más información.

#### <a name="disadvantages"></a>Inconvenientes

La asignación delimitada impone algunas consideraciones y complejidad de administración agregadas:

1. El administrador es responsable de delimitar la asignación de cada volumen para equilibrar el uso de almacenamiento en los servidores y mantener una probabilidad alta de supervivencia, tal como se describe en la sección de [procedimientos](#best-practices) recomendados.

2. Con la asignación delimitada, Reserve el equivalente de **una unidad de capacidad por servidor (sin máximo)**. Esto es algo más que la [recomendación publicada](plan-volumes.md#choosing-the-size-of-volumes) para la asignación normal, que se llegar al máximo en un total de cuatro unidades de capacidad.

3. Si se produce un error en un servidor y es necesario reemplazarlo, tal y como se describe en [quitar un servidor y sus unidades](remove-servers.md#remove-a-server-and-its-drives), el administrador es responsable de actualizar la delimitación de los volúmenes afectados agregando el nuevo servidor y quitando el error uno de los siguientes: ejemplo.

## <a name="usage-in-powershell"></a>Uso en PowerShell

Puede usar el `New-Volume` cmdlet para crear volúmenes en espacios de almacenamiento directo.

Por ejemplo, para crear un volumen de reflejo triple normal:

```PowerShell
New-Volume -FriendlyName "MyRegularVolume" -Size 100GB
```

### <a name="create-a-volume-and-delimit-its-allocation"></a>Crear un volumen y delimitar su asignación

Para crear un volumen de reflejo triple y delimitar su asignación:

1. En primer lugar, asigne los servidores del clúster a la variable `$Servers` :

    ```PowerShell
    $Servers = Get-StorageFaultDomain -Type StorageScaleUnit | Sort FriendlyName
    ```

   > [!TIP]
   > En Espacios de almacenamiento directo, el término "unidad de escala de almacenamiento" hace referencia a todo el almacenamiento sin procesar conectado a un servidor, incluidas las unidades conectadas directamente y los alojamientos externos con conexión directa con las unidades. En este contexto, es lo mismo que ' Server '.

2. Especifique los servidores que se van a usar con el nuevo `-StorageFaultDomainsToUse` parámetro e indexando en `$Servers` . Por ejemplo, para delimitar la asignación a los servidores primero, segundo, tercero y cuarto (índices 0, 1, 2 y 3):

    ```PowerShell
    New-Volume -FriendlyName "MyVolume" -Size 100GB -StorageFaultDomainsToUse $Servers[0,1,2,3]
    ```

### <a name="see-a-delimited-allocation"></a>Ver una asignación delimitada

Para ver cómo se asigna el *volumen* , use el `Get-VirtualDiskFootprintBySSU.ps1` script del [Apéndice](#appendix):

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         100 GB  100 GB  100 GB  100 GB  0       0
```

Tenga en cuenta que solo server1, server2, Server3 y 4 contienen bloques de mi *volumen*.

### <a name="change-a-delimited-allocation"></a>Cambiar una asignación delimitada

Use los `Add-StorageFaultDomain` `Remove-StorageFaultDomain` cmdlets New y para cambiar cómo se delimita la asignación.

Por ejemplo, para subir un *volumen* en un servidor:

1. Especifica que el quinto servidor **puede** almacenar los bloques de mi *volumen*:

    ```PowerShell
    Get-VirtualDisk MyVolume | Add-StorageFaultDomain -StorageFaultDomains $Servers[4]
    ```

2. Especifica que el primer servidor **no puede** almacenar los bloques de mi *volumen*:

    ```PowerShell
    Get-VirtualDisk MyVolume | Remove-StorageFaultDomain -StorageFaultDomains $Servers[0]
    ```

3. Reequilibrar el grupo de almacenamiento para que el cambio surta efecto:

    ```PowerShell
    Get-StoragePool S2D* | Optimize-StoragePool
    ```

Puede supervisar el progreso del reequilibrio con `Get-StorageJob` .

Una vez que haya finalizado, compruebe que se ha deshecho la ejecución de mi *volumen* `Get-VirtualDiskFootprintBySSU.ps1` .

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         0       100 GB  100 GB  100 GB  100 GB  0
```

Tenga en cuenta que server1 ya no contiene bloques de mi *volumen* ; en su lugar, Server5 sí.

## <a name="best-practices"></a>Procedimientos recomendados

Estos son los procedimientos recomendados que se deben seguir al usar la asignación de volúmenes delimitados:

### <a name="choose-four-servers"></a>Elegir cuatro servidores

Delimite cada volumen de reflejo triple a cuatro servidores, no más.

### <a name="balance-storage"></a>Equilibrar el almacenamiento

Equilibre la cantidad de almacenamiento que se asigna a cada servidor, teniendo en cuenta el tamaño del volumen.

### <a name="stagger-delimited-allocation-volumes"></a>Escalonar volúmenes de asignación delimitados

Para maximizar la tolerancia a errores, haga que la asignación de cada volumen sea única, lo que significa que no comparte *todos* sus servidores con otro volumen (la superposición es correcta).

Por ejemplo, en un sistema de ocho nodos: volumen 1: servidores 1, 2, 3, 4 volumen 2: servidores 5, 6, 7, 8 volumen 3: servidores 3, 4, 5, 6 volumen 4: servidores 1, 2, 7, 8

## <a name="analysis"></a>Análisis

En esta sección se deriva la probabilidad matemática de que un volumen permanezca en línea y accesible (o equivalente, la fracción esperada de almacenamiento general que permanece en línea y accesible) como función del número de errores y del tamaño del clúster.

   > [!NOTE]
   > Esta sección es de lectura opcional. Si le interesa ver las matemáticas, siga leyendo. Pero si no es así, no se preocupe: el [uso de PowerShell](#usage-in-powershell) y los [procedimientos recomendados](#best-practices) es todo lo que necesita para implementar correctamente la asignación delimitada.

### <a name="up-to-two-failures-is-always-okay"></a>Un máximo de dos errores siempre es correcto.

Cada volumen de reflejo triple puede sobrevivir a dos errores al mismo tiempo, independientemente de su asignación. Si se produce un error en dos unidades, o se produce un error en dos servidores, o uno de cada, cada volumen de reflejo triple permanece en línea y accesible, incluso con la asignación normal.

### <a name="more-than-half-the-cluster-failing-is-never-okay"></a>Más de la mitad del error del clúster nunca está bien

Por el contrario, en el caso extremo en el que se produce un error en una vez más de la mitad de servidores o unidades del clúster, el [cuórum se pierde](understand-quorum.md) y cada volumen de reflejo triple se queda sin conexión y deja de estar accesible, independientemente de su asignación.

### <a name="what-about-in-between"></a>¿Qué ocurre en entre?

Si se producen tres o más errores al mismo tiempo, pero al menos la mitad de los servidores y las unidades siguen en funcionamiento, los volúmenes con asignación delimitada pueden permanecer en línea y ser accesibles, en función de los servidores que tengan errores.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="can-i-delimit-some-volumes-but-not-others"></a>¿Puedo delimitar algunos volúmenes, pero no otros?

Sí. Puede elegir por volumen si quiere o no delimitar la asignación.

### <a name="does-delimited-allocation-change-how-drive-replacement-works"></a>¿Cambia la asignación delimitada el funcionamiento de la sustitución de la unidad?

No, es lo mismo que con la asignación normal.

## <a name="additional-references"></a>Referencias adicionales

- [Introducción a Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
- [Tolerancia a errores en Espacios de almacenamiento directo](storage-spaces-fault-tolerance.md)

## <a name="appendix"></a>Apéndice

Este script le ayuda a ver cómo se asignan los volúmenes.

Para usarlo como se describió anteriormente, Copie/pegue y guarde como `Get-VirtualDiskFootprintBySSU.ps1` .

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
