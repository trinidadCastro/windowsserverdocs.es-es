---
title: bitsadmin getstate
description: Tema de comandos de Windows para **bitsadmin GetState**, que recupera el estado del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1252d6cf-14ca-44df-beb2-930ff011f297
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 43cd8c8e614cce65f55b16fc5395b1d37de0cf95
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850468"
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

## <a name="output"></a>Salida

Los valores de salida incluyen:

| Estado | Descripción |
| --------------- | ----------- |
| En cola | El trabajo está en espera de ejecutarse. |
| Conectando | BITS está poniéndose en contacto con el servidor. |
| Transferir | BITS está transfiriendo datos. |
| Traslada | BITS transfirió correctamente todos los archivos del trabajo. |
| Suspended | El trabajo está en pausa. |
| Error | Se produjo un error no recuperable. no se volverá a intentar la transferencia. |
| Transient_Error | Se ha producido un error recuperable. la transferencia se reintenta cuando expira el retraso de reintento mínimo. |
| Confirmado | Se completó el trabajo. |
| Canceled | El trabajo se canceló. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recupera el estado del trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /getstate myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
