---
title: bitsadmin getcompletiontime
description: 'Temas de comandos de Windows para **bitsadmin getcompletiontime** : recupera la hora en que el trabajo finalizó la transferencia de datos.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7a4b3c1c-9832-4724-86b2-cce3c01bfa28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 190467d5f3a7b7244ed0d7ab3b75d4cbbf56c8d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381737"
---
# <a name="bitsadmin-getcompletiontime"></a>bitsadmin getcompletiontime



Recupera la hora en que el trabajo finalizó la transferencia de datos.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetCompletionTime <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se recupera la hora en que el trabajo denominado *myDownloadJob* ha terminado de transferir los datos.
```
C:\>bitsadmin /GetCompletionTime myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)