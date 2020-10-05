---
title: subst
description: Artículo de referencia para el comando subst, que asocia una ruta de acceso a una letra de unidad.
ms.topic: reference
ms.assetid: 3e69234c-2312-4343-868b-afc1017c622a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 52d45c9139c70c4f513a972f7789f872145c8b63
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718232"
---
# <a name="subst"></a>subst

Asocia una ruta de acceso a una letra de unidad. Si se usa sin parámetros, **subst** muestra los nombres de las unidades virtuales en vigor.

## <a name="syntax"></a>Sintaxis

```
subst [<drive1>: [<drive2>:]<path>]
subst <drive1>: /d
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<drive1>:` | Especifica la unidad virtual a la que desea asignar una ruta de acceso. |
| `[<drive2>:]<path>` | Especifica la unidad física y la ruta de acceso que desea asignar a una unidad virtual. |
| /d | Elimina una unidad (virtual) sustituida. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="remarks"></a>Observaciones

- Los siguientes comandos no funcionan y no deben usarse en las unidades especificadas en el comando **subst** :

  - [CHKDSK (comando)](chkdsk.md)

    [comando diskcomp](diskcomp.md)

    [diskcopy (comando)](diskcopy.md)

    [Format (comando)](format.md)

    [etiqueta (comando)](label.md)

    [comando Recover](recover.md)

- El `<drive1>` parámetro debe estar en el intervalo especificado por el comando **LASTDRIVE** . Si no es así, **subst** muestra el siguiente mensaje de error: `Invalid parameter - drive1:`

## <a name="examples"></a>Ejemplos

Para crear una unidad virtual z para la ruta de acceso b:\user\betty\forms, escriba:

```
subst z: b:\user\betty\forms
```

En lugar de escribir la ruta de acceso completa, puede llegar a este directorio escribiendo la letra de la unidad virtual seguida de dos puntos, como se indica a continuación:

```
z:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)