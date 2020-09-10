---
title: mask
description: Artículo de referencia del comando Mask, que quita las instantáneas de hardware que se importaron mediante el comando IMPORT.
ms.topic: reference
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ee0e4207a7c5cf6ad81ece39e9134881ad3c0239
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633711"
---
# <a name="mask"></a>mask

Quita las instantáneas de hardware que se importaron mediante el comando de **importación** .

## <a name="syntax"></a>Sintaxis

```
mask <shadowsetID>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| shadowsetID | Quita las instantáneas que pertenecen al identificador de conjunto de instantáneas especificado. |

#### <a name="remarks"></a>Observaciones

- Puede usar un alias existente o una variable de entorno en lugar de *ShadowSetID*. Use **Agregar** sin parámetros para ver los alias existentes.

### <a name="examples"></a>Ejemplos

Para quitar la instantánea importada *% Import_1%*, escriba:

```
mask %Import_1%
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)