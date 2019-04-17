---
title: Secuencias de comandos con el historial de rendimiento de espacios de almacenamiento directo
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 05/15/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: cc8ebcaaf7cc39cfadb0ebcec71ed573b436b466
ms.sourcegitcommit: 78ecb64cac789751abf9fd3f55b4a1fcbbe4dad2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "8843113"
---
# Secuencias de comandos con PowerShell y espacios de almacenamiento directo Historial de rendimiento

> Se aplica a: Windows Server Insider preliminar 17692 y versiones posterior

En Windows Server 2019, [Espacios de almacenamiento directo](storage-spaces-direct-overview.md) registra y almacena un amplio [historial de rendimiento](performance-history.md) para las máquinas virtuales, servidores, las unidades, volúmenes, adaptadores de red y mucho más. Historial de rendimiento es fácil de consulta y proceso de PowerShell para rápidamente puede ir desde *datos sin procesar* a *reales respuestas* a preguntas como:

1. ¿Hubo picos de CPU la semana pasada?
2. ¿Cualquier disco físico presenta latencia anómala?
3. ¿Qué máquinas virtuales consumen más almacenamiento e/s por segundo ahora?
4. ¿Es el ancho de banda de red saturada?
5. ¿Cuándo se ejecutará en este volumen sin espacio libre?
6. ¿En el último mes, qué máquinas virtuales usan la mayor cantidad de memoria?

El `Get-ClusterPerf` cmdlet está diseñado para el scripting. Acepta entrada de cmdlets como `Get-VM` o `Get-PhysicalDisk` la canalización de controlar la asociación y se puede canalizar su salida a los cmdlets de utilidad como `Sort-Object`, `Where-Object`, y `Measure-Object` para componer rápidamente consultas eficaces.

**Este tema proporciona y explica 6 scripts de muestra que responda a las 6 preguntas anteriores.** Presentan patrones que se puede aplicar para búsqueda picos medias, las líneas de tendencia de trazado, ejecutar a aislados de detección y mucho más, en una variedad de datos y plazos de pago. Se proporcionan como código de inicio libre para que puedas copiar, ampliar y volver a usar.

   > [!NOTE]
   > Para mayor brevedad, los scripts de muestra omiten cosas como control de errores que cabría esperar de código de PowerShell de alta calidad. Se destinan principalmente inspiración y educación en lugar de producción a usar.

## Ejemplo 1: CPU, ¿se puede ver puedes!

Esta muestra se usa el `ClusterNode.Cpu.Usage` serie desde el `LastWeek` período de tiempo para mostrar el máximo ("marca de agua de alta"), el mínimo y promedio uso de CPU para todos los servidores del clúster. También hace un análisis de cuartil simple para mostrar el uso de cuántas horas CPU fue superior a 25%, 50% y 75% en los últimos días 8.

### Screenshot

En la captura de pantalla siguiente, vemos que *Server-02* tenía un aumento inesperado de la semana pasada:

![Captura de pantalla de PowerShell](media/performance-history/Show-CpuMinMaxAvg.png)

### Funcionamiento

La salida de `Get-ClusterPerf` canalizaciones bien en integrado `Measure-Object` cmdlet, simplemente especificamos el `Value` propiedad. Con su `-Maximum`, `-Minimum`, y `-Average` marcas, `Measure-Object` nos da las tres primeras columnas casi de forma gratuita. Para realizar el análisis de cuartil, podemos canalizar a `Where-Object` y recuento de los valores de cuántos estaban `-Gt` (mayor que) 25, 50 o 75. El último paso es de presentación con `Format-Hours` y `Format-Percent` funciones auxiliares: ciertamente opcional.

### Script

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

## Ejemplo 2: Disparo, disparo, aislados de latencia

Esta muestra se usa el `PhysicalDisk.Latency.Average` serie desde el `LastHour` período de tiempo para buscar valores estadísticos aberrantes, se definen como unidades con una latencia promedio por hora o igual al + 3σ (tres estándar variaciones) por encima del promedio de población.

   > [!IMPORTANT]
   > Para mayor brevedad, este script no implementa las medidas de seguridad frente a poca variación, no controla los datos que faltan parciales, no distingue por modelo o firmware, etcetera. Por favor, juzgar buena y no dependen de este script solamente para determinar si se va a reemplazar el disco duro. Se presenta aquí solo con fines educativos.

### Screenshot

