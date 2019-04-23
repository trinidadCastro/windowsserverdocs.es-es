---
title: Bitsadmin sethelpertoken
description: Tema de los comandos de Windows para **sethelpertoken bitsadmin** -establece el token primario de la línea de comandos actual (o token de la cuenta de usuario local arbitrarias, si se especifica) como token auxiliar de un trabajo transferencia de BITS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 558a1aca66a7b3ec447136ceff9237d13efe4ede
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853006"
---
# <a name="bitsadmin-sethelpertoken"></a>Bitsadmin sethelpertoken

Establece el token primario de la línea de comandos actual (o token de la cuenta de usuario local arbitrarias, si se especifica) como un trabajo de transferencia de BITS [token auxiliar](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs).

**3.0 y versiones anteriores de BITS**: No compatible.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetHelperToken <Job> [\<username@domain\> \<password\>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo.|
|\<username@domain\> \<Contraseña\>|Opcional&mdash;las credenciales de un usuario local cuyo token que se usará la cuenta.|

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
