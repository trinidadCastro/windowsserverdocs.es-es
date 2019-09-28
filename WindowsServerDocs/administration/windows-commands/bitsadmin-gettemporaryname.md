---
title: bitsadmin gettemporaryname
description: Windows Commands topic for **bitsadmin gettemporaryname** -informa del nombre de archivo temporal del archivo especificado en el trabajo.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 68925edc-a801-4292-a812-7471c4f60fdd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7b665fae4c0bfdd5ea04b929be49f9590430b358
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381303"
---
# <a name="bitsadmin-gettemporaryname"></a>bitsadmin gettemporaryname



Notifica el nombre de archivo temporal del archivo especificado en el trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetTemporaryName <Job> <file index> 
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|Índice de archivo|Comienza en 0|

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se notifica el nombre de archivo temporal del archivo 2 para el trabajo denominado *myJob*.
```
C:\>bitsadmin /GetTemporaryName myJob 1 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)