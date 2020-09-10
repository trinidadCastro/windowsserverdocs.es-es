---
title: bitsadmin getstate
description: Artículo de referencia para el comando bitsadmin GetState, que recupera el estado del trabajo especificado.
ms.topic: reference
ms.assetid: 1252d6cf-14ca-44df-beb2-930ff011f297
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2faf05bd245f0b59eb9f10d0d46d96e8e1d3e11c
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631653"
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

#### <a name="output"></a>Resultados

Los valores de salida devueltos pueden ser:

| State | Descripción |
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
