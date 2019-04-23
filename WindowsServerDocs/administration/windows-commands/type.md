---
title: type
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c44fe905-a865-4c97-8cc5-fb95fec7d4d5
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 4ceb7365d34a2aeca21d1a699730a589f98fd549
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887406"
---
# <a name="type"></a>type


En el shell de comandos de Windows, **tipo** es integradas en el comando que muestra el contenido de un archivo de texto. Use la **tipo** comando para ver un archivo de texto sin modificación alguna.


En PowerShell, **tipo** es un alias integrado para el **[Get-Content](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-content)** que también muestra el contenido de un archivo, pero con una sintaxis diferente.


Para obtener ejemplos de cómo usar este comando en el shell de comandos de Windows (Cmd.exe), consulte [ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
type [<Drive>:][<Path>]<FileName>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<Drive>:][\<Path>]\<FileName>|Especifica la ubicación y el nombre del archivo o archivos que desea ver. Separe varios nombres de archivo con espacios.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Si *FileName* contiene espacios, escríbalo entre comillas (por ejemplo, "archivo de nombre que contiene Spaces.txt").
-   Si se muestra un archivo binario o un archivo creado por un programa, es posible que vea caracteres extraños en la pantalla, incluidos los caracteres de avance de página y los símbolos de la secuencia de escape. Estos caracteres representan los códigos de control que se usan en el archivo binario. En general, evite usar el **tipo** comando para mostrar los archivos binarios.

## <a name="BKMK_examples"></a>Ejemplos

Para mostrar el contenido de un archivo denominado playa.mar, escriba:
```
type holiday.mar 
```
Para mostrar el contenido de un archivo largo denominado playa.mar una pantalla a la vez, escriba:
```
type holiday.mar | more 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
