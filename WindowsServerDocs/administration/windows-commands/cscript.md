---
title: cscript
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fba3cbca-594e-4663-bb22-4ee0f63a1ac6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ffdbd6f67e4e4c32022134191deabd861bf248b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827676"
---
# <a name="cscript"></a>cscript

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

inicia una secuencia de comandos para que se ejecute en un entorno de línea de comandos.
## <a name="syntax"></a>Sintaxis
```
cscript <Scriptname.extension> [/B] [/D] [/E:<Engine>] [{/H:cscript|/H:wscript}] [/I] [/Job:<Identifier>] [{/Logo|/NoLogo}] [/S] [/T:<Seconds>] [/X] [/U] [/?] [<ScriptArguments>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|Scriptname.extension|Especifica la ruta de acceso y el nombre del archivo de script con la extensión de nombre de archivo opcional.|
|/B|Especifica el modo por lotes, que no muestra alertas, errores de scripting o solicita información al usuario.|
|/D|Inicia al depurador.|
|/E:<Engine>|Especifica el motor que se usa para ejecutar el script.|
|/H:cscript|Registra cscript.exe como host predeterminado para ejecutar scripts.|
|/H:wscript|Registra wscript.exe como host predeterminado para ejecutar scripts. Este es el valor predeterminado.|
|/I|Especifica el modo interactivo, que muestra las alertas, los errores de secuencias de comandos y solicita información al usuario. Este es el valor predeterminado y lo opuesto a **/B**.|
|/ Trabajo:<Identifier>|Se ejecuta el trabajo identificado por *identificador* en un archivo de script .wsf.|
|O logotipo|Especifica que la pancarta de Windows Script Host se muestra en la consola antes de que se ejecuta la secuencia de comandos. Este es el valor predeterminado y lo opuesto a **/Nologo**.|
|/Nologo|Especifica que no se muestra la pancarta de Windows Script Host antes de que se ejecuta la secuencia de comandos.|
|/S|Guarda las opciones de línea de comandos actuales para el usuario actual.|
|/T:<Seconds>|Especifica el tiempo máximo que se puede ejecutar la secuencia de comandos (en segundos). Puede especificar hasta 32.767 segundos. El valor predeterminado no es límite de tiempo.|
|/U|Especifica Unicode para la entrada y salida que se redirige desde la consola.|
|/X|Inicia la secuencia de comandos en el depurador.|
|/?|Muestra los parámetros de los comandos disponibles y proporciona ayuda para su uso. Esto es lo mismo que escribir **cscript.exe** sin parámetros ni ningún script.|
|ScriptArguments|Especifica los argumentos pasados al script. Cada argumento de la secuencia de comandos debe ir precedido por una barra diagonal (**/**).|
### <a name="remarks"></a>Comentarios
-   Para llevar a cabo esta tarea no es necesario contar con credenciales administrativas. Por lo tanto, para garantizar la mayor seguridad, se recomienda realizar esta tarea como usuario sin credenciales administrativas.
-   Para abrir un símbolo del sistema, en el **iniciar** , escriba **cmd**y, a continuación, haga clic en **símbolo**.
-   Cada parámetro es opcional; Sin embargo, no se puede especificar argumentos de script sin especificar una secuencia de comandos. Si no especifica una secuencia de comandos o los argumentos de la secuencia de comandos, cscript.exe muestra la sintaxis de cscript.exe y las opciones de host válido.
-   El **/T** parámetro impide la ejecución excesiva de secuencias de comandos estableciendo un temporizador. Cuando el tiempo de ejecución supera el valor especificado, cscript interrumpe el motor de scripts y finaliza el proceso.
-   Archivos de script de Windows suelen tengan una de las siguientes extensiones de nombre de archivo: .wsf, .vbs y .js.
-   Puede establecer propiedades de secuencias de comandos. Para obtener más información, vea Temas relacionados.
-   Windows Script Host puede utilizar archivos de script .wsf. Cada archivo .wsf puede utilizar varios motores de secuencias de comandos y realizar varias tareas.
-   Si hace doble clic en un archivo de script con una extensión que no tiene ninguna asociación, el **abrir con** aparece el cuadro de diálogo. Seleccione wscript o cscript y, a continuación, seleccione **utilizar siempre este programa para abrir este tipo de archivo**. Esto registra wscript.exe o cscript como host de script predeterminado para los archivos de este tipo de archivo.
-   Puede establecer propiedades de secuencias de comandos. Consulte [referencias adicionales](#BKMK_references) para obtener más información.
-   Windows Script Host puede utilizar archivos de script .wsf. Cada archivo .wsf puede utilizar varios motores de secuencias de comandos y realizar varias tareas.

#### <a name="BKMK_references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
