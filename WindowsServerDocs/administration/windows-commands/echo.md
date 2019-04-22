---
title: echo
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fb9fcd0f-5e73-4504-aa95-78204e1a79d3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: afb4b10a0d347367780dbaf19b764f2cabd480d9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819546"
---
# <a name="echo"></a>echo



Muestra mensajes o activa o desactiva la característica de presentación de comandos. Si se utiliza sin parámetros, **echo** muestra la configuración actual.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
echo [<Message>]
echo [on | off]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[en \| off]|Activa o desactiva la característica de presentación de comandos. Repetición de comandos está de manera predeterminada.|
|\<mensaje >|Especifica el texto que se muestra en la pantalla.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   El **echo** *mensaje* comando es especialmente útil cuando **echo** está desactivado. Para mostrar un mensaje de varias líneas sin mostrar todos los comandos, puede incluir varios **echo** *mensaje* comandos después de la **echo desactivar** comando su programa por lotes.
-   Cuando **echo** está desactivada, la línea de comandos no aparece en la ventana de símbolo del sistema. Para mostrar el símbolo del sistema, escriba **eco.**
-   Si se utiliza en un archivo por lotes, **eco** y **echo desactivar** no afectan a la configuración en el símbolo del sistema.
-   Para evitar repetir un comando determinado en un archivo por lotes, inserte un signo de arroba (@) delante del comando. Para evitar repetir todos los comandos en un archivo por lotes, incluya el **echo desactivar** comando al principio del archivo.
-   Para mostrar una barra vertical (**|**) o de redirección (**<** o **>**) cuando se utiliza **eco**, utilice un símbolo de intercalación (^) inmediatamente antes del carácter de canalización o de redirección (por ejemplo, **^|**, **^>**, o **^<**). Para mostrar un símbolo de intercalación, escriba dos símbolos de intercalación en sucesión (**^^**).

## <a name="BKMK_examples"></a>Ejemplos

Para mostrar actual **echo** , escriba:
```
echo
```
Para devolver una línea en blanco en la pantalla, escriba:
```
echo.
```

> [!NOTE]
> No incluya un espacio antes del período. En caso contrario, se mostrará el período en lugar de una línea en blanco.

Para evitar repetir comandos en el símbolo del sistema, escriba:
```
echo off 
```

> [!NOTE]
> Cuando **echo** está desactivada, la línea de comandos no aparece en la ventana de símbolo del sistema. Para volver a mostrar el símbolo del sistema, escriba **eco**.

Para evitar que todos los comandos en un archivo por lotes (incluido el **echo desactivar** comando) muestre en la pantalla, en la primera línea del tipo de archivo por lotes:
```
@echo off
```
Puede usar el **echo** comandos como parte de un **si** instrucción. Por ejemplo, para buscar el directorio actual para cualquier archivo con la extensión de nombre de archivo .rpt y echo un mensaje si se encuentra este archivo, escriba:
```
if exist *.rpt echo The report has arrived.
```
El siguiente archivo por lotes busca en el directorio actual de archivos con la extensión de nombre de archivo .txt y muestra un mensaje que indica los resultados de la búsqueda:
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
Si no hay archivos .txt se encuentran cuando se ejecuta el archivo por lotes, aparece el mensaje siguiente:
```
This directory contains no text files.
```
Si los archivos .txt se encuentran cuando se ejecute el archivo por lotes muestra la siguiente salida (en este ejemplo, suponga que los archivos File1.txt y File2.txt, File3.txt existe):
```
This directory contains the following text files:
File1.txt
File2.txt
File3.txt
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
