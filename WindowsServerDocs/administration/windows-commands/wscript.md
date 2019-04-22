---
title: wscript
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fbaf193-cdbd-414c-84c9-bb5720f84c29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 771c1231ee5379ec797f535505839de8671e32a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821306"
---
# <a name="wscript"></a>wscript



Windows Script Host proporciona un entorno en el que los usuarios pueden ejecutar scripts en una variedad de idiomas que utilizan una gran variedad de modelos de objetos para realizar tareas.

## <a name="syntax"></a>Sintaxis

```
wscript [<scriptname>] [/b] [/d] [/e:<engine>] [{/h:cscript|/h:wscript}] [/i] [/job:<identifier>] [{/logo|/nologo}] [/s] [/t:<number>] [/x] [/?] [<ScriptArguments>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|scriptname|Especifica la ruta de acceso y el nombre del archivo de script.|
|/b|Especifica el modo por lotes, que no muestra alertas, errores de scripting o solicita información al usuario. Esto es lo opuesto de **/i**.|
|/d|Inicia al depurador.|
|/e|Especifica el motor que se usa para ejecutar el script. Esto le permite ejecutar scripts que usan una extensión de nombre de archivo personalizado. Sin el parámetro /e, solo se pueden ejecutar scripts que usan las extensiones de nombre de archivo registrado. Por ejemplo, si intenta ejecutar este comando:<br>```cscript test.admin```<br>Recibirá este mensaje de error: Error de entrada: No hay ningún motor de secuencia de comandos para la extensión de archivo ". administrador."<br>Una ventaja de usar extensiones de nombre de archivo no estándar es que lo protege contra accidentalmente haga doble clic en una secuencia de comandos y ejecución de algo realmente no deseaba ejecutar. <br>No crea una asociación entre la extensión de nombre de archivo .admin y VBScript permanente. Cada vez que ejecute un script que usa una extensión de nombre de archivo .admin, deberá usar el parámetro /e.|
|/h:cscript|Registra **cscript.exe** como el host de script predeterminado para ejecutar scripts.|
|/h:wscript|Registra **wscript.exe** como el host de script predeterminado para ejecutar scripts. Este es el valor predeterminado cuando la **/h** se omite la opción.|
|/i|Especifica el modo interactivo, que muestra las alertas, los errores de secuencias de comandos y solicita información al usuario.</br>Este es el valor predeterminado y lo opuesto a **/b**.|
|/job:\<identifier>|Se ejecuta el trabajo identificado por *identificador* en un **.wsf** archivo de script.|
|/logo|Especifica que la pancarta de Windows Script Host se muestra en la consola antes de que se ejecuta la secuencia de comandos.</br>Este es el valor predeterminado y lo opuesto a **/nologo**.|
|/nologo|Especifica que no se muestra la pancarta de Windows Script Host antes de que se ejecuta la secuencia de comandos. Esto es lo opuesto de **/logo**.|
|/s|Guarda las opciones de línea de comandos actual para el usuario actual.|
|/t:\<number>|Especifica el tiempo máximo que se puede ejecutar la secuencia de comandos (en segundos). Puede especificar hasta 32.767 segundos.</br>El valor predeterminado no es límite de tiempo.|
|/x|Inicia la secuencia de comandos en el depurador.|
|ScriptArguments|Especifica los argumentos pasados al script. Cada argumento de la secuencia de comandos debe ir precedido por una barra diagonal (/).|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Para llevar a cabo esta tarea no es necesario contar con credenciales administrativas. Por lo tanto, para garantizar la mayor seguridad, se recomienda realizar esta tarea como usuario sin credenciales administrativas.
-   Para abrir un símbolo del sistema, en la pantalla **Inicio**, escriba **cmd** y, a continuación, haga clic en **símbolo del sistema**.
-   Cada parámetro es opcional; Sin embargo, no se puede especificar argumentos de script sin especificar una secuencia de comandos. Si no especifica una secuencia de comandos o los argumentos del script, **wscript.exe** muestra el **configuración de Windows Script Host** cuadro de diálogo que puede usar para establecer las propiedades globales de todos los scripts que **wscript.exe** se ejecuta en el equipo local.
-   El **/t** parámetro impide la ejecución excesiva de secuencias de comandos estableciendo un temporizador. Cuando el tiempo supera el valor especificado, **wscript** interrumpe el motor de scripts y finaliza el proceso.
-   Archivos de script de Windows suelen tengan una de las siguientes extensiones de nombre de archivo: **.wsf**, **.vbs**, **.js**.
-   Si hace doble clic en un archivo de script con una extensión que no tiene ninguna asociación, el **abrir con** aparece el cuadro de diálogo. Seleccione **wscript** o **cscript**y, a continuación, seleccione **utilizar siempre este programa para abrir este tipo de archivo**. Esto registra **wscript.exe** o **cscript.exe** como el host de script predeterminado para los archivos de este tipo de archivo.
-   Puede establecer propiedades de secuencias de comandos. Consulte [Introducción a Windows Script Host](https://technet.microsoft.com/library/cc738350(v=ws.10).aspx) para obtener más información.
-   Puede utilizar Windows Script Host **.wsf** archivos de script. Cada **.wsf** archivo puede usar varios motores de secuencias de comandos y realizar varias tareas.

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
