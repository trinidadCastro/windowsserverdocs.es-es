---
title: versión y versión de bitsadmin
description: Windows Commands topic for **bitsadmin util and version** -muestra la versión del servicio bits.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 495ef17bbf6f39f20f6729b64de4b4bec0f9a3c2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380198"
---
# <a name="bitsadmin-util-and-version"></a>versión y versión de bitsadmin

Muestra la versión del servicio BITS (por ejemplo, 2,0).

**BITSAdmin 1,5 y versiones anteriores**: No compatible.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Util /Version [/Verbose]
```

## <a name="remarks"></a>Comentarios

El modificador **verbose** realiza lo siguiente:
-   Muestra la versión del archivo para cada archivo DLL relacionado con BITS
-   Comprueba que se puede iniciar el servicio BITS
-   Muestra los valores de directiva de grupo BITS (solo Windows Vista)

## <a name="BKMK_examples"></a>Example

En el siguiente ejemplo se trata de la versión del servicio BITS.
```
C:\>bitsadmin /Util /Version
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)