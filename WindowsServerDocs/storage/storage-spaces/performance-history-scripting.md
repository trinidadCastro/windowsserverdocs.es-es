---
title: Scripting con el historial de rendimiento de espacios de almacenamiento directo
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 05/15/2018
Keywords: Espacios de almacenamiento directo
ms.localizationpriority: medium
ms.openlocfilehash: cc8ebcaaf7cc39cfadb0ebcec71ed573b436b466
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816326"
---
# <a name="scripting-with-powershell-and-storage-spaces-direct-performance-history"></a>Scripting con PowerShell y espacios de almacenamiento directo el historial de rendimiento

> Se aplica a: Compilación de Windows Server Insider Preview 17692 y versiones posterior

En Windows Server 2019, [espacios de almacenamiento directo](storage-spaces-direct-overview.md) registros y los almacenes de una amplia [historial de rendimiento](performance-history.md) para máquinas virtuales, servidores, unidades, volúmenes, los adaptadores de red y mucho más. Historial de rendimiento es muy fácil consultar y procesar en PowerShell para que pueda ir rápidamente desde *datos sin procesar* a *las respuestas reales* a preguntas como:

1. ¿Hubo algún pico de CPU semana pasada?
2. ¿Cualquier disco físico presenta la latencia anómala?
3. ¿Que las máquinas virtuales consumen más IOPS de almacenamiento ahora mismo?
4. ¿Está saturado en mi ancho de banda de red?
5. ¿Cuándo se ejecutará este volumen sin espacio libre?
6. ¿En el mes pasado, que las máquinas virtuales usan la mayor cantidad de memoria?

El `Get-ClusterPerf` cmdlet se ha creado para scripting. Acepta la entrada de los cmdlets como `Get-VM` o `Get-PhysicalDisk` por la canalización para administrar la asociación y se puede canalizar su salida en los cmdlets de utilidad como `Sort-Object`, `Where-Object`, y `Measure-Object` para crear rápidamente consultas eficaces.

**En este tema se proporciona y se explica 6 scripts de ejemplo que respondan a las 6 preguntas anteriores.** Presentan los patrones que puede aplicar para buscar valores máximos, encontrar los promedios, trazar líneas de tendencia, ejecute a valores atípicos de detección etc., en una variedad de datos y los períodos de tiempo. Se han proporcionado como el código de inicio gratuito para que pueda copiar, ampliar y volver a usar.

   > [!NOTE]
   > Para mayor brevedad, los scripts de ejemplo omiten cosas como control de errores que podría esperar de código de PowerShell de alta calidad. Están diseñados principalmente para inspiración y educación en lugar de producción utilice.

## <a name="sample-1-cpu-i-see-you"></a>Ejemplo 1: CPU, veo!

Este ejemplo se utiliza el `ClusterNode.Cpu.Usage` serie a partir de la `LastWeek` período de tiempo para mostrar el máximo ("límite superior"), el uso de CPU mínimo y promedio para todos los servidores del clúster. También realiza análisis cuartil simple para mostrar el uso de cuántas horas CPU fue superior a 25%, 50% y el 75% en los últimos 8 días.

### <a name="screenshot"></a>Screenshot

En la siguiente captura de pantalla, vemos que *Server-02* tenía un pico inexplicable última semana:

![Captura de pantalla de PowerShell](media/performance-history/Show-CpuMinMaxAvg.png)

### <a name="how-it-works"></a>Cómo funciona

La salida de `Get-ClusterPerf` canalizaciones ordenadamente en integrado `Measure-Object` cmdlet, simplemente especificamos la `Value` propiedad. Con su `-Maximum`, `-Minimum`, y `-Average` marcas, `Measure-Object` nos ofrece las tres primeras columnas casi de forma gratuita. Para realizar el análisis cuartil, podemos canalizar a `Where-Object` y contar cuántos valores eran `-Gt` (mayor que) 25, 50 o 75. El último paso es de presentación con `Format-Hours` y `Format-Percent` funciones auxiliares: sin duda opcional.

### <a name="script"></a>Script

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

## <a name="sample-2-fire-fire-latency-outlier"></a>Ejemplo 2: Incendio, incendios, valores atípicos de latencia

Este ejemplo se utiliza el `PhysicalDisk.Latency.Average` serie desde el `LastHour` definido por el período de tiempo para buscar valores atípicos estadísticos, como unidades con un por horas de latencia media superior a + 3σ (tres desviaciones estándar) por encima de la media de población.

   > [!IMPORTANT]
   > Para mayor brevedad, esta secuencia de comandos implementar medidas de seguridad contra baja varianza, no controla los datos que faltan parciales, no se distingue por modelo o firmware, etcetera. Por favor, ejecute buen juicio y no confíe en esta secuencia de comandos por sí solo para determinar si se debe reemplazar un disco duro. Se presenta aquí con fines formativos.

