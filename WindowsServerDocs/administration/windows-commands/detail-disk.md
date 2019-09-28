---
title: disco de detalles
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ff78a3f9e27cde35a7e19bdf1565c515a127261b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378578"
---
# <a name="detail-disk"></a>disco de detalles



Muestra las propiedades del disco seleccionado y los volúmenes de dicho disco.

## <a name="syntax"></a>Sintaxis

```
detail disk
```

## <a name="remarks"></a>Comentarios

-   Se debe seleccionar un disco para que esta operación se realice correctamente. Use el comando **Seleccionar disco** para seleccionar un disco y desplazar el foco a él.
-   Si el disco seleccionado es un disco duro virtual (VHD), **detail Disk** informa del tipo de bus del disco como virtual.

## <a name="BKMK_examples"></a>Example

Para ver las propiedades del disco seleccionado e información acerca de los volúmenes del disco, escriba:
```
detail disk
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

