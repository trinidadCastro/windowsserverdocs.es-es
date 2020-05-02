---
title: bitsadmin sethelpertoken
description: Tema de referencia para el comando bitsadmin sethelpertoken, que establece el token primario del símbolo del sistema actual (o un token de cuenta de usuario local arbitrario, si se especifica) como token auxiliar de un trabajo de transferencia de BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: b125f95e262c2fd78f20266e3e2b6c80cea5a789
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719397"
---
# <a name="bitsadmin-sethelpertoken"></a>bitsadmin sethelpertoken

Establece el token principal del símbolo del sistema actual (o el token de una cuenta de usuario local arbitraria, si se especifica) como [token auxiliar](https://docs.microsoft.com/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)de un trabajo de transferencia de bits.

> [!NOTE]
> Este comando no es compatible con BITS 3,0 y versiones anteriores.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /sethelpertoken <job> [<user_name@domain> <password>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| `<username@domain>` `<password>` | Opcional. Las credenciales de la cuenta de usuario local cuyo token se va a usar. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
