---
title: volumen detallado
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 38f2bc75-2ed6-4e80-aa74-ab83133db1cd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8cd5889bfd2aea835cb64ef1a4076faee0f39b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378472"
---
# <a name="detail-volume"></a>volumen detallado



Muestra los discos en los que reside el volumen actual.

## <a name="syntax"></a>Sintaxis

```
detail volume
```

## <a name="remarks"></a>Comentarios

-   Se debe seleccionar un volumen para que esta operación se realice correctamente. Use el comando **seleccionar volumen** para seleccionar un volumen y cambiar el foco a él.
-   Los detalles del volumen no se aplican a los volúmenes de solo lectura, como una unidad de CD-ROM o DVD-ROM.

## <a name="BKMK_examples"></a>Example

Para ver todos los discos en los que reside el volumen actual, escriba:
```
detail volume
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

