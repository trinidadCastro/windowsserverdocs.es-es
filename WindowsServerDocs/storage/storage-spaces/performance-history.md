---
description: 'Más información sobre: historial de rendimiento para Espacios de almacenamiento directo'
title: Historial de rendimiento de Espacios de almacenamiento directo
ms.author: cosdar
manager: eldenc
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.openlocfilehash: b90f010d45dc9e9013c2bc661232fb444247b661
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97048923"
---
# <a name="performance-history-for-storage-spaces-direct"></a>Historial de rendimiento de Espacios de almacenamiento directo

> Se aplica a: Windows Server 2019

El historial de rendimiento es una nueva característica que proporciona a los administradores de [espacios de almacenamiento directo](storage-spaces-direct-overview.md) acceso sencillo a las mediciones históricas de proceso, memoria, red y almacenamiento en servidores host, unidades, volúmenes, máquinas virtuales, etc. El historial de rendimiento se recopila automáticamente y se almacena en el clúster durante un año como máximo.

   > [!IMPORTANT]
   > Esta característica es nueva en Windows Server 2019. No está disponible en Windows Server 2016.

## <a name="get-started"></a>Introducción

El historial de rendimiento se recopila de forma predeterminada con Espacios de almacenamiento directo en Windows Server 2019. No es necesario instalar, configurar ni iniciar nada. No se requiere una conexión a Internet, System Center no es necesario y no se necesita una base de datos externa.

Para ver el historial de rendimiento del clúster de forma gráfica, use el [centro de administración de Windows](../../manage/windows-admin-center/overview.md):

![Historial de rendimiento del centro de administración de Windows](media/performance-history/perf-history-in-wac.png)

