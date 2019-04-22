---
title: disco de detalle
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6b09cf40-8d93-452b-b449-5242e62a4102
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c7a5063edf3cb2e190e8aec957e1b571c1f15bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819106"
---
# <a name="detail-disk"></a>disco de detalle



Muestra las propiedades del disco seleccionado y los volúmenes de dicho disco.

## <a name="syntax"></a>Sintaxis

```
detail disk
```

## <a name="remarks"></a>Comentarios

-   Debe seleccionarse un disco para que esta operación se realice correctamente. Use la **seleccione disco** comando para seleccionar un disco y cambiar el foco a ella.
-   Si el disco seleccionado es un disco duro virtual (VHD), **disco detalle** notifica el tipo de bus del disco como Virtual.

## <a name="BKMK_examples"></a>Ejemplos

Para ver las propiedades del disco seleccionado y obtener información acerca de los volúmenes en el disco, escriba:
```
detail disk
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

