---
title: mask
description: Artículo de referencia del comando Mask, que quita las instantáneas de hardware que se importaron mediante el comando IMPORT.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2839fce0a64f187c1445a5f6a4af6c5f0ebc9fb8
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85922103"
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

#### <a name="remarks"></a>Comentarios

- Puede usar un alias existente o una variable de entorno en lugar de *ShadowSetID*. Use **Agregar** sin parámetros para ver los alias existentes.

### <a name="examples"></a>Ejemplos

Para quitar la instantánea importada *% Import_1%*, escriba:

```
mask %Import_1%
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)