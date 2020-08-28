---
title: bdehdcfg newdriveletter
description: Artículo de referencia para el comando bdehdcfg newdriveletter, que asigna una nueva letra de unidad a la parte de una unidad utilizada como unidad del sistema.
ms.topic: reference
ms.assetid: f1f200a0-6850-4f0d-9047-f9f982a590f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cf3cf52bfd23db5aadd82170de2bf20c8e602573
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89031553"
---
# <a name="bdehdcfg-newdriveletter"></a>bdehdcfg: newdriveletter

Asigna una nueva letra de unidad a la parte de una unidad utilizada como unidad del sistema. Como práctica recomendada, se recomienda no asignar una letra de unidad a la unidad del sistema.

## <a name="syntax"></a>Sintaxis

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink|<drive_letter> merge} -newdriveletter <drive_letter>
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ---------| ----------- |
| `<drive_letter>` | Define la letra de unidad que se asignará a la unidad de destino especificada. |

## <a name="examples"></a>Ejemplos

Para asignar la unidad predeterminada a la letra de unidad `P` :

```
bdehdcfg -target default -newdriveletter P:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
