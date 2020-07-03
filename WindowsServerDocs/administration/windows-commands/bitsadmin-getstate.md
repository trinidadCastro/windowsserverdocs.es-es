---
title: bitsadmin getstate
description: Artículo de referencia para el comando bitsadmin GetState, que recupera el estado del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1252d6cf-14ca-44df-beb2-930ff011f297
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd698727cba25f15a12a331f847e7f8436d3d54e
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926671"
---
# <a name="bitsadmin-getstate"></a>bitsadmin getstate

Recupera el estado del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getstate <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

#### <a name="output"></a>Output

Los valores de salida devueltos pueden ser:

| Estado | Descripción |
| --------------- | ----------- |
| En cola | El trabajo está en espera de ejecutarse. |
| Connecting | BITS está poniéndose en contacto con el servidor. |
| Transferring | BITS está transfiriendo datos. |
| Traslada | BITS transfirió correctamente todos los archivos del trabajo. |
| Suspended | El trabajo está en pausa. |
| Error | Se produjo un error no recuperable. no se volverá a intentar la transferencia. |
| Transient_Error | Se ha producido un error recuperable. la transferencia se reintenta cuando expira el retraso de reintento mínimo. |
| Confirmado | Se completó el trabajo. |
| Canceled | El trabajo se canceló. |

## <a name="examples"></a>Ejemplos

Para recuperar el estado del trabajo denominado *myDownloadJob*:

```
bitsadmin /getstate myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
