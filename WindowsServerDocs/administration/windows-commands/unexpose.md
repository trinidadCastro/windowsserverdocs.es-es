---
title: unexpose
description: Artículo de referencia para unexpote, que anula la exposición de una instantánea que se expuso mediante el comando expoSE.
ms.topic: reference
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13e14941e2c67aa0361dcc0af2cdb1a36bf7e651
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036413"
---
# <a name="unexpose"></a>unexpose

Anula la exposición de una instantánea que se expuso mediante el comando de **exposición** . La instantánea expuesta puede especificarse por su ID. de sombra, letra de unidad, recurso compartido o punto de montaje.



## <a name="syntax"></a>Sintaxis

```
unexpose {<ShadowID> | <Drive:> | <Share> | <MountPoint>}
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<ShadowID>|Anula la exposición de la instantánea especificada por el ID. de sombra dado.|
|\<Drive:>|Anula la exposición de la instantánea asociada a la letra de unidad especificada (por ejemplo, la unidad P).|
|\<Share>|Anula la exposición de la instantánea asociada al recurso compartido especificado (por ejemplo, \\ \\ *MachineName* \) .|
|\<MountPoint>|Anula la exposición de la instantánea asociada con el punto de montaje especificado (por ejemplo, C:\shadowcopy \) .|

## <a name="remarks"></a>Observaciones

-   Puede usar un alias existente o una variable de entorno en lugar de *ShadowID*. Use **Agregar** sin parámetros para ver los alias existentes.

## <a name="examples"></a>Ejemplos

Para anular la exposición de la instantánea asociada a la unidad P, escriba:
```
unexpose P:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)