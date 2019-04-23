---
title: Recopilar datos de diagnóstico con espacios de almacenamiento directo
description: Descripción de espacios directo datos colección de herramientas de almacenamiento, con ejemplos específicos de cómo ejecutar y usarlos.
keywords: Espacios de almacenamiento, la recopilación de datos, solución de problemas, los canales de eventos, Get-SDDCDiagnosticInfo
ms.assetid: ''
ms.prod: windows-server-threshold
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 10/24/2018
ms.localizationpriority: ''
ms.openlocfilehash: eaa7d92fe6f77697614cacf1405a25e5a42e14b7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880276"
---
# <a name="collect-diagnostic-data-with-storage-spaces-direct"></a>Recopilar datos de diagnóstico con espacios de almacenamiento directo

> Se aplica a: Windows Server 2019, Windows Server 2016

Existen diversas herramientas de diagnóstico que pueden usarse para recopilar los datos necesarios para solucionar problemas de espacios de almacenamiento directo y clúster de conmutación por error. En este artículo, nos centraremos en **Get SDDCDiagnosticInfo** -una herramienta de una entrada táctil que recopilará toda la información relevante para ayudarle a diagnosticar el clúster.

<!-- The health summary report is a great start to understanding the status of your system to start diagnosing an issue. -->

Dado que los registros y otra información que **Get SDDCDiagnosticInfo** son denso, será útil para la solución avanzada de problemas que se han transferido y que es posible que la información sobre cómo solucionar problemas presentados a continuación requieren datos que se enviarán a Microsoft para evaluar las prioridades.

<!--
## Collecting live dumps

Windows will trigger the collection of a ``` LiveDump ``` when there are known resources that are hanging in kernel calls. ``` RHS ``` will trigger ```LiveDump``` collection if both the resource type and cluster ``` DumpPolicy ``` are set to 1. For physical disk it is set out of the box
-->

## <a name="installing-get-sddcdiagnosticinfo"></a>Instalación de Get-SDDCDiagnosticInfo

El **Get SDDCDiagnosticInfo** cmdlet de PowerShell (también conocida como **Get-PCStorageDiagnosticInfo**, anteriormente conocido como **prueba StorageHealth**) puede usarse para recopilar los registros de y realizar comprobaciones de estado de agrupación en clústeres de conmutación por error (clúster, recursos, redes, los nodos), espacios de almacenamiento () Clúster de discos físicos, contenedores, discos virtuales), volúmenes compartidos, recursos compartidos de archivos SMB y la desduplicación. 

Hay dos métodos de instalación de la secuencia de comandos, ambos son se describen a continuación.

### <a name="powershell-gallery"></a>PowerShell Gallery

