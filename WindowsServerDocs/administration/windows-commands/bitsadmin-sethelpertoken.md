---
title: bitsadmin sethelpertoken
description: 'Windows Commands topic for **bitsadmin sethelpertoken** : establece el token principal del símbolo del sistema actual (o un token de cuenta de usuario local arbitrario, si se especifica) como token auxiliar de un trabajo de transferencia de bits.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 91c03366998168dad9ab4530ef36a5020b8ad6ec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380571"
---
# <a name="bitsadmin-sethelpertoken"></a>bitsadmin sethelpertoken

Establece el token principal del símbolo del sistema actual (o el token de una cuenta de usuario local arbitraria, si se especifica) como [token auxiliar](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs)de un trabajo de transferencia de bits.

**BITS 3,0 y versiones anteriores**: no se admiten.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetHelperToken <Job> [\<username@domain\> \<password\>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar o el GUID del trabajo.|
|\<username@domain\> \<contraseña\>|Opcional&mdash;las credenciales de una cuenta de usuario local cuyo token se va a usar.|

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
