---
title: tipo
description: Artículo de referencia para el comando Type, que muestra el contenido de un archivo de texto.
ms.topic: reference
ms.assetid: c44fe905-a865-4c97-8cc5-fb95fec7d4d5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.openlocfilehash: 879cd68b4995c65d623d56e97c0680587eda15b4
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156363"
---
# <a name="type"></a>tipo

En el shell de comandos de Windows, **Escriba** es un comando integrado que muestra el contenido de un archivo de texto. Use el comando **Type** para ver un archivo de texto sin modificarlo.

En PowerShell, el **tipo** es un alias integrado para el [cmdlet Get-Content](/powershell/module/microsoft.powershell.management/get-content), que también muestra el contenido de un archivo, pero con una sintaxis diferente.

## <a name="syntax"></a>Sintaxis

```
type [<drive>:][<path>]<filename>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `[<drive>:][<path>]<filename>` | Especifica la ubicación y el nombre del archivo o los archivos que desea ver. Si `<filename>` contiene espacios, debe encerrarlo entre comillas (por ejemplo, "nombre de archivo que contiene Spaces.txt"). También puede agregar varios nombres de archivo agregando espacios entre ellos. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Comentarios

- Si muestra un archivo binario o un archivo creado por un programa, es posible que vea caracteres extraños en la pantalla, incluidos los caracteres avance y los símbolos de secuencia de escape. Estos caracteres representan códigos de control que se usan en el archivo binario. En general, evite usar el comando **Type** para mostrar archivos binarios.

## <a name="examples"></a>Ejemplos

Para mostrar el contenido de un archivo denominado *festivo. mar*, escriba:

```
type holiday.mar
```

Para mostrar el contenido de un archivo largo denominado *festividad. marzo* de una pantalla a la vez, escriba:

```
type holiday.mar | more
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
