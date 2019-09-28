---
title: bitsadmin getstate
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1252d6cf-14ca-44df-beb2-930ff011f297
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55be37a6b1b44b81ed9002e5e3b9eb1fd46bd0dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381235"
---
# <a name="bitsadmin-getstate"></a>bitsadmin getstate



Recupera el estado del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetState <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="remarks"></a>Comentarios

Los Estados posibles son:

|-----|-----| | EN cola | El trabajo está en espera de ejecutarse. | | CONECTAndo | BITS está poniéndose en contacto con el servidor. | | TRANSFERIR | BITS está transfiriendo datos. | | SUSPENDIDA | El trabajo está en pausa. | | ERROR | Se produjo un error no recuperable. no se volverá a intentar la transferencia. | | TRANSIENT_ERROR | Se ha producido un error recuperable. la transferencia reintenta cuando expira el retraso de reintento mínimo. | | CONFIRMADO | Se completó el trabajo. | | CANCELAdo | Se canceló el trabajo. |

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se recupera el estado del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetState myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)