Para consultar y procesarlo mediante programación, use el `Get-ClusterPerf` cmdlet New. Consulte [uso en PowerShell](#usage-in-powershell).

## <a name="whats-collected"></a>Qué se recopila

El historial de rendimiento se recopila para 7 tipos de objetos:

![Tipos de objetos](media/performance-history/types-of-object.png)

Cada tipo de objeto tiene muchas series: por ejemplo, `ClusterNode.Cpu.Usage` se recopila para cada servidor.

Para obtener detalles de lo que se recopila para cada tipo de objeto y cómo interpretarlos, consulte estos temas secundarios:

| Object             | Serie                                                                               |
|--------------------|--------------------------------------------------------------------------------------|
| Unidades             | [Lo que se recopila para las unidades](performance-history-for-drives.md)                     |
| Adaptadores de red   | [Lo que se recopila para los adaptadores de red](performance-history-for-network-adapters.md) |
| Servidores            | [Qué se recopila para los servidores](performance-history-for-servers.md)                   |
| Discos duros virtuales | [Lo que se recopila para los discos duros virtuales](performance-history-for-vhds.md)           |
| Máquinas virtuales   | [Lo que se recopila para las máquinas virtuales](performance-history-for-vms.md)              |
| Volúmenes            | [Qué se recopila para los volúmenes](performance-history-for-volumes.md)                   |
| Clústeres           | [Lo que se recopila para los clústeres](performance-history-for-clusters.md)                 |

Muchas series se agregan entre los objetos del mismo nivel a su elemento primario: por ejemplo, `NetAdapter.Bandwidth.Inbound` se recopila por separado para cada adaptador de red y se agrega al servidor general; de igual modo, `ClusterNode.Cpu.Usage` se agrega al clúster general, y así sucesivamente.

## <a name="timeframes"></a>Períodos

El historial de rendimiento se almacena hasta un año, con granularidad más reducida. En el caso de la hora más reciente, las medidas están disponibles cada diez segundos. A partir de ese momento, se combinan de forma inteligente (al calcular el promedio o sumar, según corresponda) en series menos granulares que abarcan más tiempo. En el último día, las medidas están disponibles cada cinco minutos. en la semana más reciente, cada quince minutos; etc.

En el centro de administración de Windows, puede seleccionar el intervalo de tiempo en la parte superior derecha del gráfico.

![Períodos de tiempo en el centro de administración de Windows](media/performance-history/timeframes-in-honolulu.png)

En PowerShell, use el `-TimeFrame` parámetro.

Estos son los períodos de tiempo disponibles:

| Período de tiempo   | Frecuencia de medición | Se conserva para |
|-------------|-----------------------|--------------|
| `LastHour`  | Cada 10 segundos         | 1 hora       |
| `LastDay`   | Cada 5 minutos       | 25 horas     |
| `LastWeek`  | Cada 15 minutos      | 8 días       |
| `LastMonth` | Cada 1 hora          | 35 días      |
| `LastYear`  | Cada día           | 400 días     |

## <a name="usage-in-powershell"></a>Uso en PowerShell

Use el `Get-ClusterPerformanceHistory` cmdlet para consultar y procesar el historial de rendimiento en PowerShell.

```PowerShell
Get-ClusterPerformanceHistory
```

   > [!TIP]
   > Use el alias **Get-ClusterPerf** para guardar algunas pulsaciones de teclas.

### <a name="example"></a>Ejemplo

Obtiene el uso de CPU de la máquina virtual *MyVM* durante la última hora:

```PowerShell
Get-VM "MyVM" | Get-ClusterPerf -VMSeriesName "VM.Cpu.Usage" -TimeFrame LastHour
```

Para obtener ejemplos más avanzados, vea los [scripts de ejemplo](performance-history-scripting.md) publicados que proporcionan código de inicio para buscar valores máximos, calcular promedios, trazar líneas de tendencia, ejecutar la detección de valores atípicos y mucho más.

### <a name="specify-the-object"></a>Especificar el objeto

Puede especificar el objeto que desea la canalización. Esto funciona con 7 tipos de objetos:

| Objeto de la canalización | Ejemplo     |
|----------------------|-------------|
| `Get-PhysicalDisk`   | <code>Get-PhysicalDisk -SerialNumber "XYZ456" &#124; Get-ClusterPerf</code>         |
| `Get-NetAdapter`     | <code>Get-NetAdapter "Ethernet" &#124; Get-ClusterPerf</code>                       |
| `Get-ClusterNode`    | <code>Get-ClusterNode "Server123" &#124; Get-ClusterPerf</code>                     |
| `Get-VHD`            | <code>Get-VHD "C:\ClusterStorage\MyVolume\MyVHD.vhdx" &#124; Get-ClusterPerf</code> |
| `Get-VM`             | <code>Get-VM "MyVM" &#124; Get-ClusterPerf</code>                                   |
| `Get-Volume`         | <code>Get-Volume -FriendlyName "MyVolume"  &#124; Get-ClusterPerf</code>            |
| `Get-Cluster`        | <code>Get-Cluster "MyCluster" &#124; Get-ClusterPerf</code>                         |

Si no especifica, se devuelve el historial de rendimiento del clúster global.

### <a name="specify-the-series"></a>Especificar la serie

Puede especificar la serie que desea con estos parámetros:


| Parámetro                 | Ejemplo                       | List                                                                                 |
|---------------------------|-------------------------------|--------------------------------------------------------------------------------------|
| `-PhysicalDiskSeriesName` | `"PhysicalDisk.Iops.Read"`    | [Lo que se recopila para las unidades](performance-history-for-drives.md)                     |
| `-NetAdapterSeriesName`   | `"NetAdapter.Bandwidth.Outbound"` | [Lo que se recopila para los adaptadores de red](performance-history-for-network-adapters.md) |
| `-ClusterNodeSeriesName`  | `"ClusterNode.Cpu.Usage"`     | [Qué se recopila para los servidores](performance-history-for-servers.md)                   |
| `-VHDSeriesName`          | `"Vhd.Size.Current"`          | [Lo que se recopila para los discos duros virtuales](performance-history-for-vhds.md)           |
| `-VMSeriesName`           | `"Vm.Memory.Assigned"`        | [Lo que se recopila para las máquinas virtuales](performance-history-for-vms.md)              |
| `-VolumeSeriesName`       | `"Volume.Latency.Write"`      | [Qué se recopila para los volúmenes](performance-history-for-volumes.md)                   |
| `-ClusterSeriesName`      | `"PhysicalDisk.Size.Total"`   | [Lo que se recopila para los clústeres](performance-history-for-clusters.md)                 |


   > [!TIP]
   > Use la finalización con tabulación para detectar la serie disponible.

Si no especifica, se devolverán todas las series disponibles para el objeto especificado.

### <a name="specify-the-timeframe"></a>Especificar el período de tiempo

Puede especificar el período de tiempo del historial que desee con el `-TimeFrame` parámetro.

   > [!TIP]
   > Use la finalización con tabulación para detectar los períodos de tiempo disponibles.

Si no especifica, `MostRecent` se devuelve la medida.

## <a name="how-it-works"></a>Cómo funciona

### <a name="performance-history-storage"></a>Almacenamiento del historial de rendimiento

Poco después de habilitar Espacios de almacenamiento directo, se crea un volumen de aproximadamente 10 GB denominado `ClusterPerformanceHistory` y se aprovisiona una instancia del motor de almacenamiento extensible (también conocido como Microsoft Jet). Esta base de datos ligera almacena el historial de rendimiento sin intervención ni administración de administradores.

![Volumen para el almacenamiento de historial de rendimiento](media/performance-history/perf-history-volume.png)

El volumen está respaldado por espacios de almacenamiento y usa un reflejo simple, bidireccional o de reflejo triple, en función del número de nodos del clúster de. Se repara después de los errores de la unidad o del servidor como cualquier otro volumen en Espacios de almacenamiento directo.

El volumen usa ReFS pero no es Volumen compartido de clúster (CSV), por lo que solo aparece en el nodo propietario del grupo de clústeres. Además de que se cree automáticamente, no hay nada especial sobre este volumen: puede verlo, examinarlo, cambiar su tamaño o eliminarlo (no recomendado). Si algo va mal, consulte [solución de problemas](#troubleshooting).

### <a name="object-discovery-and-data-collection"></a>Detección de objetos y recopilación de datos

El historial de rendimiento detecta automáticamente los objetos relevantes, como las máquinas virtuales, en cualquier parte del clúster y comienza a transmitir sus contadores de rendimiento. Los contadores se agregan, sincronizan e insertan en la base de datos. El streaming se ejecuta de forma continua y está optimizado para un impacto mínimo en el sistema.

La colección la controla el Servicio de mantenimiento, que tiene una alta disponibilidad: Si el nodo en el que se está ejecutando deja de funcionar, se reanudará momentos más tarde en otro nodo del clúster. El historial de rendimiento puede caducar brevemente, pero se reanudará automáticamente. Puede ver el Servicio de mantenimiento y su nodo propietario mediante la ejecución `Get-ClusterResource Health` de en PowerShell.

### <a name="handling-measurement-gaps"></a>Controlar los huecos de medición

Cuando las medidas se combinan en una serie menos granular que abarca más tiempo, tal y como se describe en [intervalos](#timeframes)de tiempo, se excluyen los períodos de los datos que faltan. Por ejemplo, si el servidor estuvo inactivo durante 30 minutos y, a continuación, se ejecuta en el 50% de la CPU durante los próximos 30 minutos, el `ClusterNode.Cpu.Usage` promedio de la hora se registrará correctamente como 50% (no 25%).

### <a name="extensibility-and-customization"></a>Extensibilidad y personalización

El historial de rendimiento es compatible con scripts. Use PowerShell para extraer cualquier historial disponible directamente desde la base de datos para crear informes automatizados o alertas, exportar el historial para protegerlo, revertir sus propias visualizaciones, etc. Consulte los [scripts de ejemplo](performance-history-scripting.md) publicados para obtener código de inicio útil.

No es posible recopilar el historial de objetos, períodos o series adicionales.

La frecuencia de medición y el período de retención no se pueden configurar actualmente.

## <a name="start-or-stop-performance-history"></a>Iniciar o detener el historial de rendimiento

### <a name="how-do-i-enable-this-feature"></a>¿Cómo habilitar esta característica?

A menos que usted `Stop-ClusterPerformanceHistory` , el historial de rendimiento está habilitado de forma predeterminada.

Para volver a habilitarlo, ejecute este cmdlet de PowerShell como administrador:

```PowerShell
Start-ClusterPerformanceHistory
```

### <a name="how-do-i-disable-this-feature"></a>¿Cómo deshabilitar esta característica?

Para dejar de recopilar el historial de rendimiento, ejecute este cmdlet de PowerShell como administrador:

```PowerShell
Stop-ClusterPerformanceHistory
```

Para eliminar las medidas existentes, use la `-DeleteHistory` marca:

```PowerShell
Stop-ClusterPerformanceHistory -DeleteHistory
```

   > [!TIP]
   > Durante la implementación inicial, puede evitar que el historial de rendimiento se inicie estableciendo el `-CollectPerformanceHistory` parámetro de `Enable-ClusterStorageSpacesDirect` en `$False` .

## <a name="troubleshooting"></a>Solución de problemas

### <a name="the-cmdlet-doesnt-work"></a>El cmdlet no funciona

Un mensaje de error como "*el término ' Get-ClusterPerf ' no se reconoce como nombre de un cmdlet*" significa que la característica no está disponible o no está instalada. Compruebe que tiene Windows Server Insider Preview versión 17692 o posterior, que ha instalado los clústeres de conmutación por error y que está ejecutando Espacios de almacenamiento directo.

   > [!NOTE]
   > Esta característica no está disponible en Windows Server 2016 o versiones anteriores.

### <a name="no-data-available"></a>Sin datos disponibles

Si un gráfico muestra "*no hay datos disponibles*" como se ha explicado, aquí se indica cómo solucionar el problema:

![Sin datos disponibles](media/performance-history/no-data-available.png)

1. Si el objeto se ha agregado o creado recientemente, espere a que se detecte (hasta 15 minutos).

2. Actualice la página o espere a la siguiente actualización en segundo plano (hasta 30 segundos).

3. Algunos objetos especiales se excluyen del historial de rendimiento, por ejemplo, las máquinas virtuales que no están en clúster y los volúmenes que no usan el sistema de archivos de Volumen compartido de clúster (CSV). Compruebe el subtema para el tipo de objeto, como el [historial de rendimiento de los volúmenes](performance-history-for-volumes.md), para la impresión precisa.

4. Si el problema persiste, abra PowerShell como administrador y ejecute el `Get-ClusterPerf` cmdlet. El cmdlet incluye lógica de solución de problemas para identificar problemas comunes, como si falta el volumen ClusterPerformanceHistory, y proporciona instrucciones de corrección.

5. Si el comando del paso anterior no devuelve nada, puede intentar reiniciar el Servicio de mantenimiento (que recopila el historial de rendimiento) mediante la ejecución `Stop-ClusterResource Health ; Start-ClusterResource Health` de en PowerShell.

## <a name="additional-references"></a>Referencias adicionales

- [Introducción a Espacios de almacenamiento directo](storage-spaces-direct-overview.md)
