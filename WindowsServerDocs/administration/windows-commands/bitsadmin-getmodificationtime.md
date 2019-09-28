---
title: bitsadmin getmodificationtime
description: 'Comando comandos de Windows para **bitsadmin getmodificationtime** : recupera la última vez que se modificó el trabajo o los datos se transfirieron correctamente.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e543945e-92c4-491e-8c2d-344f8a3e342d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 48b4d252ce6161b288726190f41f08c64efdbcf2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381531"
---
# <a name="bitsadmin-getmodificationtime"></a>bitsadmin getmodificationtime



Recupera la última vez que se modificó el trabajo o los datos se transfirieron correctamente.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetModificationTime <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se recupera la hora de la última modificación del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetModificationTime myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)