---
title: bitsadmin list
description: 'Temas de comandos de Windows para la **lista de bitsadmin** : enumera los trabajos de transferencia que son propiedad del usuario actual.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1416965e-e0e6-49cf-b1d4-b286d3cf8716
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd4787f51dc2a7843ff6cf5c4f786658e530ad8f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381100"
---
# <a name="bitsadmin-list"></a>bitsadmin list



Enumera los trabajos de transferencia que son propiedad del usuario actual.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /List [/allusers][/verbose]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/Allusers|Opcional: enumera los trabajos de todos los usuarios|
|/Verbose|Opcional: proporciona información detallada para cada trabajo.|

## <a name="remarks"></a>Comentarios

Debe tener privilegios de administrador para usar el parámetro/AllUsers

## <a name="BKMK_examples"></a>Example

En el siguiente ejemplo se recupera información sobre los trabajos que pertenecen al usuario actual.
```
C:\>bitsadmin /List 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)