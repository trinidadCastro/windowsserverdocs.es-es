---
title: x:.
description: El tema comandos de Windows para unexpote, que anula la exposición de una instantánea que se expuso mediante el comando expoSE.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2f8bbdb3b810ffbf9332608a016fc3b3e188e9f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832358"
---
# <a name="unexpose"></a>x:.

Anula la exposición de una instantánea que se expuso mediante el comando de **exposición** . La instantánea expuesta puede especificarse por su ID. de sombra, letra de unidad, recurso compartido o punto de montaje.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
unexpose {<ShadowID> | <Drive:> | <Share> | <MountPoint>}
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<ShadowID >|Anula la exposición de la instantánea especificada por el ID. de sombra dado.|
|Unidad de \<: >|Anula la exposición de la instantánea asociada a la letra de unidad especificada (por ejemplo, la unidad P).|
|\<compartir >|Anula la exposición de la instantánea asociada al recurso compartido especificado (por ejemplo, \\\\*MachineName*\).|
|\<MountPoint >|Anula la exposición de la instantánea asociada con el punto de montaje especificado (por ejemplo, C:\shadowcopy\).|

## <a name="remarks"></a>Comentarios

-   Puede usar un alias existente o una variable de entorno en lugar de *ShadowID*. Use **Agregar** sin parámetros para ver los alias existentes.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para anular la exposición de la instantánea asociada a la unidad P, escriba:
```
unexpose P:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)