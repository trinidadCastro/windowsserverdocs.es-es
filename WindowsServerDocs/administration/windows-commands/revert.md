---
title: revertir
description: Artículo de referencia para el comando Revert, que revierte un volumen a una instantánea especificada.
ms.topic: reference
ms.assetid: 75ad40e4-502a-401e-b11e-8b31e00424b5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3c909cec1e503552f68cad55489529585a5eaf51
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640630"
---
# <a name="revert"></a>revertir

Revierte un volumen a una instantánea especificada. Esto solo se admite para las instantáneas en el contexto CLIENTACCESSIBLE. Estas instantáneas son persistentes y solo las puede realizar el proveedor del sistema. Si se usa sin parámetros, **Revert** muestra la ayuda en el símbolo del sistema.

## <a name="syntax"></a>Sintaxis

```
revert <shadowcopyID>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<shadowcopyID>` | Especifica el identificador de la instantánea a la que se revierte el volumen. Si no usa este parámetro, el comando muestra la ayuda en el símbolo del sistema. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
