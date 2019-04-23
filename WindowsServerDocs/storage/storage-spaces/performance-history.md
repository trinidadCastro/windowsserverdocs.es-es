---
title: Historial de rendimiento de espacios de almacenamiento directo
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: Espacios de almacenamiento directo
ms.localizationpriority: medium
ms.openlocfilehash: 828a3265c9770bab0158067c4f856866d03e3d42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870866"
---
# <a name="performance-history-for-storage-spaces-direct"></a>Historial de rendimiento de espacios de almacenamiento directo

> Se aplica a: Windows Server 2019

Historial de rendimiento es una característica nueva que ofrece [espacios de almacenamiento directo](storage-spaces-direct-overview.md) los administradores acceso fácil a históricas mediciones de proceso, memoria, red y almacenamiento a través de los servidores host, unidades, volúmenes, máquinas virtuales y mucho más. Historial de rendimiento se recopilan automáticamente y se almacenan en el clúster de hasta un año.

   > [!IMPORTANT]
   > Esta característica es nueva en Windows Server 2019. No está disponible en Windows Server 2016.

## <a name="get-started"></a>Comenzar

Historial de rendimiento se recopila de forma predeterminada con espacios de almacenamiento directo en Windows Server 2019. No es necesario instalar, configurar o iniciar cualquier cosa. No se requiere una conexión a Internet, no se requiere System Center y no se requiere una base de datos externo.

Para ver el historial de rendimiento de su clúster gráficamente, use [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md):

![Historial de rendimiento en Windows Admin Center](media/performance-history/perf-history-in-wac.png)

