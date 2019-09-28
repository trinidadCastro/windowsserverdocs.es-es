---
title: bitsadmin nowrap
description: 'El tema comandos de Windows para **bitsadmin NoWrap** : trunca cualquier línea de texto de salida que se extienda más allá del borde derecho de la ventana de comandos.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 85a47b90-783a-41e4-96f2-81f26ae8ca93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3806ec51161eeae498e3c9b367b2aacf0bd32c99
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381054"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

Trunca cualquier línea de texto de salida que se extienda más allá del borde derecho de la ventana de comandos.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /NoWrap
```

## <a name="remarks"></a>Comentarios

De forma predeterminada, todos los modificadores, excepto el modificador **monitor** , encapsulan el resultado. Especifique el modificador **noWrap** antes de otros modificadores.

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se recupera el estado del trabajo denominado *myDownloadJob* y no se encapsula el resultado.
```
C:\>bitsadmin /NoWrap /GetState myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)