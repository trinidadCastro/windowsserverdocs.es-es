---
title: Historial de rendimiento de espacios de almacenamiento directo
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 828a3265c9770bab0158067c4f856866d03e3d42
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239262"
---
# Historial de rendimiento de espacios de almacenamiento directo

> Se aplica a: Windows Server 2019

Historial de rendimiento es una característica nueva que ofrece a los administradores de [Espacios de almacenamiento directo](storage-spaces-direct-overview.md) acceso fácil a cálculo histórica, memoria, red y las medidas de almacenamiento en todos los servidores host, unidades, volúmenes, las máquinas virtuales y mucho más. Historial de rendimiento se recopila automáticamente y se almacena en el clúster de hasta un año.

   > [!IMPORTANT]
   > Esta característica es nueva en Windows Server 2019. No está disponible en Windows Server 2016.

## Introducción

Historial de rendimiento se recopila de forma predeterminada con espacios de almacenamiento directo en Windows Server 2019. No es necesario instalar, configurar o iniciar cualquier cosa. No se requiere una conexión a Internet, no se requiere System Center y una base de datos externa no es necesario.

Para ver el rendimiento del clúster historial gráficamente, usa [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md):

![Historial de rendimiento en Windows Admin Center](media/performance-history/perf-history-in-wac.png)

