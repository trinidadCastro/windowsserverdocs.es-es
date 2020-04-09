---
title: Scripting con Espacios de almacenamiento directo historial de rendimiento
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 05/15/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4c25ed4112035fa729ccf17792a846263ec68dfc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856178"
---
# <a name="scripting-with-powershell-and-storage-spaces-direct-performance-history"></a>Scripting con PowerShell y Espacios de almacenamiento directo historial de rendimiento

> Se aplica a: Windows Server 2019

En Windows Server 2019, [espacios de almacenamiento directo](storage-spaces-direct-overview.md) registra y almacena un [historial de rendimiento](performance-history.md) extenso de máquinas virtuales, servidores, unidades, volúmenes, adaptadores de red, etc. El historial de rendimiento es fácil de consultar y procesar en PowerShell para que pueda pasar rápidamente de *datos sin procesar* a *respuestas reales* a preguntas como:

1. ¿Hubo picos de CPU la semana pasada?
2. ¿Hay algún disco físico que muestre una latencia anómala?
3. ¿Qué máquinas virtuales consumen más IOPS de almacenamiento ahora?
4. ¿El ancho de banda de red es saturado?
5. ¿Cuándo se quedará sin espacio disponible este volumen?
6. En el último mes, ¿qué máquinas virtuales usaban más memoria?

El cmdlet `Get-ClusterPerf` está creado para scripting. Acepta la entrada de cmdlets como `Get-VM` o `Get-PhysicalDisk` por la canalización para controlar la asociación, y puede canalizar su salida en cmdlets de utilidad como `Sort-Object`, `Where-Object`y `Measure-Object` para crear rápidamente consultas eficaces.

**En este tema se proporcionan y explican 6 scripts de ejemplo que responden a las 6 preguntas anteriores.** Presentan patrones que se pueden aplicar para buscar picos, buscar promedios, trazar líneas de tendencia, ejecutar la detección de valores atípicos, etc., en una gran variedad de datos y períodos de tiempo. Se proporcionan como código de inicio gratuito para copiarlo, ampliarlo y reutilizarlo.

   > [!NOTE]
   > Por motivos de brevedad, los scripts de ejemplo omiten aspectos como el control de errores que podría esperar del código de PowerShell de alta calidad. Están pensadas principalmente para inspiración y educación en lugar de usarse en producción.

## <a name="sample-1-cpu-i-see-you"></a>Ejemplo 1: CPU, me veo.

En este ejemplo se usa la serie `ClusterNode.Cpu.Usage` del período de `LastWeek` para mostrar el valor máximo ("marca de límite superior"), el mínimo y el promedio de uso de CPU para cada servidor del clúster. También realiza un análisis de cuartil sencillo para mostrar el número de horas que el uso de CPU superó el 25%, el 50% y el 75% en los últimos 8 días.

### <a name="screenshot"></a>Screenshot

En la captura de pantalla siguiente, vemos que *servidor-02* tenía un pico no explicado la semana pasada:

![Captura de pantalla de PowerShell](media/performance-history/Show-CpuMinMaxAvg.png)

### <a name="how-it-works"></a>Cómo funciona

La salida de `Get-ClusterPerf` canalizaciones perfectamente en el cmdlet de `Measure-Object` integrado, solo se especifica la propiedad `Value`. Con sus marcas de `-Maximum`, `-Minimum`y `-Average`, `Measure-Object` nos proporciona las tres primeras columnas casi de forma gratuita. Para realizar el análisis del cuartil, podemos canalizar a `Where-Object` y contar el número de valores que se `-Gt` (mayor que) 25, 50 o 75. El último paso es Beautify con `Format-Hours` y `Format-Percent` funciones auxiliares; ciertamente, es opcional.

### <a name="script"></a>Secuencia de comandos

Este es el script:

```
Function Format-Hours {
    Param (
        $RawValue
    )
    # Weekly timeframe has frequency 15 minutes = 4 points per hour
    [Math]::Round($RawValue/4)
}

Function Format-Percent {
    Param (
        $RawValue
    )
    [String][Math]::Round($RawValue) + " " + "%"
}

$Output = Get-ClusterNode | ForEach-Object {
    $Data = $_ | Get-ClusterPerf -ClusterNodeSeriesName "ClusterNode.Cpu.Usage" -TimeFrame "LastWeek"

    $Measure = $Data | Measure-Object -Property Value -Minimum -Maximum -Average
    $Min = $Measure.Minimum
    $Max = $Measure.Maximum
    $Avg = $Measure.Average

    [PsCustomObject]@{
        "ClusterNode"    = $_.Name
        "MinCpuObserved" = Format-Percent $Min
        "MaxCpuObserved" = Format-Percent $Max
        "AvgCpuObserved" = Format-Percent $Avg
        "HrsOver25%"     = Format-Hours ($Data | Where-Object Value -Gt 25).Length
        "HrsOver50%"     = Format-Hours ($Data | Where-Object Value -Gt 50).Length
        "HrsOver75%"     = Format-Hours ($Data | Where-Object Value -Gt 75).Length
    }
}

$Output | Sort-Object ClusterNode | Format-Table
```