En la captura de pantalla siguiente, vemos que no hay ningún valores aberrantes:

![Captura de pantalla de PowerShell](media/performance-history/Show-LatencyOutlierHDD.png)

### Funcionamiento

En primer lugar, se excluirán unidades de inactividad o casi inactivas, comprobando que `PhysicalDisk.Iops.Total` está constantemente `-Gt 1`. Para cada unidad de disco duro activo, te canalizar su `LastHour` período de tiempo, consta de 360 medidas cada 10 segundos, a `Measure-Object -Average` para obtener su promedio de latencia en la última hora. Esto establece la nuestra población.

Implementamos la [fórmula ampliamente conocidos](http://www.mathsisfun.com/data/standard-deviation.html) para encontrar la Media `μ` y desviación estándar `σ` de la población. Para cada unidad de disco duro activo, se compara su promedio de latencia para el promedio de población y divide por la desviación estándar. Mantenemos los valores sin procesar, por lo que podemos `Sort-Object` nuestros resultados, pero usa `Format-Latency` y `Format-StandardDeviation` funciones auxiliares a lo que te mostraremos: ciertamente opcionales de presentación.

Si cualquier unidad es más de + 3σ, te `Write-Host` en rojo; Si no, en verde.

### Script

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

## Ejemplo 3: Vecino ruidoso? Es de escritura.

Historial de rendimiento puede responder a preguntas sobre *ahora mismo*, demasiado. Nuevas mediciones están disponibles en tiempo real, cada 10 segundos. Esta muestra se usa el `VHD.Iops.Total` serie desde el `MostRecent` período de tiempo para identificar la más ocupado (algunas pueden decir "más ruidosos") consumir más almacenamiento e/s por segundo, a través de todos los hosts del clúster y mostrar el desglose de lectura y escritura de su actividad de las máquinas virtuales.

### Screenshot

En la captura de pantalla siguiente, vemos las máquinas virtuales de las 10 por la actividad de almacenamiento:

![Captura de pantalla de PowerShell](media/performance-history/Show-TopIopsVMs.png)

### Funcionamiento

A diferencia de `Get-PhysicalDisk`, el `Get-VM` cmdlet no es compatible con clústeres: devuelve solo las máquinas virtuales en el servidor local. Para realizar una consulta de todos los servidores en paralelo, Encapsulamos nuestra llamada en `Invoke-Command (Get-ClusterNode).Name { ... }`. Para cada máquina virtual, obtenemos los `VHD.Iops.Total`, `VHD.Iops.Read`, y `VHD.Iops.Write` medidas. Si no especificas la `-TimeFrame` parámetro, obtenemos los `MostRecent` único punto de datos para cada uno.

   > [!TIP]
   > Estas series reflejar la suma de la actividad de la VM para todos sus archivos VHD/VHDX. Este es un ejemplo donde historial de rendimiento es que se agregan automáticamente para que podamos. Para obtener el desglose por-VHD/VHDX, podría canalizar una persona `Get-VHD` en `Get-ClusterPerf` en lugar de la máquina virtual.

Los resultados de todos los servidores reúnen, como `$Output`, que se pueden `Sort-Object` y, después, `Select-Object -First 10`. Ten en cuenta que `Invoke-Command` decore resultados con un `PsComputerName` propiedad que indica su procedencia, lo que podemos imprimir saber dónde se ejecuta la máquina virtual.

### Script

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

## Ejemplo 4: Como dicen, "25 GB es el nuevo 10-GB"

Esta muestra se usa el `NetAdapter.Bandwidth.Total` serie desde el `LastDay` período de tiempo para buscar indicios de saturación de la red, se definen como > 90% del ancho de banda máximo teórico. Para cada adaptador de red en el clúster, compara el uso de ancho de banda observados más alto en el último día a su velocidad del vínculo establecido.

### Screenshot

En la captura de pantalla siguiente, vemos que uno *Fabrikam NX-4 Pro 2* alcanzaron el máximo en el último día:

![Captura de pantalla de PowerShell](media/performance-history/Show-NetworkSaturation.png)

### Funcionamiento

Repetimos nuestra `Invoke-Command` búsqueda anterior a `Get-NetAdapter` en cada servidor y la canalización en `Get-ClusterPerf`. A lo largo de la forma, obtenemos dos propiedades pertinentes: su `LinkSpeed` cadena como "10 Gbps" y su sin procesar `Speed` entero como 10000000000. Usamos `Measure-Object` para obtener el promedio y el pico desde el último día (aviso: cada medición en el `LastDay` período de tiempo representa cinco minutos) y se multiplica por 8 bits por byte para obtener una comparación de manzanas equivalentes.

   > [!NOTE]
   > Algunos proveedores, como Chelsio, incluyen actividad de memoria de remote direct access (RDMA) en los contadores de rendimiento de *Adaptador de red* , por lo que se incluye en el `NetAdapter.Bandwidth.Total` serie. Otras, como Mellanox, es posible que no. Si tu proveedor simplemente no así, agregue el `NetAdapter.Bandwidth.RDMA.Total` serie en su versión de este script.

### Script

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

## Ejemplo 5: Almacenamiento moderno que vuelva a estar!

Para ver las tendencias de macros, se conserva el historial de rendimiento de hasta 1 año. Esta muestra se usa el `Volume.Size.Available` serie desde el `LastYear` período de tiempo para determinar la velocidad a la que se está completando con almacenamiento y cálculo cuándo estará completa.

### Screenshot

En la captura de pantalla siguiente, vemos que el volumen de *copia de seguridad* es agregar unos 15 GB al día:

![Captura de pantalla de PowerShell](media/performance-history/Show-StorageTrend.png)

En este caso, alcanzará su capacidad en otro 42 días.

### Funcionamiento

El `LastYear` período de tiempo tiene un punto de datos al día. Aunque estrictamente solo necesita dos puntos para ajustar una línea de tendencia, en la práctica es mejor requerir más, como 14 días. Usamos `Select-Object -Last 14` para configurar una matriz de puntos *(x, y)* , para *x* en el intervalo [1, 14]. Con estos puntos, implementamos el [algoritmo lineal mínimos cuadrados](http://mathworld.wolfram.com/LeastSquaresFitting.html) de sencilla para encontrar `$A` y `$B` que parametrizar la línea de la mejor opción *y = ax + b*. Bienvenido a la escuela todo de nuevo.

Dividir el volumen `SizeRemaining` propiedad por la tendencia (la pendiente `$A`) nos permite calcular sencillos cuántos días, a la velocidad actual del almacenamiento de información, hasta que el volumen está lleno. El `Format-Bytes`, `Format-Trend`, y `Format-Days` la salida de las funciones auxiliares de presentación.

   > [!IMPORTANT]
   > Este cálculo es lineal y se basa solo en las medidas diarias 14 más recientes. Existen técnicas más sofisticadas y precisas. Por favor, juzgar buena y no dependen de este script solamente para determinar si se va a invertir en el almacenamiento en expansión. Se presenta aquí solo con fines educativos.

### Script

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

## Ejemplo 6: Necesidades de memoria, puedes ejecutar, pero no puede ocultar

Dado que es el historial de rendimiento se recopila y almacena de forma centralizada para todo el clúster, nunca se necesitan unir datos desde equipos diferentes, independientemente de cómo muchas veces las máquinas virtuales mover entre hosts. Esta muestra se usa el `VM.Memory.Assigned` serie desde el `LastMonth` período de tiempo para identificar las máquinas virtuales consumen más memoria durante los últimos 35 días.

### Screenshot

En la captura de pantalla siguiente, vemos las máquinas virtuales de las 10 mediante el uso de memoria último mes:

![Captura de pantalla de PowerShell](media/performance-history/Show-TopMemoryVMs.png)

### Funcionamiento

Repetimos nuestra `Invoke-Command` truco, introducido anteriormente, a `Get-VM` en cada servidor. Usamos `Measure-Object -Average` para obtener el promedio mensual para cada máquina virtual, a continuación, `Sort-Object` seguido `Select-Object -First 10` para obtener la tabla de clasificación. (O quizás es nuestra list? *Más quería* )

### Script

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

Eso es todo. Afortunadamente estas muestras inspiren que y ayuda comenzar. Con la eficaz y el historial de rendimiento de espacios de almacenamiento directo, scripting cuentan con asistencia `Get-ClusterPerf` cmdlet, están habilitadas para pedir: y respuesta! : complejo preguntas que administra y supervisa la infraestructura de Windows Server 2019.

## Ver también

- [Introducción a Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/getting-started-with-windows-powershell)
- [Información general de Espacios de almacenamiento directos](storage-spaces-direct-overview.md)
- [Historial de rendimiento](performance-history.md)