Para consultar y procesarlo mediante programación, usa la nueva `Get-ClusterPerf` cmdlet. Ver el [uso de PowerShell](#usage-in-powershell).

## ¿Qué se recopilan

Historial de rendimiento se recopila 7 tipos de objetos:

![Tipos de objetos](media/performance-history/types-of-object.png)

Cada tipo de objeto tiene muchas serie: por ejemplo, `ClusterNode.Cpu.Usage` se recopilan para cada servidor.

Para obtener información detallada de lo que se recopilan para cada tipo de objeto y cómo interpretarlos, consulta estos temas secundarias:

| Objeto             | Serie                                                                               |
|--------------------|--------------------------------------------------------------------------------------|
| Unidades             | [Lo que se recopilan para las unidades](performance-history-for-drives.md)                     |
| Adaptadores de red   | [¿Qué se recopilan de los adaptadores de red](performance-history-for-network-adapters.md) |
| Servidores            | [Lo que se recopilan para servidores](performance-history-for-servers.md)                   |
| Discos duros virtuales | [¿Qué se recopilan de los discos duros virtuales](performance-history-for-vhds.md)           |
| Máquinas virtuales   | [Lo que se recopilan para máquinas virtuales](performance-history-for-vms.md)              |
| Volúmenes            | [¿Qué se recopilan volúmenes](performance-history-for-volumes.md)                   |
| Clústeres           | [Lo que se recopilan para clústeres](performance-history-for-clusters.md)                 |

Número de serie se agregan a través de objetos de sistema del mismo nivel para sus elementos primarios: por ejemplo, `NetAdapter.Bandwidth.Inbound` se recopilan por separado para cada adaptador de red y se agregan al servidor general; de igual modo `ClusterNode.Cpu.Usage` se agregan al clúster general; y así sucesivamente.

## Períodos de tiempo

Se almacena el historial de rendimiento de hasta un año, con la granularidad de tipo. Para la hora más reciente, las mediciones están disponibles cada 10 segundos. A partir de entonces, se combinan inteligente (promediando o sumar, según corresponda) en la serie menos detallado que abarcan más tiempo. Para el día más reciente, las mediciones están disponibles cada cinco minutos; para la semana más reciente, cada quince minutos; y así sucesivamente.

En Windows Admin Center, puedes seleccionar el período de tiempo en la esquina superior derecha encima del gráfico.

![Períodos de tiempo en Windows Admin Center](media/performance-history/timeframes-in-honolulu.png)

En PowerShell, usa el `-TimeFrame` parámetro.

Estos son los períodos de tiempo disponibles:

| Período de tiempo   | Frecuencia de medición | Retener |
|-------------|-----------------------|--------------|
| `LastHour`  | Cada 10 segundos         | 1 hora       |
| `LastDay`   | Cada 5 minutos       | 25 horas     |
| `LastWeek`  | Cada 15 minutos      | 8 días       |
| `LastMonth` | Cada 1 hora          | 35 días      |
| `LastYear`  | Todos los días 1           | 400 días     |

## Uso de PowerShell

Usa el `Get-ClusterPerformanceHistory` cmdlet al proceso y consulta Historial de rendimiento en PowerShell.

```PowerShell
Get-ClusterPerformanceHistory
```

   > [!TIP]
   > Usar el alias de **Get-ClusterPerf** para guardar algunas pulsaciones de teclas.

### Ejemplo

Obtener el uso de CPU de máquina virtual *MyVM* de la última hora:

```PowerShell
Get-VM "MyVM" | Get-ClusterPerf -VMSeriesName "VM.Cpu.Usage" -TimeFrame LastHour
```

Para obtener ejemplos más avanzados, consulta el publicado [scripts de muestra](performance-history-scripting.md) que se ofrece código de inicio para buscar los valores de pico, calcular medias, las líneas de tendencia de trazado, ejecutar a aislados de detección y mucho más.

### Especificar el objeto

Puedes especificar el objeto que desea la canalización. Esto funciona con 7 tipos de objetos:

| Objeto de canalización | Ejemplo     |
|----------------------|-------------|
| `Get-PhysicalDisk`   | <code>Get-PhysicalDisk -SerialNumber "XYZ456" &#124; Get-ClusterPerf</code>         |
| `Get-NetAdapter`     | <code>Get-NetAdapter "Ethernet" &#124; Get-ClusterPerf</code>                       |
| `Get-ClusterNode`    | <code>Get-ClusterNode "Server123" &#124; Get-ClusterPerf</code>                     |
| `Get-VHD`            | <code>Get-VHD "C:\ClusterStorage\MyVolume\MyVHD.vhdx" &#124; Get-ClusterPerf</code> |
| `Get-VM`             | <code>Get-VM "MyVM" &#124; Get-ClusterPerf</code>                                   |
| `Get-Volume`         | <code>Get-Volume -FriendlyName "MyVolume"  &#124; Get-ClusterPerf</code>            |
| `Get-Cluster`        | <code>Get-Cluster "MyCluster" &#124; Get-ClusterPerf</code>                         |

Si no se especifica, se devuelve el historial de rendimiento para el clúster general.

### Especificar la serie

Puedes especificar la serie que desee con estos parámetros:


| Parámetro                 | Ejemplo                       | Lista                                                                                 |
|---------------------------|-------------------------------|--------------------------------------------------------------------------------------|
| `-PhysicalDiskSeriesName` | `"PhysicalDisk.Iops.Read"`    | [Lo que se recopilan para las unidades](performance-history-for-drives.md)                     |
| `-NetAdapterSeriesName`   | `"NetAdapter.Bandwidth.Outbound"` | [¿Qué se recopilan de los adaptadores de red](performance-history-for-network-adapters.md) |
| `-ClusterNodeSeriesName`  | `"ClusterNode.Cpu.Usage"`     | [Lo que se recopilan para servidores](performance-history-for-servers.md)                   |
| `-VHDSeriesName`          | `"Vhd.Size.Current"`          | [¿Qué se recopilan de los discos duros virtuales](performance-history-for-vhds.md)           |
| `-VMSeriesName`           | `"Vm.Memory.Assigned"`        | [Lo que se recopilan para máquinas virtuales](performance-history-for-vms.md)              |
| `-VolumeSeriesName`       | `"Volume.Latency.Write"`      | [¿Qué se recopilan volúmenes](performance-history-for-volumes.md)                   |
| `-ClusterSeriesName`      | `"PhysicalDisk.Size.Total"`   | [Lo que se recopilan para clústeres](performance-history-for-clusters.md)                 |


   > [!TIP]
   > Usar TAB para detectar serie disponible.

Si no se especifica, se devuelve cada serie disponible para el objeto especificado.

### Especificar el período de tiempo

Puedes especificar el período de tiempo del historial que desee con el `-TimeFrame` parámetro.

   > [!TIP]
   > Usar TAB para detectar plazos de pago disponibles.

Si no se especifica, el `MostRecent` medición se devuelve.

## Funcionamiento

### Almacenamiento de historial de rendimiento

Poco después de que se habilita espacios de almacenamiento directo, un volumen GB aproximadamente 10 denominado `ClusterPerformanceHistory` se crea y una instancia del motor de almacenamiento Extensible (también conocido como Microsoft JET) se aprovisiona allí. Esta base de datos ligera almacena el historial de rendimiento sin ninguna intervención del administrador o la administración.

![Volumen para el almacenamiento del historial de rendimiento](media/performance-history/perf-history-volume.png)

El volumen está respaldado por espacios de almacenamiento y usa reflejo bidireccional, simple o resistencia de reflejo triple, según el número de nodos del clúster. Se repara después de errores de unidad o el servidor al igual que cualquier otro volumen de espacios de almacenamiento directo.

El volumen usa a ReFS pero no es volumen compartido de clúster (CSV), por lo que solo aparece en el nodo de propietario del grupo de clúster. Además de que se creen automáticamente, que no hay nada especial acerca de este volumen: verla, navegar en él, cambiar el tamaño o eliminarla (no recomendado). Si algo va mal, consulte la [solución de problemas](#troubleshooting). 

### Colección de detección y los datos de objeto

Historial de rendimiento automáticamente detecta los objetos relevantes, como las máquinas virtuales, en cualquier lugar en el clúster y empieza a transmitir los contadores de rendimiento. Los contadores de se agregan, sincronizados e inserta en la base de datos. Streaming se ejecuta de forma continua y está optimizado para el impacto del sistema mínimos.

Colección se controla mediante el servicio de mantenimiento, que está altamente disponible: si el nodo que se está ejecutando deja de funcionar, reanudará momentos más adelante en otro nodo del clúster. Historial de rendimiento puede transcurrir brevemente, pero se reanudará automáticamente. Puedes ver el servicio de mantenimiento y su nodo propietario mediante la ejecución de `Get-ClusterResource Health` en PowerShell.

### Diferencias de medidas de control

Cuando las medidas se combinan en serie menos detallado que abarcan más tiempo, como se describe en [períodos de tiempo](#Timeframes), se excluirán los períodos de datos que faltan. Por ejemplo, si el servidor estaba inactivo durante 30 minutos, a continuación, ejecución al 50% de CPU durante los próximos 30 minutos, el `ClusterNode.Cpu.Usage` promedio de la hora se registrará correctamente como 50% (no 25%).

### Personalización y extensibilidad

Historial de rendimiento es fácil de usar secuencias de comandos. Usar PowerShell para cualquier historial disponible directamente desde la base de datos para generar informes automatizados o alertas, exportar historial por motivos de seguridad de extracción, roll tu propio visualizaciones, etcetera. Consulta los publicado [scripts de muestra](performance-history-scripting.md) de código de inicio útil.

No es posible recopilar el historial de objetos adicionales, períodos de tiempo o serie.

La frecuencia de medición y el período de retención no están configurables actualmente.

## Inicie o detenga el historial de rendimiento

### ¿Cómo puedo habilitar esta característica?

A menos que `Stop-ClusterPerformanceHistory`, historial de rendimiento está habilitado de manera predeterminada.

Para volver a habilitarlo, ejecuta este cmdlet de PowerShell como administrador:

```PowerShell
Start-ClusterPerformanceHistory
```

### ¿Cómo se deshabilita esta característica?

Para detener la recopilación de historial de rendimiento, ejecuta este cmdlet de PowerShell como administrador:

```PowerShell
Stop-ClusterPerformanceHistory
```

Para eliminar las medidas existentes, usa el `-DeleteHistory` marca:

```PowerShell
Stop-ClusterPerformanceHistory -DeleteHistory
```

   > [!TIP]
   > Durante la implementación inicial, puedes impedir que historial de rendimiento de inicio estableciendo la `-CollectPerformanceHistory` parámetro de `Enable-ClusterStorageSpacesDirect` a `$False`.

## Solución de problemas

### El cmdlet no funciona

Un mensaje de error como "*el término ' Get-ClusterPerf' no se reconoce como el nombre de un cmdlet*" significa que la característica no está disponible o instalado. Comprueba que tienes compilación 17692 o posterior de Windows Server Insider Preview, que has instalado clústeres de conmutación por error, y que estás ejecutando espacios de almacenamiento directo.

   > [!NOTE]
   > Esta característica no está disponible en Windows Server 2016 o versiones anteriores.

### No hay datos disponibles 

Si un gráfico muestra "*no hay datos disponibles*" como en la imagen, aquí te mostramos cómo solucionar problemas:

![No hay datos disponibles](media/performance-history/no-data-available.png)

1. Si el objeto recién se ha agregado o creado, espera que se detectan (hasta 15 minutos).

2. La página o esperar para la siguiente actualización en segundo plano (hasta 30 segundos).

3. Ciertos objetos especiales se excluyen de historial de rendimiento: por ejemplo, las máquinas virtuales que no están en un clúster y volúmenes que no usan el sistema de archivos de volumen compartido de clúster (CSV). Consulta el tema secundarias para el tipo de objeto, como el [historial de rendimiento de volúmenes](performance-history-for-volumes.md), para la impresión preciso.

4. Si el problema persiste, abre PowerShell como administrador y ejecuta el `Get-ClusterPerf` cmdlet. El cmdlet incluye la solución de problemas de lógica para identificar problemas comunes, por ejemplo, si falta el volumen de ClusterPerformanceHistory y se proporcionan instrucciones de corrección.

5. Si el comando en el paso anterior no devuelve nada, puedes intentar reiniciar el servicio de mantenimiento (que recopila el historial de rendimiento) mediante la ejecución de `Stop-ClusterResource Health ; Start-ClusterResource Health` en PowerShell.

## Consulta también

- [Información general de Espacios de almacenamiento directos](storage-spaces-direct-overview.md)
