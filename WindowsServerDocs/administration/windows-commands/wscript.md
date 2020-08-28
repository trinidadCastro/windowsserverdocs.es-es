---
title: wscript
description: Artículo de referencia para Wscript, que proporciona un entorno en el que los usuarios pueden ejecutar scripts en diversos lenguajes que usan diversos modelos de objetos para realizar tareas.
ms.topic: reference
ms.assetid: 2fbaf193-cdbd-414c-84c9-bb5720f84c29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 689775951f1c97b7cc704801cd39691033005afe
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038073"
---
# <a name="wscript"></a>wscript



Windows Script Host proporciona un entorno en el que los usuarios pueden ejecutar scripts en diversos lenguajes que usan diversos modelos de objetos para realizar tareas.

## <a name="syntax"></a>Sintaxis

```
wscript [<scriptname>] [/b] [/d] [/e:<engine>] [{/h:cscript|/h:wscript}] [/i] [/job:<identifier>] [{/logo|/nologo}] [/s] [/t:<number>] [/x] [/?] [<ScriptArguments>]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|nombreDeScript|Especifica la ruta de acceso y el nombre del archivo de script.|
|/b|Especifica el modo por lotes, que no muestra las alertas, los errores de scripting ni los mensajes de entrada. Es lo contrario de **/i**.|
|/d|Inicia el depurador.|
|/e|Especifica el motor que se usa para ejecutar el script. Esto le permite ejecutar scripts que usan una extensión de nombre de archivo personalizada. Sin el parámetro/e, solo se pueden ejecutar scripts que utilicen las extensiones de nombre de archivo registradas. Por ejemplo, si intenta ejecutar este comando:<br>```cscript test.admin```<br>Recibirá este mensaje de error: error de entrada: no hay ningún motor de scripts para la extensión de archivo. admin.<br>Una ventaja de usar extensiones de nombre de archivo no estándar es que protege contra el hecho de hacer doble clic accidentalmente en un script y ejecutar algo que realmente no deseaba ejecutar. <br>Esto no crea una asociación permanente entre la extensión de nombre de archivo. admin y VBScript. Cada vez que se ejecuta un script que usa una extensión de nombre de archivo. admin, se debe usar el parámetro/e.|
|/h: cscript|Registra **cscript.exe** como el host de script predeterminado para ejecutar scripts.|
|/h: Wscript|Registra **wscript.exe** como el host de script predeterminado para ejecutar scripts. Este es el valor predeterminado cuando se omite la opción **/h** .|
|/i|Especifica el modo interactivo, que muestra las alertas, los errores de scripting y los mensajes de entrada.</br>Este es el valor predeterminado y el contrario de **/b**.|
|/trabajo\<identifier>|Ejecuta el trabajo identificado por el *identificador* en un archivo de script **. wsf** .|
|/logo|Especifica que el banner de Windows Script Host se muestra en la consola antes de que se ejecute el script.</br>Este es el valor predeterminado y el contrario de **/nologo**.|
|/nologo|Especifica que el titular de Windows Script Host no se muestra antes de que se ejecute el script. Es lo contrario de **/logo**.|
|/s|Guarda las opciones actuales del símbolo del sistema para el usuario actual.|
|/t:\<number>|Especifica el tiempo máximo que se puede ejecutar el script (en segundos). Puede especificar hasta 32.767 segundos.</br>El valor predeterminado es sin límite de tiempo.|
|/x|Inicia el script en el depurador.|
|ScriptArguments|Especifica los argumentos que se pasan al script. Cada argumento de script debe ir precedido de una barra diagonal (/).|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   La realización de esta tarea no le exige que tenga credenciales administrativas. Por consiguiente, como medida de seguridad recomendada, considere la posibilidad de realizar esta tarea como un usuario sin credenciales administrativas.
-   Para abrir un símbolo del sistema, en la pantalla **Inicio**, escriba **cmd** y, a continuación, haga clic en **símbolo del sistema**.
-   Cada parámetro es opcional; sin embargo, no se pueden especificar argumentos de script sin especificar un script. Si no especifica ningún argumento de script o script, **wscript.exe** muestra el cuadro de diálogo **configuración de Windows Script Host** , que puede usar para establecer las propiedades globales de los scripts de todos los scripts que **wscript.exe** se ejecutan en el equipo local.
-   El parámetro **/t** evita la ejecución excesiva de scripts mediante la configuración de un temporizador. Cuando el tiempo supera el valor especificado, **Wscript** interrumpe el motor de scripts y finaliza el proceso.
-   Los archivos de script de Windows suelen tener una de las siguientes extensiones de nombre de archivo: **. wsf**, **. vbs**, **. js**.
-   Si hace doble clic en un archivo de script con una extensión que no tiene ninguna asociación, aparece el cuadro de diálogo **abrir con** . Seleccione **Wscript** o **cscript**y, a continuación, seleccione **usar siempre este programa para abrir este tipo de archivo**. Esto registra **wscript.exe** o **cscript.exe** como host de script predeterminado para los archivos de este tipo de archivo.
-   Puede establecer propiedades para scripts individuales. Para obtener más información, consulte [Introducción a Windows Script Host](/previous-versions/windows/it-pro/windows-server-2003/cc738350(v=ws.10)) .
-   Windows Script Host puede usar archivos de script **. wsf** . Cada archivo **. wsf** puede usar varios motores de scripting y realizar varios trabajos.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
