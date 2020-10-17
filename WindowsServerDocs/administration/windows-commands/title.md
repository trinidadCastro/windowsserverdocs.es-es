---
title: title
description: Artículo de referencia para el comando title, que crea un título para la ventana del símbolo del sistema.
ms.topic: reference
ms.assetid: c0bbe8bd-201a-4b6c-b617-5d9809881dc8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f4aaaf198a898589699547c522a734e9e3e5aba2
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156759"
---
# <a name="title"></a>title

Crea un título para la ventana de símbolo del sistema.

## <a name="syntax"></a>Sintaxis

```
title [<string>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<string>` | Especifica el texto que se va a mostrar como título de la ventana del símbolo del sistema. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Comentarios

- Para crear el título de una ventana para los programas por lotes, incluya el comando **título** al principio de un programa por lotes.

- Una vez establecido el título de una ventana, solo se puede restablecer mediante el comando **título** .

## <a name="examples"></a>Ejemplos

Para cambiar el título de la ventana del símbolo del sistema a *actualizar archivos* mientras el archivo por lotes ejecuta el comando **Copy** y devolver el título de nuevo al *símbolo del sistema*, escriba el siguiente script:

```
@echo off
title Updating Files
copy \\server\share\*.xls c:\users\common\*.xls
echo Files Updated.
title Command Prompt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
