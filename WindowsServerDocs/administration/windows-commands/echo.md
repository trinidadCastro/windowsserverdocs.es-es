---
title: echo
description: Tema de referencia para el comando echo, que muestra mensajes o activa o desactiva la característica de repetición de comandos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fb9fcd0f-5e73-4504-aa95-78204e1a79d3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca2a10a5d52c9d175d453a164f3ab4f47ca0841d
ms.sourcegitcommit: 430c6564c18f89eecb5bbc39cfee1a6f1d8ff85b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/26/2020
ms.locfileid: "83855669"
---
# <a name="echo"></a>echo

Muestra mensajes o activa o desactiva la característica de repetición de comandos. Si se usa sin parámetros, **echo** muestra el valor de eco actual.

## <a name="syntax"></a>Sintaxis

```
echo [<message>]
echo [on | off]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| [on \| OFF] | Activa o desactiva la característica de repetición de comandos. El eco de comandos está activado de forma predeterminada. |
| `<message>` | Especifica el texto que se va a mostrar en la pantalla. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Notas

- El `echo <message>` comando es especialmente útil cuando se desactiva el **eco** . Para mostrar un mensaje de varias líneas sin mostrar ningún comando, puede incluir varios `echo <message>` comandos después del comando **echo off** en el programa por lotes.

- Una vez desactivado el **eco** , el símbolo del sistema no aparece en la ventana del símbolo del sistema. Para mostrar el símbolo del sistema, escriba **echo.**

- Si se usa en un archivo por lotes, la opción **eco** y **eco** no afecta a la configuración en el símbolo del sistema.

- Para evitar que se repita un comando determinado en un archivo por lotes, inserte un `@` Inicio de sesión delante del comando. Para evitar que se repitan todos los comandos en un archivo por lotes, incluya el comando **echo off** al principio del archivo.

- Para mostrar una canalización ( `|` ) o un carácter de redireccionamiento ( `<` o `>` ) cuando use el **eco**, use un símbolo de intercalación ( `^` ) inmediatamente antes del carácter de canalización o redirección. Por ejemplo, `^|` , `^>` o `^<` ). Para mostrar un símbolo de intercalación, escriba dos intercalaciones consecutivas ( `^^` ).

### <a name="examples"></a>Ejemplos

Para mostrar la configuración de **eco** actual, escriba:

```
echo
```

Para repetir una línea en blanco en la pantalla, escriba:

```
echo.
```

> [!NOTE]
> No incluya un espacio antes del período. De lo contrario, el punto aparece en lugar de una línea en blanco.

Para evitar que se repitan los comandos en el símbolo del sistema, escriba:

```
echo off
```

> [!NOTE]
> Cuando se desactiva el **eco** , el símbolo del sistema no aparece en la ventana del símbolo del sistema. Para volver a mostrar el símbolo del sistema, escriba **echo**.

Para evitar que todos los comandos de un archivo por lotes (incluido el comando **echo off** ) se muestren en la pantalla, en la primera línea del tipo de archivo por lotes:

```
@echo off
```

Puede usar el comando **echo** como parte de una instrucción **If** . Por ejemplo, para buscar en el directorio actual cualquier archivo con la extensión de nombre de archivo. RPT y para que se muestre un mensaje si se encuentra un archivo de este tipo, escriba:

```
if exist *.rpt echo The report has arrived.
```

El siguiente archivo por lotes busca en el directorio actual los archivos con la extensión de nombre de archivo. txt y muestra un mensaje que indica los resultados de la búsqueda:

```
@echo off
if not exist *.txt (
echo This directory contains no text files.
) else (
   echo This directory contains the following text files:
   echo.
   dir /b *.txt
   )
```

Si no se encuentran archivos. txt al ejecutar el archivo por lotes, se muestra el siguiente mensaje:

```
This directory contains no text files.
```

Si se encuentran archivos. txt cuando se ejecuta el archivo por lotes, se muestran los resultados siguientes (en este ejemplo, se supone que existen los archivos archivo1. txt, archivo2. txt y Archivo3. txt):

```
This directory contains the following text files:
File1.txt
File2.txt
File3.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
