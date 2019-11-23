---
title: x:.
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e10126739ef82b060e271e9b804a77658b5ec82
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392266"
---
# <a name="unexpose"></a>x:.



Anula la exposición de una instantánea que se expuso mediante el comando de **exposición** . La instantánea expuesta puede especificarse por su ID. de sombra, letra de unidad, recurso compartido o punto de montaje.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
unexpose {<ShadowID> | <Drive:> | <Share> | <MountPoint>}
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<ShadowID >|Anula la exposición de la instantánea especificada por el ID. de sombra dado.|
|Unidad de \<: >|Anula la exposición de la instantánea asociada a la letra de unidad especificada (por ejemplo, la unidad P).|
|\<compartir >|Anula la exposición de la instantánea asociada al recurso compartido especificado (por ejemplo, \\\\*MachineName*\).|
|\<MountPoint >|Anula la exposición de la instantánea asociada con el punto de montaje especificado (por ejemplo, C:\shadowcopy\).|

## <a name="remarks"></a>Observaciones

-   Puede usar un alias existente o una variable de entorno en lugar de *ShadowID*. Use **Agregar** sin parámetros para ver los alias existentes.

## <a name="BKMK_examples"></a>Example

Para anular la exposición de la instantánea asociada a la unidad P, escriba:
```
unexpose P:
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)