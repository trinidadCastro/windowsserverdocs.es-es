---
title: bitsadmin getdescription
description: 'Temas de comandos de Windows para **bitsadmin getDescription** : recupera la descripción del trabajo especificado.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f3974603-ebbe-4d31-8217-040fe2d90c85
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 02ab91ad9b6d1d6d1ef67465bb5c982fbddc1bb4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381649"
---
# <a name="bitsadmin-getdescription"></a>bitsadmin getdescription



Recupera la descripción del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetDescription <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se recupera la descripción del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetDescription myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)