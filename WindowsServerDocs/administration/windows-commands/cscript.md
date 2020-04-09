---
title: cscript
description: Comando comandos de Windows para cscript, que inicia un script para que se ejecute en un entorno de línea de comandos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fba3cbca-594e-4663-bb22-4ee0f63a1ac6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1fc82e1203f81ed966beb8e3906ce95493265195
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846778"
---
# <a name="cscript"></a>cscript

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Inicia un script para que se ejecute en un entorno de línea de comandos.

## <a name="syntax"></a>Sintaxis
```
cscript <Scriptname.extension> [/B] [/D] [/E:<Engine>] [{/H:cscript|/H:wscript}] [/I] [/Job:<Identifier>] [{/Logo|/NoLogo}] [/S] [/T:<Seconds>] [/X] [/U] [/?] [<ScriptArguments>]
```
#### <a name="parameters"></a>Parámetros

|      Parámetro       |                                                                      Descripción                                                                       |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
| ScriptName. Extension |                                 Especifica la ruta de acceso y el nombre del archivo de script con la extensión de nombre de archivo opcional.                                 |
|          /B          |                                Especifica el modo por lotes, que no muestra las alertas, los errores de scripting ni los mensajes de entrada.                                |
|          /D          |                                                                  Inicia el depurador.                                                                  |
|     /E:<Engine>      |                                                  Especifica el motor que se usa para ejecutar el script.                                                  |
|      /H: cscript      |                                         Registra Cscript. exe como host de script predeterminado para ejecutar scripts.                                          |
|      /H: Wscript      |                               Registra Wscript. exe como host de script predeterminado para ejecutar scripts. Este es el valor predeterminado.                               |
|          /I          |        Especifica el modo interactivo, que muestra las alertas, los errores de scripting y los mensajes de entrada. Este es el valor predeterminado y el contrario de **/b**.         |
|  /Trabajo:<Identifier>   |                                             Ejecuta el trabajo identificado por el *identificador* en un archivo de script. wsf.                                             |
|        /Logo         | Especifica que el banner de Windows Script Host se muestra en la consola antes de que se ejecute el script. Este es el valor predeterminado y el contrario de **/nologo**. |
|       /Nologo        |                                 Especifica que el titular de Windows Script Host no se muestra antes de que se ejecute el script.                                 |
|          /S          |                                             Guarda las opciones actuales del símbolo del sistema para el usuario actual.                                             |
|     /T:<Seconds>     |            Especifica el tiempo máximo que se puede ejecutar el script (en segundos). Puede especificar hasta 32.767 segundos. El valor predeterminado es sin límite de tiempo.             |
|          /U          |                                      Especifica Unicode para la entrada y salida que se redirige desde la consola.                                       |
|          /X          |                                                           Inicia el script en el depurador.                                                           |
|          /?          |  Muestra los parámetros de comando disponibles y proporciona ayuda para usarlos. Esto es lo mismo que escribir **Cscript. exe** sin ningún parámetro y sin ningún script.  |
|   ScriptArguments    |                        Especifica los argumentos que se pasan al script. Cada argumento de script debe ir precedido de una barra diagonal ( **/** ).                         |

### <a name="remarks"></a>Comentarios
-   Para realizar esta tarea no necesita credenciales administrativas. Por lo tanto, para garantizar la mayor seguridad, se recomienda realizar esta tarea como usuario sin credenciales administrativas.
-   Para abrir un símbolo del sistema, en la pantalla **Inicio** , escriba **cmd**y, a continuación, haga clic en **símbolo del sistema**.
-   Cada parámetro es opcional; sin embargo, no se pueden especificar argumentos de script sin especificar un script. Si no especifica ningún argumento de script o script, Cscript. exe mostrará la sintaxis de Cscript. exe y las opciones de host válidas.
-   El parámetro **/t** evita la ejecución excesiva de scripts mediante la configuración de un temporizador. Cuando el tiempo de ejecución supera el valor especificado, cscript interrumpe el motor de scripts y finaliza el proceso.
-   Los archivos de script de Windows suelen tener una de las siguientes extensiones de nombre de archivo:. wsf,. vbs,. js.
-   Puede establecer propiedades para scripts individuales. Para obtener más información, consulte los Temas relacionados.
-   Windows Script Host puede usar archivos de script. wsf. Cada archivo. wsf puede usar varios motores de scripting y realizar varios trabajos.
-   Si hace doble clic en un archivo de script con una extensión que no tiene ninguna asociación, aparece el cuadro de diálogo **abrir con** . Seleccione Wscript o CSCRIPT y, a continuación, seleccione **usar siempre este programa para abrir este tipo de archivo**. Esto registra Wscript. exe o CScript como host de script predeterminado para los archivos de este tipo de archivo.
-   Puede establecer propiedades para scripts individuales. Consulte [referencias adicionales](#BKMK_references) para obtener más información.
-   Windows Script Host puede usar archivos de script. wsf. Cada archivo. wsf puede usar varios motores de scripting y realizar varios trabajos.

#### <a name="additional-references"></a><a name=BKMK_references></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
