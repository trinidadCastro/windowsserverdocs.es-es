---
title: bitsadmin getstate
description: Tema comandos de Windows para bitsadmin GetState
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
ms.openlocfilehash: 2cff790c8787b1514e8523a4583184d6f6a59efc
ms.sourcegitcommit: 51e0b575ef43cd16b2dab2db31c1d416e66eebe8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2020
ms.locfileid: "76259110"
---
# <a name="bitsadmin-getstate"></a>bitsadmin getstate


Recupera el estado del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetState <Job>
```

## <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
|    Trabajo    | El nombre para mostrar del trabajo o el GUID |

## <a name="remarks"></a>Comentarios

Los estados posibles son:

|      Estado      | Descripción |
| --------------- | ----------- |
| EN cola          | El trabajo está en espera de ejecutarse. |
| CONECTANDO      | BITS está poniéndose en contacto con el servidor. |
| TRANSFERIR    | BITS está transfiriendo datos. |
| TRASLADA     | BITS transfirió correctamente todos los archivos del trabajo. |
| SUSPENDIDAS       | El trabajo está en pausa. |
| ERROR           | Se produjo un error no recuperable. no se volverá a intentar la transferencia. |
| TRANSIENT_ERROR | Se ha producido un error recuperable. la transferencia se reintenta cuando expira el retraso de reintento mínimo. |
| CONFIRMADO    | Se completó el trabajo. |
| CANCELADO        | El trabajo se canceló. |

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se recupera el estado del trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /GetState myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
