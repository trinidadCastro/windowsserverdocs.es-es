---
title: mask
description: Artículo de referencia del comando Mask, que quita las instantáneas de hardware que se importaron mediante el comando IMPORT.
ms.topic: article
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a893d32dca90169d51a04db66b3dc796cbc69a46
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87886522"
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