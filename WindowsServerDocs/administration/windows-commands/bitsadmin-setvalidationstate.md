---
title: bitsadmin setvalidationstate
description: 'Windows Commands topic for **bitsadmin setvalidationstate** : establece el estado de validación del contenido del archivo especificado en el trabajo.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 37d7fa3a8a91abf1e7b6ac5a51b6cebd78984a91
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380401"
---
# <a name="bitsadmin-setvalidationstate"></a>bitsadmin setvalidationstate



Establece el estado de validación del contenido del archivo especificado en el trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetValidationState <Job> <file index> <true|false> 
```

## <a name="parameters"></a>Parámetros

| Parámetro  |          Descripción           |
|------------|--------------------------------|
|    Trabajo     | El nombre para mostrar del trabajo o el GUID |
| Índice de archivo |         Comienza en 0          |
|    True    |             False              |

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se establece el estado de validación de contenido del archivo 2 en TRUE para el trabajo denominado *myJob*.
```
C:\>bitsadmin /SetValidationState myJob 2 TRUE 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)