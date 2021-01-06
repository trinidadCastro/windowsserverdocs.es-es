---
title: type
description: Artículo de referencia para el comando Type, que muestra el contenido de un archivo de texto.
ms.topic: reference
ms.assetid: c44fe905-a865-4c97-8cc5-fb95fec7d4d5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2020
ms.openlocfilehash: 911a7bc36f3aa4cc1be9dbbcba58432abed9bb14
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947301"
---
# <a name="type"></a>type

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
