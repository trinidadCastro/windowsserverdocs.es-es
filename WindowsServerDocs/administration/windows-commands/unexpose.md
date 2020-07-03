---
title: unexpose
description: Artículo de referencia para unexpote, que anula la exposición de una instantánea que se expuso mediante el comando expoSE.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 02edb1f2c9331a22473123f0327dbc84cb05a865
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85937307"
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

## <a name="remarks"></a>Comentarios

-   Puede usar un alias existente o una variable de entorno en lugar de *ShadowID*. Use **Agregar** sin parámetros para ver los alias existentes.

## <a name="examples"></a>Ejemplos

Para anular la exposición de la instantánea asociada a la unidad P, escriba:
```
unexpose P:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)