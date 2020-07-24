---
title: bitsadmin gethelpertokensid
description: Artículo de referencia para el comando bitsadmin gethelpertokensid, que devuelve el SID del token auxiliar de un trabajo de transferencia de BITS, si se ha establecido uno.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 3c8ef7502c524994454c697e2fa7d5dee223da5d
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86955467"
---
# <a name="bitsadmin-gethelpertokensid"></a>bitsadmin gethelpertokensid

Devuelve el SID de un [token auxiliar](/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)de un trabajo de transferencia de bits, si se ha establecido uno.

> [!NOTE]
> Este comando no es compatible con BITS 3,0 y versiones anteriores.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /gethelpertokensid <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar el SID de un trabajo de transferencia de BITS denominado *myDownloadJob*:

```
bitsadmin /gethelpertokensid myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
