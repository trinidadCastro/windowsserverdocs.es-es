---
title: tipo
description: Temas de comandos de Windows para el tipo, que muestra el contenido de un archivo de texto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c44fe905-a865-4c97-8cc5-fb95fec7d4d5
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 3163601d118df315edcae540917313703f677d52
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832418"
---
# <a name="type"></a>tipo

En el shell de comandos de Windows, **Escriba** es un comando integrado que muestra el contenido de un archivo de texto. Use el comando **Type** para ver un archivo de texto sin modificarlo.

En PowerShell, el **tipo** es un alias integrado para el cmdlet **[Get-Content](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-content)** , que también muestra el contenido de un archivo, pero con una sintaxis diferente.

Para obtener ejemplos de cómo usar este comando en el shell de comandos de Windows (cmd. exe), vea [ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
type [<Drive>:][<Path>]<FileName>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<> de unidad:] [\<path >]\<FileName >|Especifica la ubicación y el nombre del archivo o los archivos que desea ver. Separe varios nombres de archivo con espacios.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Si *filename* contiene espacios, escríbalo entre comillas (por ejemplo, el nombre de archivo que contiene espacios. txt).
-   Si muestra un archivo binario o un archivo creado por un programa, es posible que vea caracteres extraños en la pantalla, incluidos los caracteres avance y los símbolos de secuencia de escape. Estos caracteres representan códigos de control que se usan en el archivo binario. En general, evite usar el comando **Type** para mostrar archivos binarios.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para mostrar el contenido de un archivo denominado festivo. mar, escriba:
```
type holiday.mar 
```
Para mostrar el contenido de un archivo largo denominado festividad. marzo de una pantalla a la vez, escriba:
```
type holiday.mar | more 
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