### <a name="screenshot"></a>Screenshot

En la siguiente captura de pantalla, vemos que hay no hay valores atípicos:

![Captura de pantalla de PowerShell](media/performance-history/Show-LatencyOutlierHDD.png)

### <a name="how-it-works"></a>Cómo funciona

En primer lugar, se excluyen las unidades inactivas o casi inactivas comprobando que `PhysicalDisk.Iops.Total` es constantemente `-Gt 1`. Para cada activo HDD, nos canalizar sus `LastHour` período de tiempo, consta de 360 mediciones en intervalos de 10 segundos, a `Measure-Object -Average` para obtener el promedio de latencia en la última hora. Esta acción configura la población.

Implementamos la [ampliamente conocido fórmula](http://www.mathsisfun.com/data/standard-deviation.html) para buscar la Media `μ` y la desviación estándar `σ` del rellenado. Para cada activo HDD, nos comparar su promedio de latencia a la media de población y dividir por la desviación estándar. Mantenemos los valores sin procesar, por lo que podemos `Sort-Object` nuestros resultados, pero use `Format-Latency` y `Format-StandardDeviation` funciones auxiliares para lo que le mostraremos – ciertamente opcionales de presentación.

Si es cualquier unidad de más de + 3σ, nos `Write-Host` en rojo; si no se encuentra, en verde.

### <a name="script"></a>Script

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

## <a name="sample-3-noisy-neighbor-thats-write"></a>Ejemplo 3: ¿Vecino ruidoso? Eso es de escritura.

Historial de rendimiento puede responder a preguntas sobre *ahora*, demasiado. Nuevas medidas están disponibles en tiempo real, cada 10 segundos. Este ejemplo se utiliza el `VHD.Iops.Total` serie a partir de la `MostRecent` período de tiempo para identificar el más ocupado (algunos podrían decir "ruidoso") las máquinas virtuales consumen más IOPS de almacenamiento, entre todos los hosts del clúster y mostrar el desglose de lectura/escritura de sus actividad.

### <a name="screenshot"></a>Screenshot

En la siguiente captura de pantalla, vemos las máquinas virtuales de las 10 mediante la actividad de almacenamiento:

![Captura de pantalla de PowerShell](media/performance-history/Show-TopIopsVMs.png)

### <a name="how-it-works"></a>Cómo funciona

A diferencia de `Get-PhysicalDisk`, el `Get-VM` cmdlet no es compatible con clústeres: solo devuelve las máquinas virtuales en el servidor local. Para realizar una consulta de todos los servidores en paralelo, se incluyen nuestra llamada en `Invoke-Command (Get-ClusterNode).Name { ... }`. Para todas las máquinas virtuales, obtenemos el `VHD.Iops.Total`, `VHD.Iops.Read`, y `VHD.Iops.Write` medidas. Al no especificar el `-TimeFrame` parámetro, obtenemos el `MostRecent` único punto de datos para cada uno.

   > [!TIP]
   > Estas series refleja la suma de la actividad de la máquina virtual a todos sus archivos VHD/VHDX. Este es un ejemplo donde el historial de rendimiento es se agregan automáticamente para nosotros. Para obtener el desglose por VHD/VHDX, se puede canalizar un individuo `Get-VHD` en `Get-ClusterPerf` en lugar de la máquina virtual.

Los resultados de todos los servidores de unificación como `$Output`, que se puede `Sort-Object` y, a continuación, `Select-Object -First 10`. Tenga en cuenta que `Invoke-Command` decora los resultados con un `PsComputerName` propiedad que indica su procedencia, lo que podemos imprimir saber dónde se está ejecutando la máquina virtual.

### <a name="script"></a>Script

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

## <a name="sample-4-as-they-say-25-gig-is-the-new-10-gig"></a>Ejemplo 4: Como dicen, "25 GB es el nuevo 10-gig"

Este ejemplo se utiliza el `NetAdapter.Bandwidth.Total` serie a partir de la `LastDay` define el período de tiempo para examinar en busca de indicios de saturación de la red, como > 90% del ancho de banda máximo teórico. Para cada adaptador de red del clúster, compara el uso de ancho de banda observados más alto en el último día para su velocidad de vínculo indicados.

### <a name="screenshot"></a>Screenshot

En la siguiente captura de pantalla, vemos que una *Fabrikam NX 4 Pro 2* un pico en el último día:

![Captura de pantalla de PowerShell](media/performance-history/Show-NetworkSaturation.png)

### <a name="how-it-works"></a>Cómo funciona

Repetimos nuestro `Invoke-Command` baza anterior a `Get-NetAdapter` en cada servidor y la canalización en `Get-ClusterPerf`. En el camino, tomamos dos propiedades importantes: su `LinkSpeed` cadena como "10 Gbps" y su raw `Speed` entero, como 10000000000. Usamos `Measure-Object` para obtener el promedio y máximo del último día (recordatorio: cada medida en el `LastDay` período de tiempo representa 5 minutos) y se multiplica por 8 bits por byte para obtener una comparación de manzanas a manzanas.

   > [!NOTE]
   > Algunos proveedores, como Chelsio, incluyen la actividad de memoria directo remoto (RDMA) de acceso en sus *adaptador de red* contadores de rendimiento, por lo que se incluye en el `NetAdapter.Bandwidth.Total` serie. Como Mellanox, puede que otros no. Si no, basta con su proveedor la `NetAdapter.Bandwidth.RDMA.Total` serie en su versión de esta secuencia de comandos.

### <a name="script"></a>Script

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

## <a name="sample-5-make-storage-trendy-again"></a>Ejemplo 5: Asegúrese de almacenamiento moderno nuevo.

Para analizar las tendencias de macro, se conserva el historial de rendimiento hasta 1 año. Este ejemplo se utiliza el `Volume.Size.Available` serie a partir de la `LastYear` período de tiempo para determinar la velocidad a la que se está llenando el almacenamiento y la estimación de cuándo será completa.

### <a name="screenshot"></a>Screenshot

En la siguiente captura de pantalla, vemos la *copia de seguridad* volumen consiste en agregar unos 15 GB por día:

![Captura de pantalla de PowerShell](media/performance-history/Show-StorageTrend.png)

En este caso, alcanzará su capacidad en otro 42 días.

### <a name="how-it-works"></a>Cómo funciona

El `LastYear` período de tiempo tiene un punto de datos al día. Aunque estrictamente solo necesita dos puntos para ajustar una línea de tendencia, en la práctica es mejor requieren más, como los 14 días. Usamos `Select-Object -Last 14` para configurar una matriz de *(x, y)* puntos, para *x* en el intervalo [1, 14]. Con estos puntos, implementamos el sencillo [algoritmo lineal de mínimos cuadrados](http://mathworld.wolfram.com/LeastSquaresFitting.html) encontrar `$A` y `$B` que parametrizar la línea de ajuste perfecto *y = ax + b*. Bienvenido al Instituto todo de nuevo.

Al dividir el volumen `SizeRemaining` propiedad por la tendencia (la pendiente `$A`) nos permite estimar ordinariamente cuántos días a la velocidad actual de crecimiento del almacenamiento, hasta que el volumen está lleno. El `Format-Bytes`, `Format-Trend`, y `Format-Days` la salida de la presentación de las funciones auxiliares.

   > [!IMPORTANT]
   > Esta estimación es lineal y se basa solo en las medidas más recientes de diarias 14. Existen técnicas más sofisticadas y precisas. Por favor, ejecute buen juicio y no confíe en esta secuencia de comandos por sí solo para determinar si se debe invertir en expandir el almacenamiento. Se presenta aquí con fines formativos.

### <a name="script"></a>Script

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

## <a name="sample-6-memory-hog-you-can-run-but-you-cant-hide"></a>Ejemplo 6: Necesidades de memoria, puede ejecutar pero no se puede ocultar

Dado que es el historial de rendimiento se recopilan y almacenan de forma centralizada para todo el clúster, ya no necesita unir los datos de distintos equipos, independientemente de cómo muchas veces las máquinas virtuales mover entre hosts. Este ejemplo se utiliza el `VM.Memory.Assigned` serie a partir de la `LastMonth` período de tiempo para identificar las máquinas virtuales consumen más memoria durante los últimos 35 días.

### <a name="screenshot"></a>Screenshot

En la siguiente captura de pantalla, vemos las máquinas virtuales de las 10 mediante el uso de memoria último mes:

![Captura de pantalla de PowerShell](media/performance-history/Show-TopMemoryVMs.png)

### <a name="how-it-works"></a>Cómo funciona

Repetimos nuestro `Invoke-Command` truco, descrito anteriormente, a `Get-VM` en cada servidor. Usamos `Measure-Object -Average` para obtener el promedio mensual para todas las máquinas virtuales, a continuación, `Sort-Object` seguido `Select-Object -First 10` obtener nuestra tabla de clasificación. (O quizás es nuestro *más deseaba* lista?)

### <a name="script"></a>Script

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

Ya está. Espero que estas muestras inspiran y le ayudan a que empezar a trabajar. Con el eficaz y de historial de rendimiento de espacios de almacenamiento directo, secuencias de comandos compatible con `Get-ClusterPerf` cmdlet, tiene la posibilidad de hacerme:! – complejo preguntas como administrar y supervisar la infraestructura de Windows Server 2019.

## <a name="see-also"></a>Vea también

- [Introducción a Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/getting-started-with-windows-powershell)
- [Información general de espacios directo de almacenamiento](storage-spaces-direct-overview.md)
- [Historial de rendimiento](performance-history.md)
