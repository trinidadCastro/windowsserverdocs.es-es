---
title: bdehdcfg newdriveletter
description: 'El tema comandos de Windows para bdehdcfg newdriveletter: asigna una nueva letra de unidad a la parte de una unidad utilizada como unidad del sistema.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f1f200a0-6850-4f0d-9047-f9f982a590f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2abd4a686f358b5dd844514735edb3ffaa13845
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382230"
---
# <a name="bdehdcfg-newdriveletter"></a>bdehdcfg: newdriveletter



Asigna una nueva letra de unidad a la parte de una unidad utilizada como unidad del sistema. Para obtener un ejemplo de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -newdriveletter <DriveLetter>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|@no__t 0DriveLetter >|Define la letra de unidad que se asignará a la unidad de destino especificada.|

## <a name="remarks"></a>Comentarios

Como práctica recomendada, se recomienda no asignar una letra de unidad a la unidad del sistema.

## <a name="BKMK_Examples"></a>Example

En el ejemplo siguiente se muestra la unidad predeterminada a la que se asigna la letra de unidad P.
```
bdehdcfg -target default -newdriveletter P:
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)