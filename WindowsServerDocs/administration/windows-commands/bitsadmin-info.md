---
title: bitsadmin info
description: En el tema comandos de Windows para se **muestra información de resumen sobre el trabajo especificado.** -bitsadmin info
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b6710d73860315fcd13670669871cd310ffb41c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381081"
---
# <a name="bitsadmin-info"></a>bitsadmin info



Muestra información de resumen sobre el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Info <Job> [/verbose]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="remarks"></a>Comentarios

Use el parámetro/verbose para proporcionar información detallada sobre el trabajo.

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se recupera información sobre el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /Info myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)