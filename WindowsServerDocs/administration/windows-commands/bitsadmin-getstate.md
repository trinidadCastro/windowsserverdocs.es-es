---
title: bitsadmin getstate
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f7ed7529fda264efaceb6b4b36e36e728c211f3f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889626"
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
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="remarks"></a>Comentarios

Los Estados posibles son:

|-----|-----| | EN COLA | El trabajo está esperando para ejecutarse. | | CONECTANDO | BITS es ponerse en contacto con el servidor. | | TRANSFERIR | BITS está transfiriendo datos. | | SUSPENDE | El trabajo está en pausa. | | ERROR | Se produjo un error no recuperable; no se volverá a la transferencia. | | TRANSIENT_ERROR | Se produjo un error recuperable; vuelve a intentar la transferencia cuando expira el intervalo entre reintentos mínimo. | | CONFIRMADO | El trabajo se completó. | | CANCELADO | Se canceló el trabajo. |

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se recupera el estado del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetState myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)