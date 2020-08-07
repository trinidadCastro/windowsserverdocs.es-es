---
title: cscript
description: Artículo de referencia para el comando cscript, que inicia un script para que se ejecute en un entorno de línea de comandos.
ms.topic: article
ms.assetid: fba3cbca-594e-4663-bb22-4ee0f63a1ac6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3f6e27cae1531e0c10e8721d7f7fe11487406e35
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87891537"
---
# <a name="cscript"></a>cscript

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Inicia un script para ejecutarlo en un entorno de línea de comandos.

>[!IMPORTANT]
> La realización de esta tarea no le exige que tenga credenciales administrativas. Por consiguiente, como medida de seguridad recomendada, considere la posibilidad de realizar esta tarea como un usuario sin credenciales administrativas.

## <a name="syntax"></a>Sintaxis

```
cscript <scriptname.extension> [/b] [/d] [/e:<engine>] [{/h:cscript | /h:wscript}] [/i] [/job:<identifier>] [{/logo | /nologo}] [/s] [/t:<seconds>] [x] [/u] [/?] [<scriptarguments>]
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| scriptName. Extension | Especifica la ruta de acceso y el nombre del archivo de script con la extensión de nombre de archivo opcional. |
| /b | Especifica el modo por lotes, que no muestra las alertas, los errores de scripting ni los mensajes de entrada. |
| /d | Inicia el depurador. |
| /e:`<engine>` | Especifica el motor que se usa para ejecutar el script. |
| /h: cscript | Registra cscript.exe como el host de script predeterminado para ejecutar scripts. |
| /h: Wscript | Registra wscript.exe como el host de script predeterminado para ejecutar scripts. Este es el valor predeterminado. |
| /i | Especifica el modo interactivo, que muestra las alertas, los errores de scripting y los mensajes de entrada. Este es el valor predeterminado y el contrario de `/b` . |
| /trabajo<identifier> | Ejecuta el trabajo identificado por el *identificador* en un archivo de script. wsf. |
| /logo | Especifica que el banner de Windows Script Host se muestra en la consola antes de que se ejecute el script. Este es el valor predeterminado y el contrario de `/nologo` . |
| /nologo | Especifica que el titular de Windows Script Host no se muestra antes de que se ejecute el script. |
| /s | Guarda las opciones actuales del símbolo del sistema para el usuario actual. |
| /t:<seconds> | Especifica el tiempo máximo que se puede ejecutar el script (en segundos). Puede especificar hasta 32.767 segundos. El valor predeterminado es sin límite de tiempo. |
| /U | Especifica Unicode para la entrada y salida que se redirige desde la consola. |
| /x | Inicia el script en el depurador. |
| /? | Muestra los parámetros de comando disponibles y proporciona ayuda para usarlos. Esto es lo mismo que escribir **cscript.exe** sin ningún parámetro y sin ningún script. |
| scriptarguments | Especifica los argumentos que se pasan al script. Cada argumento de script debe ir precedido de una barra diagonal ( **/** ). |

#### <a name="remarks"></a>Observaciones

- Cada parámetro es opcional; sin embargo, no se pueden especificar argumentos de script sin especificar un script. Si no especifica ningún argumento de script o script, cscript.exe muestra la sintaxis de cscript.exe y las opciones de host válidas.

- El parámetro **/t** evita la ejecución excesiva de scripts mediante la configuración de un temporizador. Cuando el tiempo de ejecución supera el valor especificado, cscript interrumpe el motor de scripts y finaliza el proceso.

- Los archivos de script de Windows suelen tener una de las siguientes extensiones de nombre de archivo:. wsf,. vbs,. js. Windows Script Host puede usar archivos de script. wsf. Cada archivo. wsf puede usar varios motores de scripting y realizar varios trabajos.

- Si hace doble clic en un archivo de script con una extensión que no tiene ninguna asociación, aparece el cuadro de diálogo **abrir con** . Seleccione Wscript o CSCRIPT y, a continuación, seleccione **usar siempre este programa para abrir este tipo de archivo**. Esto registra wscript.exe o CScript como host de script predeterminado para los archivos de este tipo de archivo.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
