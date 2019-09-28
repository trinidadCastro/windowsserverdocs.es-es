---
title: Recopilación de datos de diagnóstico con Espacios de almacenamiento directo
description: Descripción de Espacios de almacenamiento directo herramientas de recopilación de datos, con ejemplos específicos de cómo ejecutarlas y usarlas.
keywords: Espacios de almacenamiento, recopilación de datos, solución de problemas, canales de eventos, Get-SDDCDiagnosticInfo
ms.assetid: ''
ms.prod: windows-server
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 10/24/2018
ms.localizationpriority: ''
ms.openlocfilehash: 67f35e3afa8e9eafabe7b22eb60cc85c7be6cb23
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402874"
---
# <a name="collect-diagnostic-data-with-storage-spaces-direct"></a>Recopilación de datos de diagnóstico con Espacios de almacenamiento directo

> Se aplica a: Windows Server 2019 y Windows Server 2016

Hay varias herramientas de diagnóstico que se pueden usar para recopilar los datos necesarios para solucionar problemas de Espacios de almacenamiento directo y el clúster de conmutación por error. En este artículo, nos centraremos en **Get-SDDCDiagnosticInfo** : una herramienta táctil que recopilará toda la información relevante para ayudarle a diagnosticar el clúster.

Dado que los registros y otra información que **Get-SDDCDiagnosticInfo** son densas, la información sobre la solución de problemas que se presenta a continuación será útil para solucionar problemas avanzados que se han escalado y que pueden requerir que se envíen datos a Microsoft para la clasificación.

## <a name="installing-get-sddcdiagnosticinfo"></a>Instalación de Get-SDDCDiagnosticInfo

El cmdlet **Get-SDDCDiagnosticInfo** de PowerShell (también conocido como **Get-PCStorageDiagnosticInfo**, conocido anteriormente como **Test-StorageHealth**) se puede usar para recopilar registros y realizar comprobaciones de estado de los clústeres de conmutación por error (clúster, recursos, redes, nodos), espacios de almacenamiento (discos físicos, alojamientos, etc.). Discos virtuales), volúmenes compartidos de clúster, recursos compartidos de archivos SMB y desduplicación. 

Hay dos métodos para instalar el script, los cuales se describen a continuación.

### <a name="powershell-gallery"></a>Galería de PowerShell