## <a name="sample-2-fire-fire-latency-outlier"></a>Ejemplo 2: desencadenar, desencadenar, valores atípicos de latencia

En este ejemplo se usa la serie `PhysicalDisk.Latency.Average` de la `LastHour` período de tiempo para buscar valores atípicos estadísticos, definidos como unidades con una latencia media por hora que supera el valor de +3 σ (tres desviaciones estándar) por encima de la media de población.

   > [!IMPORTANT]
   > Por motivos de brevedad, este script no implementa medidas de seguridad frente a la varianza baja, no controla los datos que faltan parcialmente, no distingue por modelo o firmware, etc. Ejerza un buen juicio y no confíe solo en este script para determinar si se debe reemplazar un disco duro. Se presenta aquí únicamente con fines educativos.

### <a name="screenshot"></a>Screenshot

En la captura de pantalla siguiente, vemos que no hay valores atípicos:

![Captura de pantalla de PowerShell](media/performance-history/Show-LatencyOutlierHDD.png)

### <a name="how-it-works"></a>Cómo funciona

En primer lugar, se excluyen las unidades inactivas o casi inactivas comprobando que `PhysicalDisk.Iops.Total` es constantemente `-Gt 1`. Para cada HDD activo, canalizamos su `LastHour` período de tiempo, formado por 360 medidas a intervalos de 10 segundos, para `Measure-Object -Average` obtener su promedio de latencia en la última hora. Esto configura el rellenado.

