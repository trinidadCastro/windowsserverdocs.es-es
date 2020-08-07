---
title: md
description: Artículo de referencia del comando MD, que crea un directorio o subdirectorio.
ms.topic: article
ms.assetid: 82162d00-cc34-4776-9e55-4b4836dbd6a9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94dd8f696b97d56fe082f73287a3d50dc7f70883
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87886514"
---
# <a name="md"></a>md

Crea un directorio o subdirectorio. Las extensiones de comandos, que están habilitadas de forma predeterminada, permiten usar un único comando **MD** para crear directorios intermedios en una ruta de acceso especificada.

> [!NOTE]
> Este comando es el mismo que el [comando mkdir](mkdir.md).

## <a name="syntax"></a>Sintaxis

```
md [<drive>:]<path>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<drive>`: | Especifica la unidad en la que desea crear el nuevo directorio. |
| `<path>` | Especifica el nombre y la ubicación del nuevo directorio. El sistema de archivos determina la longitud máxima de cualquier ruta de acceso única. Es un parámetro obligatorio. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para crear un directorio denominado *Directory1* en el directorio actual, escriba:

```
md Directory1
```

Para crear el árbol de directorios *Taxes\Property\Current* en el directorio raíz, con las extensiones de comando habilitadas, escriba:

```
md \Taxes\Property\Current
```

Para crear el árbol de directorios *Taxes\Property\Current* en el directorio raíz como en el ejemplo anterior, pero con extensiones de comando deshabilitadas, escriba la siguiente secuencia de comandos:

```
md \Taxes
md \Taxes\Property
md \Taxes\Property\Current
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [mkdir (comando)](mkdir.md)