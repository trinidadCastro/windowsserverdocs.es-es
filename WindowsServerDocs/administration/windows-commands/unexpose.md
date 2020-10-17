---
title: unexpose
description: Artículo de referencia para el comando unexpote, que anula la exposición de una instantánea expuesta.
ms.topic: reference
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 35e545d632790f8c4d7d69d6e9fea1e7e1a07dc4
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156314"
---
# <a name="unexpose"></a>unexpose

Anula la exposición de una instantánea que se expuso mediante el [comando de exposición](expose.md). La instantánea expuesta puede especificarse por su ID. de sombra, letra de unidad, recurso compartido o punto de montaje.

## <a name="syntax"></a>Sintaxis

```
unexpose {<shadowID> | <drive:> | <share> | <mountpoint>}
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<shadowID>` | Muestra la instantánea especificada por el ID. de sombra dado. Puede usar un alias existente o una variable de entorno en lugar de `<shadowID>` . Use el [comando agregar](add.md) sin parámetros para ver todos los alias existentes. |
| `<drive:>` | Muestra la instantánea asociada a la letra de unidad especificada (por ejemplo, la unidad P). |
| `<share>` | Muestra la instantánea asociada al recurso compartido especificado (por ejemplo, `\\MachineName` ). |
| `<mountpoint>` | Muestra la instantánea asociada con el punto de montaje especificado (por ejemplo, `C:\shadowcopy\` ). |
| add | Si se usa sin parámetros, se mostrarán los alias existentes. |

## <a name="examples"></a>Ejemplos

Para anular la exposición de la instantánea asociada a * unidad P: \* , escriba:

```
unexpose P:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Agregar comando](add.md)

- [exponer (comando)](expose.md)
