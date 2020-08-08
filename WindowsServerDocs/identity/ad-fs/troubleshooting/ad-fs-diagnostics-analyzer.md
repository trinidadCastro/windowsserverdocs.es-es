---
title: Analizador de diagnósticos de la Ayuda de AD FS
description: En este documento se describe AD FS analizador de diagnóstico de ayuda y cómo puede realizar las comprobaciones básicas con AD FS módulo de PowerShell de diagnóstico.
author: billmath
ms.author: billmath
manager: mtillman
ms.reviewer: anandyadavMSFT
ms.date: 03/29/2019
ms.topic: article
ms.openlocfilehash: e6d6063d3d01cda48a662160c9ad661169371677
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87956202"
---
# <a name="ad-fs-help-diagnostics-analyzer"></a>Analizador de diagnósticos de la Ayuda de AD FS

AD FS tiene numerosas configuraciones que admiten la amplia variedad de funcionalidades que proporciona para la autenticación y el desarrollo de aplicaciones. Durante la solución de problemas, se recomienda asegurarse de que todos los valores de AD FS estén configurados correctamente. En ocasiones, realizar una comprobación manual de esta configuración puede llevar mucho tiempo. AD FS Help Diagnostics Analyzer puede ayudarle a realizar las comprobaciones básicas con el módulo de PowerShell ADFSToolbox. Después de realizar las comprobaciones, AD FS ayuda proporciona el [analizador de diagnósticos](https://aka.ms/adfsdiagnosticsanalyzer) para ayudarle a visualizar fácilmente los resultados y ofrecer pasos de corrección.

La operación completa realiza 3 pasos sencillos:

1. Configuración del módulo ADFSToolbox en el servidor principal de AD FS o servidor WAP
2. Ejecutar el diagnóstico y cargar el archivo en AD FS ayuda
3. Ver el análisis de diagnósticos y resolver los problemas

Vaya a [AD FS Help Diagnostics Analyzer https://aka.ms/adfsdiagnosticsanalyzer) (](https://aka.ms/adfsdiagnosticsanalyzer) para empezar a solucionar problemas).

![AD FS herramienta analizador de diagnósticos en AD FS ayuda](media/ad-fs-diagonostics-analyzer/home.png)

## <a name="step-1-setup-the-adfstoolbox-module-on-ad-fs-server"></a>Paso 1: configurar el módulo ADFSToolbox en AD FS Server

Para ejecutar el [analizador de diagnósticos](https://aka.ms/adfsdiagnosticsanalyzer), debe instalar el módulo de PowerShell ADFSToolbox. Si el servidor de AD FS tiene conectividad a Internet, puede instalar el módulo ADFSToolbox directamente desde la galería de PowerShell. En el caso de que no haya conectividad a Internet, puede instalarla manualmente.

[!WARNING]
Si usa AD FS 2,1 o una versión anterior, debe instalar la versión 1.0.13 de ADFSToolbox. ADFSToolbox ya no admite AD FS 2,1 o inferior en las versiones más recientes.

![Analizador de diagnósticos de AD FS: instalación](media/ad-fs-diagonostics-analyzer/step1_v2.png)

### <a name="setup-using-powershell-gallery"></a>Instalación mediante la galería de PowerShell

Si el servidor de AD FS tiene conectividad a Internet, se recomienda instalar el módulo ADFSToolbox directamente desde la galería de PowerShell mediante los comandos de PowerShell que se indican a continuación.

   ```powershell
    Install-Module -Name ADFSToolbox -force
    Import-Module ADFSToolbox -force
   ```

### <a name="setup-manually"></a>Configuración manual

El módulo ADFSToolbox se debe copiar manualmente en los servidores AD FS o WAP. Siga las instrucciones que se describen a continuación.

1. Inicie una ventana de PowerShell con privilegios elevados en un equipo que tenga acceso a Internet.
2. Instale el módulo del cuadro de herramientas AD FS.

    ```powershell
    Install-Module -Name ADFSToolbox -Force
    ```
3. Copie la carpeta ADFSToolbox `%SYSTEMDRIVE%\Program Files\WindowsPowerShell\Modules\` que se encuentra en la máquina local en la misma ubicación de la máquina AD FS o WAP.

4. Inicie una ventana de PowerShell con privilegios elevados en la máquina AD FS y ejecute el siguiente cmdlet para importar el módulo.

    ```powershell
    Import-Module ADFSToolbox -Force
    ```

## <a name="step-2-execute-the-diagnostics-cmdlet"></a>Paso 2: ejecutar el cmdlet de diagnóstico

![Herramienta analizador de diagnóstico de AD FS: ejecución y carga de resultados](media/ad-fs-diagonostics-analyzer/step2_v2.png)

Se puede usar un solo comando para ejecutar las pruebas de diagnóstico de forma cómoda en todos los servidores de AD FS de la granja. El módulo de PowerShell usará las sesiones remotas de PS para ejecutar las pruebas de diagnóstico en diferentes servidores de la granja.

```powershell
    Export-AdfsDiagnosticsFile [-ServerNames <list of servers>]
```

En un servidor 2016 o superior AD FS granja de servidores, el comando leerá la lista de servidores de AD FS de AD FS configuración. Las pruebas de diagnóstico se intentan en cada servidor de la lista. Si la lista de servidores de AD FS no está disponible (ejemplo 2012R2), las pruebas se ejecutan en el equipo local. Para especificar una lista de servidores en los que se van a ejecutar las pruebas, utilice el argumento **servernames** para proporcionar una lista de servidores. A continuación se proporciona un ejemplo

```powershell
    Export-AdfsDiagnosticsFile -ServerNames @("adfs1.contoso.com", "adfs2.contoso.com")
```

El resultado es un archivo JSON que se crea en el mismo directorio donde se ejecutó el comando. El nombre del archivo es AdfsDiagnosticsFile- \<timestamp\> . AdfsDiagnosticsFile-07312019-184201.jsun nombre de archivo de ejemplo.

## <a name="step-3-upload-the-diagnostics-file"></a>Paso 3: cargar el archivo de diagnóstico

En el paso 3, en [https://aka.ms/adfsdiagnosticsanalyzer](https://aka.ms/adfsdiagnosticsanalyzer) use el explorador de archivos para seleccionar el archivo de resultados que se va a cargar.

Haga clic en **cargar** para finalizar la carga.

Al iniciar sesión con un cuenta de Microsoft, los resultados de diagnóstico se pueden guardar para un punto de vista posterior y se pueden enviar al soporte técnico de Microsoft. Si en algún momento abre un caso de soporte técnico, Microsoft podrá ver los resultados del analizador de diagnóstico y ayudar a solucionar el problema con mayor rapidez.

![Herramienta analizador de diagnóstico de AD FS: Inicio de sesión](media/ad-fs-diagonostics-analyzer/sign_in_step.png)

## <a name="step-4-view-diagnostics-analysis-and-resolve-any-issues"></a>Paso 4: ver el análisis de diagnósticos y resolver los problemas

Existen cinco secciones de resultados de pruebas:

1. Error: esta sección contiene una lista de pruebas con errores. Cada resultado consta de:
2. ADVERTENCIA: esta sección contiene una lista de pruebas que han provocado una advertencia. Estos problemas no tendrán más probabilidades de que se produzcan problemas en la autenticación en una escala más amplia, pero se deben abordar en primer lugar.
3. Passed: esta sección contiene la lista de pruebas que se han superado y no tienen ningún elemento de acción para el usuario.
4. No ejecutado: esta sección contiene la lista de pruebas que no se pudieron ejecutar debido a la falta de información.
5. No aplicable: esta sección contiene la lista de pruebas que no se ejecutaron porque no se aplicaron para el servidor determinado en el que se estaba ejecutando el comando.

![Herramienta analizador de diagnósticos de AD FS: lista de resultados de pruebas ](media/ad-fs-diagonostics-analyzer/step3a_v3.png) se muestra cada resultado de la prueba con detalles que describen la prueba y la resolución de los pasos:

1. Nombre de la prueba: nombre de la prueba que se ejecutó.
2. Descripción: una descripción de la prueba.
3. Detalles: Descripción de la operación general realizada durante la prueba.
4. Pasos de resolución: pasos sugeridos para resolver el problema resaltado por la prueba

![Herramienta analizador de diagnóstico de AD FS: resolución de errores](media/ad-fs-diagonostics-analyzer/step3b_v3.png)

## <a name="next"></a>Siguientes

- [Usar AD FS guías de troublehshooting de ayuda](https://aka.ms/adfshelp/troubleshooting )
- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)
