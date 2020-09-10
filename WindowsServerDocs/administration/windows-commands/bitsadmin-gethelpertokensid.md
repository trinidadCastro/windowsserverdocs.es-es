---
title: bitsadmin gethelpertokensid
description: Artículo de referencia para el comando bitsadmin gethelpertokensid, que devuelve el SID del token auxiliar de un trabajo de transferencia de BITS, si se ha establecido uno.
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 03/01/2019
ms.openlocfilehash: 645957ab5526c98e806a4e7875e1baece3d375b3
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632040"
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