El [Galería de PowerShell](https://www.powershellgallery.com/packages/PrivateCloud.DiagnosticInfo) es una instantánea del repositorio de GitHub. Tenga en cuenta que para instalar elementos desde la Galería de PowerShell requiere la versión más reciente del módulo PowerShellGet, que está disponible en Windows 10, en Windows Management Framework (WMF) 5.0 o en el instalador basado en MSI (para PowerShell 3 y 4).

Puede instalar el módulo ejecutando el comando siguiente en PowerShell con privilegios de administrador:

``` PowerShell
Install-PackageProvider NuGet -Force
Install-Module PrivateCloud.DiagnosticInfo -Force
Import-Module PrivateCloud.DiagnosticInfo -Force
```

Para actualizar el módulo, ejecute el siguiente comando en PowerShell:

``` PowerShell
Update-Module PrivateCloud.DiagnosticInfo
```

### <a name="github"></a>GitHub

El [repositorio de GitHub](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/) es la versión más actualizada del módulo, ya que nos estamos continuamente iteración aquí. Para instalar el módulo desde GitHub, descargue el módulo más reciente desde el [archivo](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/archive/master.zip) y extraiga el directorio PrivateCloud.DiagnosticInfo a la ruta correcta de los módulos de PowerShell apuntado ```$env:PSModulePath```

``` PowerShell
# Allowing Tls12 and Tls11 -- e.g. github now requires Tls12
# If this is not set, the Invoke-WebRequest fails with "The request was aborted: Could not create SSL/TLS secure channel."
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
$module = 'PrivateCloud.DiagnosticInfo'
Invoke-WebRequest -Uri https://github.com/PowerShell/$module/archive/master.zip -OutFile $env:TEMP\master.zip
Expand-Archive -Path $env:TEMP\master.zip -DestinationPath $env:TEMP -Force
if (Test-Path $env:SystemRoot\System32\WindowsPowerShell\v1.0\Modules\$module) {
    rm -Recurse $env:SystemRoot\System32\WindowsPowerShell\v1.0\Modules\$module -ErrorAction Stop
    Remove-Module $module -ErrorAction SilentlyContinue
} else {
    Import-Module $module -ErrorAction SilentlyContinue
} 
if (-not ($m = Get-Module $module -ErrorAction SilentlyContinue)) {
    $md = "$env:ProgramFiles\WindowsPowerShell\Modules"
} else {
    $md = (gi $m.ModuleBase -ErrorAction SilentlyContinue).PsParentPath
    Remove-Module $module -ErrorAction SilentlyContinue
    rm -Recurse $m.ModuleBase -ErrorAction Stop
}
cp -Recurse $env:TEMP\$module-master\$module $md -Force -ErrorAction Stop
rm -Recurse $env:TEMP\$module-master,$env:TEMP\master.zip
Import-Module $module -Force

``` 

Si necesita obtener este módulo en un clúster sin conexión, descargue el archivo zip, muévalo a su nodo de servidor de destino e instale el módulo.

## <a name="gathering-logs"></a>Recopilando registros

Después de haber habilitado los canales de eventos y completar el proceso de instalación, puede usar el cmdlet de PowerShell Get-SDDCDiagnosticInfo en el módulo para obtener:

- Informes de mantenimiento de almacenamiento, además de obtener más información sobre los componentes en mal estado
- Informes de capacidad de almacenamiento por grupo, el volumen y el volumen desduplicado
- Registros de eventos de todos los nodos del clúster y un informe de resumen de error

Se supone que el clúster de almacenamiento tiene el nombre *"CLUS01".*

Que se ejecuta en un clúster de almacenamiento remoto:

``` PowerShell
Get-SDDCDiagnosticInfo -ClusterName CLUS01
```

Ejecute localmente en el nodo de almacenamiento en clúster:

``` PowerShell
Get-SDDCDiagnosticInfo
```

Para guardar los resultados en una carpeta especificada:

``` PowerShell
Get-SDDCDiagnosticInfo -WriteToPath D:\Folder 
```

Este es un ejemplo del aspecto de esto en un clúster real:

``` PowerShell
New-Item -Name SDDCDiagTemp -Path d:\ -ItemType Directory -Force
Get-SddcDiagnosticInfo -ClusterName S2D-Cluster -WriteToPath d:\SDDCDiagTemp
```

Como puede ver, el script también hará validación del estado actual del clúster

![captura de pantalla de powershell de recopilación de datos](media/data-collection/CollectData.png)

Como puede ver, todos los datos se escriben en la carpeta SDDCDiagTemp

![datos de captura de pantalla de explorador de archivos](media/data-collection/CollectDataFolder.png)

Después de que se finalizará la secuencia de comandos, creará ZIP en el directorio de usuarios

![datos zip en la captura de pantalla de powershell](media/data-collection/CollectDataResult.png)

Vamos a generar un informe en un archivo de texto

```PowerShell
#find the latest diagnostic zip in UserProfile
    $DiagZip=(get-childitem $env:USERPROFILE | where Name -like HealthTest*.zip)
    $LatestDiagPath=($DiagZip | sort lastwritetime | select -First 1).FullName
#expand to temp directory
    New-Item -Name SDDCDiagTemp -Path d:\ -ItemType Directory -Force
    Expand-Archive -Path $LatestDiagPath -DestinationPath D:\SDDCDiagTemp -Force
#generate report and save to text file
    $report=Show-SddcDiagnosticReport -Path D:\SDDCDiagTemp
    $report | out-file d:\SDDCReport.txt
    
```

Como referencia, este es un vínculo a la [informe de ejemplo](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/SDDCReport.txt) y [zip de ejemplo](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/HealthTest-S2D-Cluster-20180522-1546.ZIP).

Para ver esto en Windows Admin Center (versión 1812 y versiones posteriores), vaya a la *diagnósticos* ficha. Tal como se ve en la siguiente captura de pantalla, puede: 

- Instalar las herramientas de diagnóstico
- Actualícelas (si están obsoletas), 
- Programar ejecuciones diarias de diagnóstico (estos tienen un bajo impacto en el sistema, normalmente toman < 5 minutos en segundo plano y no ocupan más de 500mb en el clúster)
- Vista anteriormente recopila información de diagnóstico si tiene que darle para admitir o analizar usted mismo.

![captura de pantalla de diagnósticos WAC](media/data-collection/Wac.png)

## <a name="get-sddcdiagnosticinfo-output"></a>Salida de Get-SDDCDiagnosticInfo

Estos son los archivos incluidos en la salida de Get-SDDCDiagnosticInfo comprimida.

### <a name="health-summary-report"></a>Informe de resumen de estado

El informe de resumen de estado se guarda como:
- 0_CloudHealthSummary.log

Este archivo se genera después de analizar todos los datos recopilados y está pensado para proporcionar un resumen rápido del sistema. Contiene:

- Información del sistema
- Información general sobre el estado de almacenamiento (número de nodos de seguridad de volúmenes en línea, de clúster compartido los recursos en línea, los componentes en mal estado, etcetera.)
- Obtener más información sobre los componentes en mal estado (recursos de clúster que están sin conexión, con error o pendiente en línea)
- Información de controladores y firmware
- Detalles de volumen, disco físico y grupo
- Rendimiento de almacenamiento (se recopilan los contadores de rendimiento)

Este informe se está actualizando continuamente para incluir la información más útil. Para obtener la información más reciente, consulte el [GitHub README](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/edit/master/README.md).

### <a name="logs-and-xml-files"></a>Los registros y archivos XML

El script de registro varias secuencias de comandos de recopilación de ejecuciones y guarda el resultado como archivos xml. Recopilamos información de diagnóstico de almacenamiento, información del sistema (MSInfo32), sin filtrar los registros de eventos (agrupación en clústeres de conmutación por error, diagnósticos de dis, hyper-v, espacios de almacenamiento etc.) y los registros de clúster y el estado (registros operativos). Para obtener la información más reciente sobre qué información se recopila, consulte el [README GitHub (lo que recopilamos)](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/blob/master/README.md#what-does-the-cmdlet-output-include).

<!--
## Enabling event channels

When Windows Server is installed, many event channels are enabled by default. But sometimes when diagnosing an issue, we want to be able to enable some of these event channels since it will help in triaging and diagnosing system issues.

You could enable additional event channels on each server node in your cluster as needed; however, this approach presents two problems:

1. You need to remember to enable the same event channels on every new server node that you add to your cluster.
2. When diagnosing, it can be tedious to enable specific event channels, reproduce the error, and repeat this process until you root cause.

To avoid these issues, you can enable event channels on cluster startup. The list of enabled event channels on your cluster can be configured using the public property **EnabledEventLogs**. By default, the following event channels are enabled:

```powershell
PS C:\Windows\system32> (get-cluster).EnabledEventLogs
```

Here's an example of the output:
```
Microsoft-Windows-Hyper-V-VmSwitch-Diagnostic,4,0xFFFFFFFD
Microsoft-Windows-SMBDirect/Debug,4
Microsoft-Windows-SMBServer/Analytic
Microsoft-Windows-Kernel-LiveDump/Analytic
```

The **EnabledEventLogs** property is a multistring, where each string is in the form: **channel-name, log-level, keyword-mask**. The **keyword-mask** can be a hexadecimal (prefix 0x), octal (prefix 0), or decimal number (no prefix) number that each event contains (so you can filter by it). For instance, to add a new event channel to the list and to configure both **log-level** and **keyword-mask** you can run:

```powershell
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,2,321"
```

If you want to set the **log-level** but keep the **keyword-mask** at its default value, you can use either of the following commands:

```powershell
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,2"
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,2,"
```

If you want to keep the **log-level** at its default value, but set the **keyword-mask** you can run the following command:

```powershell
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,,0xf1"
```

If you want to keep both the **log-level** and the **keyword-mask** at their default values, you can run any of the following commands:

```powershell
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic"
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,"
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,,"
```

These event channels will be enabled on every cluster node when the cluster service starts or whenever the **EnabledEventLogs** property is changed.
-->

## <a name="how-to-consume-the-xml-files-from-get-pcstoragediagnosticinfo"></a>Cómo consumir los archivos XML de Get-PCStorageDiagnosticInfo
Puede consumir los datos de los archivos XML que se proporcionan en los datos recopilados por el **Get PCStorageDiagnosticInfo** cmdlet. Estos archivos contienen información sobre el virtual discos, discos físicos, información básica del clúster y otro PowerShell relacionados con las salidas. 

Para ver los resultados de estas salidas, abra una ventana de PowerShell y ejecute los siguientes pasos. 

```PowerShell
ipmo storage
$d = import-clixml <filename> 
$d
```

## <a name="what-to-expect-next"></a>¿Qué esperar a continuación?
Muchas mejoras y nuevos cmdlets para analizar SDDC estado del sistema.
Proporcionar comentarios sobre lo que gustaría ver problema [aquí](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/issues). Además, no dude en compartir útiles cambios en la secuencia de comandos, mediante el envío de un [solicitud de extracción](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/pulls).