Para consultar y procesarlo mediante programación, use la nueva `Get-ClusterPerf` cmdlet. Consulte [uso de PowerShell](#usage-in-powershell).

## <a name="whats-collected"></a>¿Qué información se recopila

Historial de rendimiento se recopila para los tipos de objetos 7:

![Tipos de objetos](media/performance-history/types-of-object.png)

Cada tipo de objeto tiene el número de series: por ejemplo, `ClusterNode.Cpu.Usage` se recopilan para cada servidor.

Para obtener información detallada de lo que se recopilan para cada tipo de objeto y cómo interpretarlos, consulte estos temas secundarios:

| Object             | serie                                                                               |
|--------------------|--------------------------------------------------------------------------------------|
| Unidades             | [¿Qué información se recopila para unidades de](performance-history-for-drives.md)                     |
| Adaptadores de red   | [¿Qué información se recopila para adaptadores de red](performance-history-for-network-adapters.md) |
| Servidores            | [¿Qué información se recopila para servidores](performance-history-for-servers.md)                   |
| Discos duros virtuales | [¿Qué información se recopila para discos duros virtuales](performance-history-for-vhds.md)           |
| Máquinas virtuales   | [¿Qué información se recopila para máquinas virtuales](performance-history-for-vms.md)              |
| Volúmenes            | [¿Qué información se recopila para volúmenes](performance-history-for-volumes.md)                   |
| Clústeres           | [¿Qué información se recopila para clústeres](performance-history-for-clusters.md)                 |

Número de series se agrega entre objetos del mismo nivel a su elemento principal: por ejemplo, `NetAdapter.Bandwidth.Inbound` se recopilan por separado para cada adaptador de red y se agregan al servidor general; igualmente `ClusterNode.Cpu.Usage` se agregan al clúster general; y así sucesivamente.

## <a name="timeframes"></a>Períodos de tiempo

Historial de rendimiento se almacena durante hasta un año con granularidad decreciente. Para obtener la hora más reciente, las medidas están disponibles cada diez segundos. A partir de entonces, se combinan inteligente (al calcular el promedio o sumar, según corresponda) en serie menos pormenorizado que abarcan más tiempo. Para el día más reciente, las medidas están disponibles cada cinco minutos; para la última semana, cada quince minutos; y así sucesivamente.

En Windows Admin Center, puede seleccionar el período de tiempo en la esquina superior derecha encima del gráfico.

![Períodos de tiempo en Windows Admin Center](media/performance-history/timeframes-in-honolulu.png)

En PowerShell, use el `-TimeFrame` parámetro.

Estos son los períodos de tiempo disponibles:

| Período de tiempo   | Frecuencia de medición | Conserva |
|-------------|-----------------------|--------------|
| `LastHour`  | Cada 10 segundos         | 1 hora       |
| `LastDay`   | Cada 5 minutos       | 25 horas     |
| `LastWeek`  | Cada 15 minutos      | 8 días       |
| `LastMonth` | Cada hora          | 35 días      |
| `LastYear`  | Cada día           | 400 días     |

## <a name="usage-in-powershell"></a>Uso en PowerShell

Use el `Get-ClusterPerformanceHistory` cmdlet al historial de rendimiento de consulta y proceso en PowerShell.

```PowerShell
Get-ClusterPerformanceHistory
```

   > [!TIP]
   > Use la **Get ClusterPerf** alias para guardar algunas pulsaciones de teclas.

### <a name="example"></a>Ejemplo

Obtener el uso de CPU de la máquina virtual *MyVM* durante la última hora:

```PowerShell
Get-VM "MyVM" | Get-ClusterPerf -VMSeriesName "VM.Cpu.Usage" -TimeFrame LastHour
```

Para obtener ejemplos más avanzados, vea publicado [scripts de ejemplo](performance-history-scripting.md) que proporcionan código de inicio para buscar valores máximos, calcular promedios, trazar líneas de tendencia, ejecute valores atípicos, detección y mucho más.

### <a name="specify-the-object"></a>Especifique el objeto

Puede especificar el objeto que desea que la canalización. Esto funciona con 7 tipos de objetos:

| Objeto de canalización | Ejemplo     |
|----------------------|-------------|
| `Get-PhysicalDisk`   | <code>Get-PhysicalDisk -SerialNumber "XYZ456" &#124; Get-ClusterPerf</code>         |
| `Get-NetAdapter`     | <code>Get-NetAdapter "Ethernet" &#124; Get-ClusterPerf</code>                       |
| `Get-ClusterNode`    | <code>Get-ClusterNode "Server123" &#124; Get-ClusterPerf</code>                     |
| `Get-VHD`            | <code>Get-VHD "C:\ClusterStorage\MyVolume\MyVHD.vhdx" &#124; Get-ClusterPerf</code> |
| `Get-VM`             | <code>Get-VM "MyVM" &#124; Get-ClusterPerf</code>                                   |
| `Get-Volume`         | <code>Get-Volume -FriendlyName "MyVolume"  &#124; Get-ClusterPerf</code>            |
| `Get-Cluster`        | <code>Get-Cluster "MyCluster" &#124; Get-ClusterPerf</code>                         |

Si no se especifica, se devuelve el historial de rendimiento para el clúster en general.

### <a name="specify-the-series"></a>Especificar la serie

Puede especificar la serie que desea con estos parámetros:


| Parámetro                 | Ejemplo                       | Lista                                                                                 |
|---------------------------|-------------------------------|--------------------------------------------------------------------------------------|
| `-PhysicalDiskSeriesName` | `"PhysicalDisk.Iops.Read"`    | [¿Qué información se recopila para unidades de](performance-history-for-drives.md)                     |
| `-NetAdapterSeriesName`   | `"NetAdapter.Bandwidth.Outbound"` | [¿Qué información se recopila para adaptadores de red](performance-history-for-network-adapters.md) |
| `-ClusterNodeSeriesName`  | `"ClusterNode.Cpu.Usage"`     | [¿Qué información se recopila para servidores](performance-history-for-servers.md)                   |
| `-VHDSeriesName`          | `"Vhd.Size.Current"`          | [¿Qué información se recopila para discos duros virtuales](performance-history-for-vhds.md)           |
| `-VMSeriesName`           | `"Vm.Memory.Assigned"`        | [¿Qué información se recopila para máquinas virtuales](performance-history-for-vms.md)              |
| `-VolumeSeriesName`       | `"Volume.Latency.Write"`      | [¿Qué información se recopila para volúmenes](performance-history-for-volumes.md)                   |
| `-ClusterSeriesName`      | `"PhysicalDisk.Size.Total"`   | [¿Qué información se recopila para clústeres](performance-history-for-clusters.md)                 |


   > [!TIP]
   > Utilice el tabulador para descubrir serie disponible.

Si no se especifica, se devuelve cada serie está disponible para el objeto especificado.

### <a name="specify-the-timeframe"></a>Especifique el período de tiempo

Puede especificar el período de tiempo del historial que quiera con el `-TimeFrame` parámetro.

   > [!TIP]
   > Utilice el tabulador para detectar los períodos de tiempo disponible.

Si no se especifica, el `MostRecent` se devuelve la medida.

## <a name="how-it-works"></a>Cómo funciona

### <a name="performance-history-storage"></a>Almacenamiento del historial de rendimiento

Poco después de habilitar espacios de almacenamiento directo, un volumen GB aproximadamente 10 denominado `ClusterPerformanceHistory` se crea y se aprovisiona una instancia del motor de almacenamiento Extensible (también conocido como Microsoft JET) no existe. Esta base de datos ligera almacena el historial de rendimiento sin ninguna intervención del administrador o administración.

![Volumen de almacenamiento del historial de rendimiento](media/performance-history/perf-history-volume.png)

El volumen está respaldado por espacios de almacenamiento y utiliza un reflejo bidireccional de simple, o resistencia de reflejo triple, dependiendo del número de nodos del clúster. Se repara después de errores de unidad o el servidor al igual que cualquier otro volumen en espacios de almacenamiento directo.

El volumen usa a ReFS pero no es volumen de compartidos de clúster (CSV), por lo que solo aparece en el nodo de propietario del grupo de clúster. Además de que se creen automáticamente, que no hay nada especial acerca de este volumen: puede verlo, examinarla, cambiar su tamaño o eliminarlo (no recomendado). Si algo va mal, consulte [Troubleshooting](#troubleshooting). 

### <a name="object-discovery-and-data-collection"></a>Recopilación de datos y detección de objeto

Historial de rendimiento automáticamente detecta los objetos relevantes, como las máquinas virtuales, en cualquier lugar en el clúster y comienza a transmitir sus contadores de rendimiento. Los contadores se agregan, sincronizados e insertados en la base de datos. Transmisión por secuencias se ejecuta continuamente y está optimizado para el impacto del sistema mínimos.

Colección se controla mediante el servicio de mantenimiento, que es de alta disponibilidad: si el nodo donde se está ejecutando deja de funcionar, reanudará momentos más tarde en otro nodo del clúster. Puede transcurrir brevemente un historial de rendimiento, pero se reanudará automáticamente. Puede ver el servicio de mantenimiento y su nodo propietario mediante la ejecución de `Get-ClusterResource Health` en PowerShell.

### <a name="handling-measurement-gaps"></a>Controlar los intervalos de medición

Cuando se combinan las mediciones en serie menos pormenorizado que abarcan más tiempo, como se describe en [los períodos de tiempo](#Timeframes), se excluyen los puntos de datos que faltan. Por ejemplo, si el servidor estaba inactivo durante 30 minutos, a continuación, ejecutar al 50% de CPU durante los próximos 30 minutos, el `ClusterNode.Cpu.Usage` promedio para la hora se registrará correctamente como (no un 25%) del 50%.

### <a name="extensibility-and-customization"></a>Extensibilidad y personalización

Historial de rendimiento es compatible con scripting. Usar PowerShell para extraer ningún historial disponible directamente desde la base de datos para generar informes automatizados o alertas, exportar historial por motivos de seguridad, restaurar sus propias visualizaciones, etcetera. Vea publicado [scripts de ejemplo](performance-history-scripting.md) para código de inicio útil.

No es posible recopilar historial para los objetos adicionales, los períodos de tiempo o series.

La frecuencia de medición y el período de retención no son configurables actualmente.

## <a name="start-or-stop-performance-history"></a>Iniciar o detener el historial de rendimiento

### <a name="how-do-i-enable-this-feature"></a>¿Cómo se puede habilitar esta característica?

A menos que `Stop-ClusterPerformanceHistory`, historial de rendimiento está habilitado de forma predeterminada.

Para volver a habilitarla, ejecute este cmdlet de PowerShell como administrador:

```PowerShell
Start-ClusterPerformanceHistory
```

### <a name="how-do-i-disable-this-feature"></a>¿Cómo se puede deshabilitar esta característica?

Para detener la recopilación de historial de rendimiento, ejecute este cmdlet de PowerShell como administrador:

```PowerShell
Stop-ClusterPerformanceHistory
```

Para eliminar las medidas existentes, use el `-DeleteHistory` marca:

```PowerShell
Stop-ClusterPerformanceHistory -DeleteHistory
```

   > [!TIP]
   > Durante la implementación inicial, puede impedir que el historial de rendimiento a partir de estableciendo el `-CollectPerformanceHistory` parámetro de `Enable-ClusterStorageSpacesDirect` a `$False`.

## <a name="troubleshooting"></a>Solución de problemas

### <a name="the-cmdlet-doesnt-work"></a>El cmdlet no funciona

Al igual que un mensaje de error "*el término 'Get-ClusterPerf' no se reconoce como el nombre de un cmdlet*" significa que la característica no está disponible o está instalada. Compruebe que tiene la compilación de Windows Server Insider Preview 17692 o posterior, que ha instalado la agrupación en clústeres de conmutación por error y que ejecuta espacios de almacenamiento directo.

   > [!NOTE]
   > Esta característica no está disponible en Windows Server 2016 o una versión anterior.

### <a name="no-data-available"></a>No hay datos disponibles 

Si se muestra un gráfico "*no hay datos disponibles*" como se muestra, aquí le mostramos cómo solucionar problemas:

![No hay datos disponibles](media/performance-history/no-data-available.png)

1. Si el objeto se agregó o se creó recientemente, espere para que se pueda detectar (hasta 15 minutos).

2. Actualice la página o esperar a que la próxima actualización en segundo plano (hasta 30 segundos).

3. Se excluyen algunos objetos especiales de historial de rendimiento: por ejemplo, las máquinas virtuales que no están en clúster y los volúmenes que no usan el sistema de archivos del volumen compartido de clúster (CSV). Compruebe el subtema para el tipo de objeto, como [historial de rendimiento para los volúmenes](performance-history-for-volumes.md), para la letra chica.

4. Si el problema persiste, abra PowerShell como administrador y ejecute el `Get-ClusterPerf` cmdlet. El cmdlet incluye la solución de problemas de lógica para identificar los problemas comunes, por ejemplo, si falta el volumen ClusterPerformanceHistory y proporciona instrucciones de corrección.

5. Si el comando en el paso anterior no devuelve nada, puede intentar reiniciar el servicio de mantenimiento (que recopila el historial de rendimiento) mediante la ejecución de `Stop-ClusterResource Health ; Start-ClusterResource Health` en PowerShell.

## <a name="see-also"></a>Vea también

- [Información general de espacios directo de almacenamiento](storage-spaces-direct-overview.md)
