---
title: mkdir
description: Artículo de referencia para el comando mkdir, que crea un directorio o subdirectorio.
ms.topic: reference
ms.assetid: 033a57a2-5deb-4c98-aa78-61ce8df2a330
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: bfefe4bc040d3e3f28adb6b37529764f28fca25b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639508"
---
# <a name="mkdir"></a>mkdir

Crea un directorio o subdirectorio. Las extensiones de comandos, que están habilitadas de forma predeterminada, permiten usar un único comando **mkdir** para crear directorios intermedios en una ruta de acceso especificada.

> [!NOTE]
> Este comando es el mismo que el [comando MD](md.md).

## <a name="syntax"></a>Sintaxis

```
mkdir [<drive>:]<path>
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
mkdir Directory1
```

Para crear el árbol de directorios *Taxes\Property\Current* en el directorio raíz, con las extensiones de comando habilitadas, escriba:

```
mkdir \Taxes\Property\Current
```

Para crear el árbol de directorios *Taxes\Property\Current* en el directorio raíz como en el ejemplo anterior, pero con extensiones de comando deshabilitadas, escriba la siguiente secuencia de comandos:

```
mkdir \Taxes
mkdir \Taxes\Property
mkdir \Taxes\Property\Current
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [MD (comando)](md.md)