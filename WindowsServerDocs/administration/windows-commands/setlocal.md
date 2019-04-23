---
title: setlocal
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e4e4b6d3-3f1a-4851-a782-25ee2470e16e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 70e58e3c3a7c3de594c620f7530816b57727d4c3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868866"
---
# <a name="setlocal"></a>setlocal



Inicia la localización de las variables de entorno en un archivo por lotes. La búsqueda continúa hasta que una coincidencia **endlocal** comando se encuentra o se alcanza el final del archivo por lotes.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
setlocal [enableextensions | disableextensions] [enabledelayedexpansion | disabledelayedexpansion]
```

## <a name="arguments"></a>Argumentos

|Argumento|Descripción|
|--------|-----------|
|enableextensions|Habilita las extensiones de comando hasta que la coincidencia de **endlocal** se encuentra el comando, independientemente de la configuración antes de la **setlocal** se ejecutó el comando.|
|DISABLEEXTENSIONS|Deshabilita las extensiones de comando hasta que la coincidencia de **endlocal** se encuentra el comando, independientemente de la configuración antes de la **setlocal** se ejecutó el comando.|
|enabledelayedexpansion|Permite la expansión de variables de entorno retrasada hasta que la coincidencia de **endlocal** se encuentra el comando, independientemente de la configuración antes de la **setlocal** se ejecutó el comando.|
|disabledelayedexpansion|Deshabilita la expansión de variables de entorno retrasada hasta que la coincidencia de **endlocal** se encuentra el comando, independientemente de la configuración antes de la **setlocal** se ejecutó el comando.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Uso de **setlocal**

    Cuando usas **setlocal** fuera de un secuencia de comandos o archivo por lotes, no tiene ningún efecto.
-   Cambiar las variables de entorno

    Use **setlocal** para cambiar las variables de entorno al ejecutar un archivo por lotes. Los cambios de entorno realizados después de ejecutar **setlocal** son locales en el archivo por lotes. El programa Cmd.exe restaura la configuración anterior cuando encuentra un **endlocal** comando o llega al final del archivo por lotes.
-   Comandos de anidamiento

    Puede tener más de un **setlocal** o **endlocal** comando en un programa por lotes (es decir, comandos anidados).
-   Las pruebas para las extensiones de comando en archivos por lotes

    El **setlocal** comando establece la variable ERRORLEVEL. Si pasa {**enableextensions** | **disableextensions**} o {**enabledelayedexpansion**  |   **DISABLEDELAYEDEXPANSION**}, se establece la variable ERRORLEVEL en **0** (cero). En caso contrario, se establece en **1**. Puede usar esta información en scripts por lotes para determinar si las extensiones están disponibles, tal como se muestra en el ejemplo siguiente:  
    ```
    setlocal enableextensions
    verify other 2>nul
    if errorlevel 1 echo Unable to enable extensions
    ```  
    Dado que **cmd** no establece la variable ERRORLEVEL cuando se deshabilitan las extensiones de comando, el **comprobar** comando inicializa la variable ERRORLEVEL en un valor distinto de cero cuando se usa con un no válido argumento. Además, si usa el **setlocal** comando con argumentos {**enableextensions** | **disableextensions**} o {**enabledelayedexpansion**   |  **disabledelayedexpansion**} y no establece la variable ERRORLEVEL en **1**, las extensiones de comando no están disponibles.

## <a name="BKMK_examples"></a>Ejemplos

Puede localizar las variables de entorno en un archivo por lotes, como se muestra en el siguiente script de ejemplo:
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

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)