Implementamos la [fórmula ampliamente conocida](http://www.mathsisfun.com/data/standard-deviation.html) para encontrar el promedio de `μ` y la desviación estándar `σ` de la población. Para cada unidad de disco duro activa, se compara su promedio de latencia con el promedio de población y se divide por la desviación estándar. Mantenemos los valores sin procesar, de modo que podamos `Sort-Object` nuestros resultados, pero usar `Format-Latency` y `Format-StandardDeviation` funciones auxiliares para Beautify lo que mostraremos (ciertamente opcional).

Si alguna unidad es mayor que +3 σ, `Write-Host` en rojo; en caso contrario, en verde.

### <a name="script"></a>Secuencia de comandos

Este es el script:

```
Function Format-Latency {
    Param (
        $RawValue
    )
    $i = 0 ; $Labels = ("s", "ms", "μs", "ns") # Petabits, just in case!
    Do { $RawValue *= 1000 ; $i++ } While ( $RawValue -Lt 1 )
    # Return
    [String][Math]::Round($RawValue, 2) + " " + $Labels[$i]
}

Function Format-StandardDeviation {
    Param (
        $RawValue
    )
    If ($RawValue -Gt 0) {
        $Sign = "+"
    }
    Else {
        $Sign = "-"
    }
    # Return
    $Sign + [String][Math]::Round([Math]::Abs($RawValue), 2) + "σ"
}

$HDD = Get-StorageSubSystem Cluster* | Get-PhysicalDisk | Where-Object MediaType -Eq HDD

$Output = $HDD | ForEach-Object {

    $Iops = $_ | Get-ClusterPerf -PhysicalDiskSeriesName "PhysicalDisk.Iops.Total" -TimeFrame "LastHour"
    $AvgIops = ($Iops | Measure-Object -Property Value -Average).Average

    If ($AvgIops -Gt 1) { # Exclude idle or nearly idle drives

        $Latency = $_ | Get-ClusterPerf -PhysicalDiskSeriesName "PhysicalDisk.Latency.Average" -TimeFrame "LastHour"
        $AvgLatency = ($Latency | Measure-Object -Property Value -Average).Average

        [PsCustomObject]@{
            "FriendlyName"  = $_.FriendlyName
            "SerialNumber"  = $_.SerialNumber
            "MediaType"     = $_.MediaType
            "AvgLatencyPopulation" = $null # Set below
            "AvgLatencyThisHDD"    = Format-Latency $AvgLatency
            "RawAvgLatencyThisHDD" = $AvgLatency
            "Deviation"            = $null # Set below
            "RawDeviation"         = $null # Set below
        }
    }
}

If ($Output.Length -Ge 3) { # Minimum population requirement

    # Find mean μ and standard deviation σ
    $μ = ($Output | Measure-Object -Property RawAvgLatencyThisHDD -Average).Average
    $d = $Output | ForEach-Object { ($_.RawAvgLatencyThisHDD - $μ) * ($_.RawAvgLatencyThisHDD - $μ) }
    $σ = [Math]::Sqrt(($d | Measure-Object -Sum).Sum / $Output.Length)

    $FoundOutlier = $False

    $Output | ForEach-Object {
        $Deviation = ($_.RawAvgLatencyThisHDD - $μ) / $σ
        $_.AvgLatencyPopulation = Format-Latency $μ
        $_.Deviation = Format-StandardDeviation $Deviation
        $_.RawDeviation = $Deviation
        # If distribution is Normal, expect >99% within 3σ
        If ($Deviation -Gt 3) {
            $FoundOutlier = $True
        }
    }

    If ($FoundOutlier) {
        Write-Host -BackgroundColor Black -ForegroundColor Red "Oh no! There's an HDD significantly slower than the others."
    }
    Else {
        Write-Host -BackgroundColor Black -ForegroundColor Green "Good news! No outlier found."
    }

    $Output | Sort-Object RawDeviation -Descending | Format-Table FriendlyName, SerialNumber, MediaType, AvgLatencyPopulation, AvgLatencyThisHDD, Deviation

}
Else {
    Write-Warning "There aren't enough active drives to look for outliers right now."
}
```

## <a name="sample-3-noisy-neighbor-thats-write"></a>Ejemplo 3: vecino ruidoso? ¡ Eso es escribir!

El historial de rendimiento también puede responder a preguntas en *este momento*. Las nuevas medidas están disponibles en tiempo real, cada 10 segundos. En este ejemplo se usa la serie de `VHD.Iops.Total` de la `MostRecent` período de tiempo para identificar las máquinas virtuales de mayor nivel (algunas podrían decir "noisiest") que consumen la mayoría de las IOPS de almacenamiento, en todos los hosts del clúster, y muestran el desglose de lectura y escritura de su actividad.

### <a name="screenshot"></a>Screenshot

En la captura de pantalla siguiente, vemos las 10 principales máquinas virtuales por actividad de almacenamiento:

![Captura de pantalla de PowerShell](media/performance-history/Show-TopIopsVMs.png)

### <a name="how-it-works"></a>Cómo funciona

A diferencia de `Get-PhysicalDisk`, el cmdlet de `Get-VM` no es compatible con los clústeres, sino que solo devuelve las máquinas virtuales del servidor local. Para realizar consultas desde cada servidor en paralelo, se ajusta nuestra llamada en `Invoke-Command (Get-ClusterNode).Name { ... }`. Para cada máquina virtual, obtenemos las medidas de `VHD.Iops.Total`, `VHD.Iops.Read`y `VHD.Iops.Write`. Si no se especifica el parámetro `-TimeFrame`, obtenemos el `MostRecent` único punto de datos para cada uno.

   > [!TIP]
   > Estas series reflejan la suma de la actividad de esta máquina virtual en todos sus archivos VHD/VHDX. Este es un ejemplo en el que se agrega automáticamente el historial de rendimiento. Para obtener el desglose por VHD/VHDX, puede canalizar una `Get-VHD` individual en `Get-ClusterPerf` en lugar de en la máquina virtual.

Los resultados de cada servidor se unen como `$Output`, que podemos `Sort-Object` y, después, `Select-Object -First 10`. Tenga en cuenta que `Invoke-Command` decora los resultados con una propiedad `PsComputerName` que indica de dónde proceden, que se pueden imprimir para saber dónde se ejecuta la máquina virtual.

### <a name="script"></a>Secuencia de comandos

Este es el script:

```
$Output = Invoke-Command (Get-ClusterNode).Name {
    Function Format-Iops {
        Param (
            $RawValue
        )
        $i = 0 ; $Labels = (" ", "K", "M", "B", "T") # Thousands, millions, billions, trillions...
        Do { if($RawValue -Gt 1000){$RawValue /= 1000 ; $i++ } } While ( $RawValue -Gt 1000 )
        # Return
        [String][Math]::Round($RawValue) + " " + $Labels[$i]
    }

    Get-VM | ForEach-Object {
        $IopsTotal = $_ | Get-ClusterPerf -VMSeriesName "VHD.Iops.Total"
        $IopsRead  = $_ | Get-ClusterPerf -VMSeriesName "VHD.Iops.Read"
        $IopsWrite = $_ | Get-ClusterPerf -VMSeriesName "VHD.Iops.Write"
        [PsCustomObject]@{
            "VM" = $_.Name
            "IopsTotal" = Format-Iops $IopsTotal.Value
            "IopsRead"  = Format-Iops $IopsRead.Value
            "IopsWrite" = Format-Iops $IopsWrite.Value
            "RawIopsTotal" = $IopsTotal.Value # For sorting...
        }
    }
}

$Output | Sort-Object RawIopsTotal -Descending | Select-Object -First 10 | Format-Table PsComputerName, VM, IopsTotal, IopsRead, IopsWrite
```

## <a name="sample-4-as-they-say-25-gig-is-the-new-10-gig"></a>Ejemplo 4: como dicen, "25 gigas es el nuevo 10 Giga"

En este ejemplo se usa la serie `NetAdapter.Bandwidth.Total` de la `LastDay` período de tiempo para buscar signos de saturación de la red, definidos como > 90% del ancho de banda máximo teórico. Para cada adaptador de red del clúster, compara el mayor uso de ancho de banda observado en el último día con la velocidad de vínculo indicada.

### <a name="screenshot"></a>Screenshot

En la captura de pantalla siguiente, vemos que un *#2 de Fabrikam NX-4 Pro* alcanza el máximo en el último día:

![Captura de pantalla de PowerShell](media/performance-history/Show-NetworkSaturation.png)

### <a name="how-it-works"></a>Cómo funciona

Repetimos nuestra `Invoke-Command` baza de arriba para `Get-NetAdapter` en cada servidor y canalización en `Get-ClusterPerf`. A lo largo del proceso, tomamos dos propiedades importantes: su `LinkSpeed` cadena como "10 Gbps" y su número entero de `Speed` sin formato como 10 mil millones. Usamos `Measure-Object` para obtener el promedio y el pico del último día (recordatorio: cada medida en el período de `LastDay` representa 5 minutos) y multiplicar por 8 bits por byte para obtener una comparación de manzanas a manzanas.

   > [!NOTE]
   > Algunos proveedores, como Chelsio, incluyen actividad de acceso directo a memoria remota (RDMA) en los contadores de rendimiento del *adaptador de red* , por lo que se incluye en la serie `NetAdapter.Bandwidth.Total`. Otros, como Mellanox, no pueden. Si el proveedor no lo hace, simplemente agregue la serie `NetAdapter.Bandwidth.RDMA.Total` en su versión de este script.

### <a name="script"></a>Secuencia de comandos

Este es el script:

```
$Output = Invoke-Command (Get-ClusterNode).Name {

    Function Format-BitsPerSec {
        Param (
            $RawValue
        )
        $i = 0 ; $Labels = ("bps", "kbps", "Mbps", "Gbps", "Tbps", "Pbps") # Petabits, just in case!
        Do { $RawValue /= 1000 ; $i++ } While ( $RawValue -Gt 1000 )
        # Return
        [String][Math]::Round($RawValue) + " " + $Labels[$i]
    }

    Get-NetAdapter | ForEach-Object {

        $Inbound = $_ | Get-ClusterPerf -NetAdapterSeriesName "NetAdapter.Bandwidth.Inbound" -TimeFrame "LastDay"
        $Outbound = $_ | Get-ClusterPerf -NetAdapterSeriesName "NetAdapter.Bandwidth.Outbound" -TimeFrame "LastDay"

        If ($Inbound -Or $Outbound) {

            $InterfaceDescription = $_.InterfaceDescription
            $LinkSpeed = $_.LinkSpeed
    
            $MeasureInbound = $Inbound | Measure-Object -Property Value -Maximum
            $MaxInbound = $MeasureInbound.Maximum * 8 # Multiply to bits/sec
    
            $MeasureOutbound = $Outbound | Measure-Object -Property Value -Maximum
            $MaxOutbound = $MeasureOutbound.Maximum * 8 # Multiply to bits/sec
    
            $Saturated = $False
    
            # Speed property is Int, e.g. 10000000000
            If (($MaxInbound -Gt (0.90 * $_.Speed)) -Or ($MaxOutbound -Gt (0.90 * $_.Speed))) {
                $Saturated = $True
                Write-Warning "In the last day, adapter '$InterfaceDescription' on server '$Env:ComputerName' exceeded 90% of its '$LinkSpeed' theoretical maximum bandwidth. In general, network saturation leads to higher latency and diminished reliability. Not good!"
            }
    
            [PsCustomObject]@{
                "NetAdapter"  = $InterfaceDescription
                "LinkSpeed"   = $LinkSpeed
                "MaxInbound"  = Format-BitsPerSec $MaxInbound
                "MaxOutbound" = Format-BitsPerSec $MaxOutbound
                "Saturated"   = $Saturated
            }
        }
    }
}

$Output | Sort-Object PsComputerName, InterfaceDescription | Format-Table PsComputerName, NetAdapter, LinkSpeed, MaxInbound, MaxOutbound, Saturated
```

## <a name="sample-5-make-storage-trendy-again"></a>Ejemplo 5: volver a crear la tendencia de almacenamiento.

Para ver las tendencias de las macros, el historial de rendimiento se conserva hasta 1 año. En este ejemplo se utiliza la serie `Volume.Size.Available` de la `LastYear` período de tiempo para determinar la velocidad a la que se está llenando y calcular el almacenamiento cuando esté lleno.

### <a name="screenshot"></a>Screenshot

En la captura de pantalla siguiente, vemos que el volumen de *copia de seguridad* agrega unos 15 GB por día:

![Captura de pantalla de PowerShell](media/performance-history/Show-StorageTrend.png)

A esta velocidad, alcanzará su capacidad en otro 42 días.

### <a name="how-it-works"></a>Cómo funciona

El período de tiempo `LastYear` tiene un punto de datos por día. Aunque solo necesita estrictamente dos puntos para ajustarse a una línea de tendencia, en la práctica es mejor requerir más, como 14 días. Usamos `Select-Object -Last 14` para configurar una matriz de puntos *(x, y)* , para *x* en el intervalo [1, 14]. Con estos puntos, se implementa el [algoritmo de mínimos cuadrados lineal](http://mathworld.wolfram.com/LeastSquaresFitting.html) sencillo para buscar `$A` y `$B` que parametrizan la línea de mejor ajuste *y = AX + b*. Bienvenido a la escuela alta de nuevo.

La división de la propiedad del `SizeRemaining` del volumen por la tendencia (la pendiente `$A`) nos permite calcular de forma bruta cuántos días, a la velocidad actual de crecimiento del almacenamiento, hasta que el volumen está lleno. Las funciones auxiliares `Format-Bytes`, `Format-Trend`y `Format-Days` Beautify la salida.

   > [!IMPORTANT]
   > Esta estimación es lineal y solo se basa en las 14 medidas diarias más recientes. Existen técnicas más sofisticadas y precisas. Ejecute un buen criterio y no se base en este script solo para determinar si desea invertir en la expansión del almacenamiento. Se presenta aquí únicamente con fines educativos.

### <a name="script"></a>Secuencia de comandos

Este es el script:

```

Function Format-Bytes {
    Param (
        $RawValue
    )
    $i = 0 ; $Labels = ("B", "KB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB")
    Do { $RawValue /= 1024 ; $i++ } While ( $RawValue -Gt 1024 )
    # Return
    [String][Math]::Round($RawValue) + " " + $Labels[$i]
}

Function Format-Trend {
    Param (
        $RawValue
    )
    If ($RawValue -Eq 0) {
        "0"
    }
    Else {
        If ($RawValue -Gt 0) {
            $Sign = "+"
        }
        Else {
            $Sign = "-"
        }
        # Return
        $Sign + $(Format-Bytes [Math]::Abs($RawValue)) + "/day"
    }
}

Function Format-Days {
    Param (
        $RawValue
    )
    [Math]::Round($RawValue)
}

$CSV = Get-Volume | Where-Object FileSystem -Like "*CSV*"

$Output = $CSV | ForEach-Object {

    $N = 14 # Require 14 days of history

    $Data = $_ | Get-ClusterPerf -VolumeSeriesName "Volume.Size.Available" -TimeFrame "LastYear" | Sort-Object Time | Select-Object -Last $N

    If ($Data.Length -Ge $N) {

        # Last N days as (x, y) points
        $PointsXY = @()
        1..$N | ForEach-Object {
            $PointsXY += [PsCustomObject]@{ "X" = $_ ; "Y" = $Data[$_-1].Value }
        }

        # Linear (y = ax + b) least squares algorithm
        $MeanX = ($PointsXY | Measure-Object -Property X -Average).Average
        $MeanY = ($PointsXY | Measure-Object -Property Y -Average).Average
        $XX = $PointsXY | ForEach-Object { $_.X * $_.X }
        $XY = $PointsXY | ForEach-Object { $_.X * $_.Y }
        $SSXX = ($XX | Measure-Object -Sum).Sum - $N * $MeanX * $MeanX
        $SSXY = ($XY | Measure-Object -Sum).Sum - $N * $MeanX * $MeanY
        $A = ($SSXY / $SSXX)
        $B = ($MeanY - $A * $MeanX)
        $RawTrend = -$A # Flip to get daily increase in Used (vs decrease in Remaining)
        $Trend = Format-Trend $RawTrend

        If ($RawTrend -Gt 0) {
            $DaysToFull = Format-Days ($_.SizeRemaining / $RawTrend)
        }
        Else {
            $DaysToFull = "-"
        }
    }
    Else {
        $Trend = "InsufficientHistory"
        $DaysToFull = "-"
    }

    [PsCustomObject]@{
        "Volume"     = $_.FileSystemLabel
        "Size"       = Format-Bytes ($_.Size)
        "Used"       = Format-Bytes ($_.Size - $_.SizeRemaining)
        "Trend"      = $Trend
        "DaysToFull" = $DaysToFull
    }
}

$Output | Format-Table
```

## <a name="sample-6-memory-hog-you-can-run-but-you-cant-hide"></a>Ejemplo 6: Hog de memoria, puede ejecutar pero no puede ocultar

Dado que el historial de rendimiento se recopila y almacena de forma centralizada para todo el clúster, nunca es necesario unir los datos de distintos equipos, independientemente del número de veces que las máquinas virtuales se muevan entre los hosts. En este ejemplo se usa la serie `VM.Memory.Assigned` del período de `LastMonth` para identificar las máquinas virtuales que consumen más memoria durante los últimos 35 días.

### <a name="screenshot"></a>Screenshot

En la captura de pantalla siguiente, vemos las 10 principales máquinas virtuales por uso de memoria el mes pasado:

![Captura de pantalla de PowerShell](media/performance-history/Show-TopMemoryVMs.png)

### <a name="how-it-works"></a>Cómo funciona

Repetimos nuestro truco `Invoke-Command`, introducido anteriormente, para `Get-VM` en cada servidor. Usamos `Measure-Object -Average` para obtener el promedio mensual de cada máquina virtual y, a continuación, `Sort-Object` seguido de `Select-Object -First 10` para obtener nuestro marcador. (O tal vez sea nuestra lista *más deseada* ?)

### <a name="script"></a>Secuencia de comandos

Este es el script:

```
$Output = Invoke-Command (Get-ClusterNode).Name {
    Function Format-Bytes {
        Param (
            $RawValue
        )
        $i = 0 ; $Labels = ("B", "KB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB")
        Do { if( $RawValue -Gt 1024 ){ $RawValue /= 1024 ; $i++ } } While ( $RawValue -Gt 1024 )
        # Return
        [String][Math]::Round($RawValue) + " " + $Labels[$i]
    }
    
    Get-VM | ForEach-Object {
        $Data = $_ | Get-ClusterPerf -VMSeriesName "VM.Memory.Assigned" -TimeFrame "LastMonth"
        If ($Data) {
            $AvgMemoryUsage = ($Data | Measure-Object -Property Value -Average).Average
            [PsCustomObject]@{
                "VM" = $_.Name
                "AvgMemoryUsage" = Format-Bytes $AvgMemoryUsage.Value
                "RawAvgMemoryUsage" = $AvgMemoryUsage.Value # For sorting...
            }
        }
    }
}

$Output | Sort-Object RawAvgMemoryUsage -Descending | Select-Object -First 10 | Format-Table PsComputerName, VM, AvgMemoryUsage
```

Ya está. Afortunadamente, estos ejemplos le inspiren y le ayudarán a empezar. Con Espacios de almacenamiento directo historial de rendimiento y el eficaz cmdlet de `Get-ClusterPerf` compatible con scripting, está capacitado para preguntar y responder. : preguntas complejas al administrar y supervisar la infraestructura de Windows Server 2019.

## <a name="see-also"></a>Vea también

- [Introducción a Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/getting-started-with-windows-powershell)
- [Información general de Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
- [Historial de rendimiento](performance-history.md)
