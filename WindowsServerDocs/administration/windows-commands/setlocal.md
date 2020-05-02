---
title: setlocal
description: Tema de referencia para setlocal, que inicia la localización de variables de entorno en un archivo por lotes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e4e4b6d3-3f1a-4851-a782-25ee2470e16e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: def4e8c58399af498e3c097f53e526fbedf9de0d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721860"
---
# <a name="setlocal"></a>setlocal

Inicia la localización de variables de entorno en un archivo por lotes. La localización continúa hasta que se encuentra un comando **endlocal** coincidente o hasta que se alcanza el final del archivo por lotes.



## <a name="syntax"></a>Sintaxis

```
setlocal [enableextensions | disableextensions] [enabledelayedexpansion | disabledelayedexpansion]
```

## <a name="arguments"></a>Argumentos

|Argumento|Descripción|
|--------|-----------|
|ENABLEEXTENSIONS|Habilita las extensiones de comando hasta que se encuentre el comando **endlocal** coincidente, independientemente de la configuración anterior a la ejecución del comando **setlocal** .|
|disableextensions|Deshabilita las extensiones de comando hasta que se encuentre el comando **endlocal** coincidente, independientemente de la configuración anterior a la ejecución del comando **setlocal** .|
|enabledelayedexpansion|Habilita la expansión de variables de entorno diferida hasta que se encuentre el comando **endlocal** coincidente, independientemente de la configuración anterior a la ejecución del comando **setlocal** .|
|disabledelayedexpansion|Deshabilita la expansión de la variable de entorno diferida hasta que se encuentre el comando **endlocal** coincidente, independientemente de la configuración anterior a la ejecución del comando **setlocal** .|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   Usar **setlocal**

    Cuando se usa **setlocal** fuera de un script o un archivo por lotes, no tiene ningún efecto.
-   Cambio de variables de entorno

    Use **setlocal** para cambiar las variables de entorno cuando ejecute un archivo por lotes. Los cambios de entorno realizados después de ejecutar **setlocal** son locales para el archivo por lotes. El programa cmd. exe restaura la configuración anterior cuando encuentra un comando **endlocal** o alcanza el final del archivo por lotes.
-   Anidar comandos

    Puede tener más de un comando **setlocal** o **endlocal** en un programa por lotes (es decir, comandos anidados).
-   Probar las extensiones de comandos en archivos por lotes

    El comando **setlocal** establece la variable errorlevel. Si pasa {**ENABLEEXTENSIONS** | **DISABLEEXTENSIONS**} o {**enabledelayedexpansion** | **disabledelayedexpansion**}, la variable ERRORLEVEL se establece en **0** (cero). De lo contrario, se establece en **1**. Puede usar esta información en scripts por lotes para determinar si las extensiones están disponibles, tal y como se muestra en el ejemplo siguiente:  
    ```
    setlocal enableextensions
    verify other 2>nul
    if errorlevel 1 echo Unable to enable extensions
    ```  
    Dado que **cmd** no establece la variable ERRORLEVEL cuando se deshabilitan las extensiones de comando, el comando **Verify** inicializa la variable ERRORLEVEL en un valor distinto de cero cuando se usa con un argumento no válido. Además, si usa el comando **setlocal** con argumentos {**ENABLEEXTENSIONS** | **DISABLEEXTENSIONS**} o {**enabledelayedexpansion** | **disabledelayedexpansion**} y no establece la variable ERRORLEVEL en **1**, las extensiones de comando no estarán disponibles.

## <a name="examples"></a>Ejemplos

Puede localizar las variables de entorno en un archivo por lotes, tal y como se muestra en el siguiente script de ejemplo:
```
rem *******Begin Comment**************
rem This program starts the superapp batch program on the network,
rem directs the output to a file, and displays the file
rem in Notepad.
rem *******End Comment**************
@echo off
setlocal
path=g:\programs\superapp;%path%
call superapp>c:\superapp.out
endlocal
start notepad c:\superapp.out
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)