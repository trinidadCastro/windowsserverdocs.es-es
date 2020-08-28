---
title: escritor
description: Artículo de referencia del escritor, que comprueba que un escritor o componente está incluido o excluye un escritor o componente del procedimiento de copia de seguridad o restauración.
ms.topic: reference
ms.assetid: 7cf98295-411d-4705-8573-f898ff45c140
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: db18b7f03bed4fc43da2ebee71c2e2a536d5e1d5
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038083"
---
# <a name="writer"></a>escritor



Comprueba que un escritor o componente está incluido o excluye un escritor o componente del procedimiento de copia de seguridad o restauración. Si se usa sin parámetros, el **escritor** muestra la ayuda en el símbolo del sistema.

## <a name="syntax"></a>Sintaxis

```
writer verify [<Writer> | <Component>]
writer exclude [<Writer> | <Component>]
```

### <a name="parameters"></a>Parámetros

| Parámetro  |                                                                                      Descripción                                                                                      |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   Comprobación   | Comprueba que el escritor o el componente especificado está incluido en el procedimiento de copia de seguridad o restauración. Se producirá un error en el procedimiento de copia de seguridad o restauración si no se incluye el escritor o el componente. |
|  exclude   |                                                   Excluye el escritor o componente especificado del procedimiento de copia de seguridad o restauración.                                                    |
| [\<Writer> |                                                                                     <Component>]                                                                                      |

## <a name="examples"></a>Ejemplos

Para comprobar un escritor especificando su GUID (en este ejemplo, 4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f), escriba:
```
writer verify {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
```
Para excluir un escritor con el nombre escritor del sistema, escriba:
```
writer exclude System Writer
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)