El [Galería de PowerShell](https://www.powershellgallery.com/packages/PrivateCloud.DiagnosticInfo) es una instantánea del repositorio de github. Tenga en cuenta que la instalación de elementos desde el Galería de PowerShell requiere la versión más reciente del módulo PowerShellGet, que está disponible en Windows 10, en Windows Management Framework (WMF) 5,0 o en el instalador basado en MSI (para PowerShell 3 y 4).

Puede instalar el módulo mediante la ejecución del siguiente comando en PowerShell con privilegios de administrador:

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

El [repositorio de github](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/) es la versión más actualizada del módulo, ya que continuamente se realiza una iteración. Para instalar el módulo desde GitHub, descargue el módulo más reciente del [archivo](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/archive/master.zip) y extraiga el directorio PrivateCloud. DiagnosticInfo a la ruta correcta de los módulos de PowerShell que apunta ```$env:PSModulePath```.

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

Si necesita obtener este módulo en un clúster sin conexión, descargue el archivo zip, muévalo al nodo del servidor de destino e instale el módulo.

## <a name="gathering-logs"></a>Recopilación de registros

Una vez que haya habilitado los canales de eventos y completado el proceso de instalación, puede usar el cmdlet Get-SDDCDiagnosticInfo de PowerShell en el módulo para obtener:

- Informes sobre el estado del almacenamiento, además de detalles sobre componentes incorrectos
- Informes de la capacidad de almacenamiento por grupo, volumen y volumen desduplicado
- Registros de eventos de todos los nodos de clúster y un informe de error de Resumen

Supongamos que el clúster de almacenamiento tiene el nombre *"CLUS01".*

Para ejecutar en un clúster de almacenamiento remoto:

``` PowerShell
Get-SDDCDiagnosticInfo -ClusterName CLUS01
```

Para ejecutar localmente en el nodo de almacenamiento en clúster:

``` PowerShell
Get-SDDCDiagnosticInfo
```

Para guardar los resultados en una carpeta especificada:

``` PowerShell
Get-SDDCDiagnosticInfo -WriteToPath D:\Folder 
```

Este es un ejemplo de cómo se ve en un clúster real:

``` PowerShell
New-Item -Name SDDCDiagTemp -Path d:\ -ItemType Directory -Force
Get-SddcDiagnosticInfo -ClusterName S2D-Cluster -WriteToPath d:\SDDCDiagTemp
```

Como puede ver, el script también realizará la validación del estado actual del clúster.

![captura de pantalla de PowerShell de recopilación de datos](media/data-collection/CollectData.png)

Como puede ver, todos los datos se escriben en la carpeta SDDCDiagTemp

![captura de pantalla de datos en el explorador de archivos](media/data-collection/CollectDataFolder.png)

Una vez finalizado el script, se creará un archivo ZIP en el directorio de los usuarios.

![captura de pantalla de datos adzip de PowerShell](media/data-collection/CollectDataResult.png)

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

Como referencia, este es un vínculo al [Informe de ejemplo](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/SDDCReport.txt) y el [código postal de ejemplo](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/HealthTest-S2D-Cluster-20180522-1546.ZIP).

Para verlo en el centro de administración de Windows (versión 1812 en adelante), vaya a la pestaña *diagnósticos* . Tal como se muestra en la siguiente captura de pantalla, puede 

- Instalar herramientas de diagnóstico
- Actualizarlos (si no están actualizados), 
- Programar ejecuciones de diagnóstico diarias (esto tiene un impacto bajo en el sistema, normalmente tardan < 5 minutos en segundo plano y no ocuparán más de 500 MB en el clúster)
- Vea la información de diagnóstico recopilada previamente si necesita proporcionarla para admitirla o analizarla usted mismo.

![captura de pantalla de diagnósticos de WAC](media/data-collection/Wac.png)

## <a name="get-sddcdiagnosticinfo-output"></a>Salida de Get-SDDCDiagnosticInfo

A continuación se muestran los archivos que se incluyen en la salida comprimida de Get-SDDCDiagnosticInfo.

### <a name="health-summary-report"></a>Informe de Resumen de estado

El informe de Resumen de estado se guarda como:
- 0_CloudHealthSummary. log

Este archivo se genera después de analizar todos los datos recopilados y está pensado para proporcionar un breve resumen del sistema. Contiene:

- Información del sistema
- Información general sobre el estado del almacenamiento (número de nodos actualizados, recursos en línea, volúmenes compartidos de clúster en línea, componentes incorrectos, etc.)
- Detalles sobre los componentes incorrectos (recursos de clúster que están sin conexión, con error o pendientes en línea)
- Información del firmware y del controlador
- Detalles del grupo, el disco físico y el volumen
- Rendimiento de almacenamiento (se recopilan los contadores de rendimiento)

Este informe se está actualizando continuamente para incluir información más útil. Para obtener la información más reciente, consulte el [archivo Léame de github](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/edit/master/README.md).

### <a name="logs-and-xml-files"></a>Registros y archivos XML

El script ejecuta varios scripts de recopilación de registros y guarda el resultado como archivos XML. Recopilamos registros de clústeres y de mantenimiento, información del sistema (MSInfo32), registros de eventos sin filtrar (clústeres de conmutación por error, diagnósticos dis, Hyper-v, espacios de almacenamiento, etc.) y información de diagnóstico de almacenamiento (registros operativos). Para obtener la información más reciente sobre la información que se recopila, consulte el [archivo Léame de github (qué recopilamos)](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/blob/master/README.md#what-does-the-cmdlet-output-include).

## <a name="how-to-consume-the-xml-files-from-get-pcstoragediagnosticinfo"></a>Cómo usar los archivos XML de Get-PCStorageDiagnosticInfo
Puede usar los datos de los archivos XML proporcionados en los datos recopilados por el cmdlet **Get-PCStorageDiagnosticInfo** . Estos archivos tienen información acerca de los discos virtuales, los discos físicos, la información básica del clúster y otras salidas relacionadas con PowerShell. 

Para ver los resultados de estas salidas, abra una ventana de PowerShell y ejecute los pasos siguientes. 

```PowerShell
ipmo storage
$d = import-clixml <filename> 
$d
```

## <a name="what-to-expect-next"></a>¿Qué esperar a continuación?
Muchas mejoras y nuevos cmdlets para analizar el estado del sistema de SDDC.
Proporcione comentarios sobre lo que le gustaría ver mediante el envío de problemas [aquí](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/issues). Además, no dude en contribuir a realizar cambios útiles en el script mediante el envío de una [solicitud de incorporación](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/pulls)de cambios.
