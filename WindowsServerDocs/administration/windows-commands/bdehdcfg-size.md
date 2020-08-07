---
title: bdehdcfg size
description: Artículo de referencia para el comando bdehdcfg size, que especifica el tamaño de la partición del sistema cuando se crea una nueva unidad del sistema.
ms.topic: article
ms.assetid: 80f55b1d-a28d-4edf-9997-1fb918b7b5a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc511f72e721561ce27e20b55ceda2e10bb0fdf4
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895076"
---
# <a name="bdehdcfg-size"></a>bdehdcfg: tamaño

Especifica el tamaño de la partición del sistema cuando se crea una nueva unidad del sistema. Si no especifica un tamaño, la herramienta utilizará el valor predeterminado de 300 MB. El tamaño mínimo de la unidad del sistema es 100 MB. Si va a almacenar las herramientas de recuperación del sistema u otras herramientas del sistema en la partición del sistema, deberá aumentar el tamaño consiguientemente.

> [!NOTE]
> El comando **size** no se puede combinar con el `target <drive_letter> merge` comando.

## <a name="syntax"></a>Sintaxis

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink} -size <size_in_mb>
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<size_in_mb>` | Indica el número de megabytes (MB) que se va a utilizar para la nueva partición. |

## <a name="examples"></a>Ejemplos

Para asignar 500 MB a la unidad del sistema predeterminada:

```
bdehdcfg -target default -size 500
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
