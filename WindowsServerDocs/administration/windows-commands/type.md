---
title: type
description: Artículo de referencia para el tipo, que muestra el contenido de un archivo de texto.
ms.topic: article
ms.assetid: c44fe905-a865-4c97-8cc5-fb95fec7d4d5
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 1cb4e28a079c39aaadd85267c004781cc9c84f3c
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896653"
---
# <a name="type"></a>type

En el shell de comandos de Windows, **Escriba** es un comando integrado que muestra el contenido de un archivo de texto. Use el comando **Type** para ver un archivo de texto sin modificarlo.

En PowerShell, el **tipo** es un alias integrado para el cmdlet **[Get-Content](/powershell/module/microsoft.powershell.management/get-content)** , que también muestra el contenido de un archivo, pero con una sintaxis diferente.

## <a name="syntax"></a>Sintaxis

```
type [<Drive>:][<Path>]<FileName>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<Drive>:][\<Path>]\<FileName>|Especifica la ubicación y el nombre del archivo o los archivos que desea ver. Separe varios nombres de archivo con espacios.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   Si *filename* contiene espacios, escríbalo entre comillas (por ejemplo, el nombre de archivo que contiene Spaces.txt).
-   Si muestra un archivo binario o un archivo creado por un programa, es posible que vea caracteres extraños en la pantalla, incluidos los caracteres avance y los símbolos de secuencia de escape. Estos caracteres representan códigos de control que se usan en el archivo binario. En general, evite usar el comando **Type** para mostrar archivos binarios.

## <a name="examples"></a>Ejemplos

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
