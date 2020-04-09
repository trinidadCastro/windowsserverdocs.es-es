---
title: bitsadmin sethelpertoken
description: Windows Commands topic for bitsadmin sethelpertoken, que establece el token principal del símbolo del sistema actual (o un token de cuenta de usuario local arbitrario, si se especifica) como token auxiliar de un trabajo de transferencia de BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: a1e8fd0054cadf3bf06b6e5b7bdf5010b18781e1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849538"
---
# <a name="bitsadmin-sethelpertoken"></a>bitsadmin sethelpertoken

Establece el token principal del símbolo del sistema actual (o el token de una cuenta de usuario local arbitraria, si se especifica) como [token auxiliar](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs)de un trabajo de transferencia de bits.

**BITS 3,0 y versiones anteriores**: no se admiten.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetHelperToken <Job> [\<username@domain\> \<password\>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar o el GUID del trabajo.|
|\<username@domain\> \<contraseña\>|Opcional&mdash;las credenciales de una cuenta de usuario local cuyo token se va a usar.|

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
