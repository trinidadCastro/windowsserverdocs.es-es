---
title: tree
description: Artículo de referencia para el árbol, que muestra la estructura de directorios de una ruta de acceso, o del disco de una unidad, de forma gráfica.
ms.topic: reference
ms.assetid: 345d3192-401e-4a3b-a8ac-36a85c7be79d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c9f586dbcf9072feffe71e1c3d792f10afc951a4
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156407"
---
# <a name="tree"></a>tree

Muestra gráficamente la estructura de directorios de una ruta de acceso o del disco de una unidad. La estructura que muestra este comando depende de los parámetros que se especifiquen en el símbolo del sistema. Si no especifica una unidad o una ruta de acceso, este comando muestra la estructura de árbol que empieza con el directorio actual de la unidad actual.

## <a name="syntax"></a>Sintaxis

```
tree [<drive>:][<path>] [/f] [/a]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<drive>:` | Especifica la unidad que contiene el disco para el que desea mostrar la estructura de directorios. |
| `<path>` | Especifica el directorio para el que desea mostrar la estructura de directorios. |
| /f | Muestra los nombres de los archivos de cada directorio. |
| /a | Especifica el uso de caracteres de texto en lugar de caracteres gráficos para mostrar las líneas que vinculan a los subdirectorios. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para mostrar los nombres de todos los subdirectorios del disco en la unidad actual, escriba:

```
tree \
```

Para mostrar una pantalla a la vez, los archivos de todos los directorios de la unidad C, escriba:

```
tree c:\ /f | more
```

Para imprimir una lista de todos los directorios de la unidad C, escriba:

```
tree c:\ /f  prn
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
