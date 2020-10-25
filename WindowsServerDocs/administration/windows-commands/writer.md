---
title: escritor
description: Artículo de referencia del comando Writer, que comprueba que un escritor o componente está incluido o excluye un escritor o componente del procedimiento de copia de seguridad o restauración.
ms.topic: reference
ms.assetid: 7cf98295-411d-4705-8573-f898ff45c140
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d6af778ca49f8be88a08e04ecfe4165752cc796d
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524870"
---
# <a name="writer"></a>escritor

Comprueba que un escritor o componente está incluido o excluye un escritor o componente del procedimiento de copia de seguridad o restauración. Si se usa sin parámetros, el **escritor** muestra la ayuda en el símbolo del sistema.

## <a name="syntax"></a>Sintaxis

```
writer verify [writer> | <component>]
writer exclude [<writer> | <component>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| Comprobación | Comprueba que el escritor o el componente especificado está incluido en el procedimiento de copia de seguridad o restauración. Se producirá un error en el procedimiento de copia de seguridad o restauración si no se incluye el escritor o el componente. |
| exclude | Excluye el escritor o componente especificado del procedimiento de copia de seguridad o restauración. |

## <a name="examples"></a>Ejemplos

Para comprobar un escritor especificando su GUID (en este ejemplo, 4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f), escriba:

```
writer verify {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
```

Para excluir un escritor con el nombre *escritor del sistema*, escriba:

```
writer exclude System Writer
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
