---
title: bitsadmin getfilestransferred
description: 'Temas de comandos de Windows para **bitsadmin getfilestransferred** : recupera el número de archivos transferidos para el trabajo especificado.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e282815c-938b-4ac0-a09d-9baafb656dcb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d02d9d7bc216a5ad7ca922e716c368f64c4b9a44
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381603"
---
# <a name="bitsadmin-getfilestransferred"></a>bitsadmin getfilestransferred



Recupera el número de archivos transferidos para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetFilesTransferred <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se recupera el número de archivos transferidos en el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetFilesTransferred myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)