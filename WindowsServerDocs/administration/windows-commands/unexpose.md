---
title: Anule la exposición
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: fe9cb5dfd8ae6c71fdc72ddc1e8421229f98f5d0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837476"
---
# <a name="unexpose"></a>Anule la exposición



Unexposes una instantánea que se expone mediante el **exponer** comando. La copia sombra expuestos se puede especificar por su Id. de la sombra, letra de unidad, recurso compartido o punto de montaje.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
unexpose {<ShadowID> | <Drive:> | <Share> | <MountPoint>}
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<ShadowID>|Unexposes la instantánea especificada por el identificador de instantánea especificado.|
|\<Drive:>|Unexposes la instantánea asociada con la letra de unidad especificada (por ejemplo, la unidad P).|
|\<Share>|Unexposes la instantánea asociada con el recurso compartido especificado (por ejemplo, \\ \\ *MachineName*\).|
|\<MountPoint>|Unexposes la instantánea asociada al punto de montaje especificado (por ejemplo, C:\shadowcopy\).|

## <a name="remarks"></a>Comentarios

-   Puede usar un alias existente o una variable de entorno en lugar de *IdDeInstantánea*. Use **agregar** sin parámetros para ver los alias existentes.

## <a name="BKMK_examples"></a>Ejemplos

Para que se anule la exposición de la instantánea asociada con la unidad P, escriba:
```
unexpose P:
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)