---
title: Analizador de diagnósticos de la Ayuda de AD FS
description: Este documento describe el analizador de diagnóstico de Ayuda de AD FS y cómo puede realizar la basic comprueba con el módulo de PowerShell de diagnósticos de AD FS.
author: billmath
ms.author: billmath
manager: mtillman
ms.reviewer: anandyadavMSFT
ms.date: 02/13/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a5b5f895a0575094f8f1af950bde82e1d56325b2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829126"
---
# <a name="ad-fs-help-diagnostics-analyzer"></a>Analizador de diagnósticos de la Ayuda de AD FS

AD FS tiene varias opciones de configuración que admiten la amplia variedad de funcionalidad que proporciona para el desarrollo de aplicaciones y la autenticación. Durante la solución de problemas, se recomienda para asegurarse de que toda la configuración de AD FS están configurado correctamente. Realizar una comprobación manual de estas opciones a veces puede tardar mucho tiempo. Analizador de diagnóstico de Ayuda de AD FS puede ayudar a realizar las comprobaciones básicas con el módulo ADFSToolbox PowerShell. Después de realizar las comprobaciones, se proporciona ayuda de AD FS el [analizador de diagnóstico](https://aka.ms/adfsdiagnosticsanalyzer) para ayudarle a visualizar los resultados con facilidad y ofrecen los pasos de corrección.

La operación completa tarda 3 pasos sencillos:

1. Programa de instalación del módulo ADFSToolbox en el servidor de AD FS principal o el servidor WAP
2. Ejecute los diagnósticos y cargue el archivo en la Ayuda de AD FS
3. Ver análisis de diagnósticos y resuelva cualquier problema

Vaya a [analizador de diagnóstico de Ayuda de AD FS (https://aka.ms/adfsdiagnosticsanalyzer) ](https://aka.ms/adfsdiagnosticsanalyzer) para empezar a solucionar problemas.

![Herramienta de analizador de diagnóstico de AD FS en la Ayuda de AD FS](media/ad-fs-diagonostics-analyzer/home.png)

## <a name="step-1-setup-the-adfstoolbox-module-on-ad-fs-server"></a>Paso 1: Programa de instalación del módulo ADFSToolbox en el servidor de AD FS

Para ejecutar el [analizador de diagnóstico](https://aka.ms/adfsdiagnosticsanalyzer), debe instalar el módulo ADFSToolbox PowerShell. Si el servidor de AD FS tiene conectividad a internet, puede instalar el módulo ADFSToolbox directamente desde la Galería de PowerShell. En el caso sin conectividad a internet, clone el repositorio de GitHub para la instalación manual. 

![Analizador de diagnóstico de AD FS: programa de instalación](media/ad-fs-diagonostics-analyzer/step1.png)

### <a name="setup-using-powershell-gallery"></a>Configuración mediante la Galería de PowerShell

Si el servidor de AD FS tiene conectividad a internet, se recomienda instalar el módulo ADFSToolbox directamente desde la Galería de PowerShell mediante los comandos de PowerShell que se indican a continuación.
 
   ```powershell 
    Install-Module -Name ADFSToolbox -force
    Import-Module ADFSToolbox -force
   ```
### <a name="setup-manually-by-cloning-the-repository"></a>Configurar manualmente mediante la clonación del repositorio

El módulo ADFSToolbox puede instalarse manualmente desde GitHub directamente. Siga las instrucciones siguientes para clonar el repositorio e instalar el módulo ADFSToolbox en el servidor de AD FS.

1. Descargue el [repositorio](https://github.com/Microsoft/adfsToolbox/archive/master.zip)
2. Descomprima el archivo descargado y copie la carpeta principales adfsToolbox a % SYSTEMDRIVE %\\archivos de programa\\WindowsPowerShell\\módulos\\.
3. Importe el módulo de PowerShell. En una ventana de PowerShell con privilegios elevados, ejecute lo siguiente:
 
   ```powershell 
    Import-Module ADFSToolbox -Force
   ```

## <a name="step-2-execute-the-diagnostics-and-upload-the-file-to-ad-fs-help"></a>Paso 2: Ejecute los diagnósticos y cargue el archivo en la Ayuda de AD FS

Un solo comando puede utilizarse para ejecutar fácilmente las pruebas de diagnóstico en todos los servidores de AD FS en la granja de servidores. El módulo de PowerShell usará las sesiones remotas de PS para ejecutar las pruebas de diagnóstico en diferentes servidores de la granja de servidores.

```powershell
    Export-AdfsDiagnosticsFile [-adfsServers <list of servers>]
```

En una granja de AD FS Server 2016, el comando leerá la lista de servidores de AD FS de la configuración de AD FS. Las pruebas de diagnóstico, a continuación, se intenta realizar en cada servidor en la lista. Si la lista de servidores de AD FS no está disponible (ejemplo 2012R2), a continuación, las pruebas se ejecutan en el equipo local. Para especificar una lista de servidores en la que las pruebas se ejecutarán, use el **adfsServers** argumento para proporcionar una lista de servidores. A continuación se proporciona un ejemplo

```powershell
    Export-AdfsDiagnosticsFile -adfsServers @("sts1.contoso.com", "sts2.contoso.com", "sts3.contoso.com")
```

El resultado es un archivo JSON que se crea en el mismo directorio donde se ejecutó el comando. El nombre del archivo es ADFSDiagnosticsFile -\<timestamp\>. Un nombre de archivo de ejemplo es ADFSDiagnosticsFile 20180716124030.

En el paso 2 en [ https://aka.ms/adfsdiagnosticsanalyzer ](https://aka.ms/adfsdiagnosticsanalyzer) utilice el Explorador de archivos para seleccionar el archivo de resultados para cargar.

![Herramienta de analizador de diagnóstico de AD FS: ejecutar y cargar los resultados](media/ad-fs-diagonostics-analyzer/step2.png)

Haga clic en **cargar** para finalizar la carga y continuar con el paso siguiente.

## <a name="step-3-view-diagnostics-analysis-and-resolve-any-issues"></a>Paso 3: Ver análisis de diagnósticos y resuelva cualquier problema

Hay cuatro secciones de los resultados de pruebas:

1. Error: Esta sección contiene la lista de pruebas con error. Cada resultado consta de:
2. Advertencia: Esta sección contiene una lista de pruebas que han dado como resultado una advertencia. Estos problemas no es probable que esto en los problemas en torno a la autenticación en una escala más amplia, pero se deben solucionar lo antes posible.
3. No aplicable: Esta sección contiene la lista de pruebas no ejecutadas debido a que no se aplica para el servidor en particular en el que se estaba ejecutando el comando.
4. Pasar: Esta sección contiene la lista de pruebas que se pasa y no tener ningún elemento de acción del usuario.

Se muestra cada resultado de prueba con los detalles que describen la prueba y la resolución de los pasos:

1. Nombre de la prueba: Nombre de la prueba que se ejecutó
2. Detalles: Descripción de la operación general que se realizó durante la prueba
3. Pasos de resolución: Los pasos sugeridos para resolver el problema resaltado por la prueba

![Herramienta de analizador de diagnóstico de AD FS - lista de resultados de pruebas](media/ad-fs-diagonostics-analyzer/step3a.png)

![Herramienta de analizador de diagnóstico de AD FS - resolución de errores](media/ad-fs-diagonostics-analyzer/step3b.png)

## <a name="next"></a>Siguiente

- [Utilizar las guías de Ayuda de AD FS troublehshooting](https://aka.ms/adfshelp/troubleshooting )
- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)

 
