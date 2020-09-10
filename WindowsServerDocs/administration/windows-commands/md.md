---
title: md
description: Artículo de referencia del comando MD, que crea un directorio o subdirectorio.
ms.topic: reference
ms.assetid: 82162d00-cc34-4776-9e55-4b4836dbd6a9
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1026d14c39b4058ba3e775356d27f3be8d4e8bf9
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633706"
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