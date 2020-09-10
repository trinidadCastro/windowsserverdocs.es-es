---
title: bdehdcfg target
description: Artículo de referencia para el comando bdehdcfg Target, que prepara una partición para su uso como unidad del sistema mediante BitLocker y la recuperación de Windows.
ms.topic: reference
ms.assetid: f761d25d-8349-4ac7-ac46-6bb340a4348f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7cc13e3c224aa1af944c5e9c9550737addcb6980
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632812"
---
# <a name="bdehdcfg-target"></a>bdehdcfg: destino

Prepara una partición para su uso como unidad del sistema mediante BitLocker y la recuperación de Windows. De forma predeterminada, esta partición se crea sin una letra de unidad.

## <a name="syntax"></a>Sintaxis

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink|<drive_letter> merge}
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| default | Indica que la herramienta de línea de comandos seguirá el mismo proceso que el asistente para la instalación de BitLocker. |
| unallocated | Crea la partición del sistema a partir del espacio sin asignar disponible en el disco. |
| `<drive_letter>` Prime | Reduce la unidad especificada en la cantidad necesaria para crear una partición activa del sistema. Para utilizar este comando, la unidad especificada debe tener al menos el 5 por ciento de espacio disponible. |
| `<drive_letter>` sin | Utiliza la unidad de disco especificada como partición activa del sistema. La unidad del sistema operativo no puede ser un destino para la combinación. |

## <a name="examples"></a>Ejemplos

Para designar una unidad existente (P) como unidad del sistema:

```
bdehdcfg -target P: merge
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
