---
title: versión y bitsadmin util
description: Tema de los comandos de Windows para **bitsadmin util y versión** -muestra la versión del servicio BITS.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2e768ec5ae43fc17c480b9deede698cca01c6291
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882876"
---
# <a name="bitsadmin-util-and-version"></a>versión y bitsadmin util

Muestra la versión del servicio de BITS (por ejemplo, 2.0).

**BITSAdmin 1.5 y versiones anterior**: No compatible.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Util /Version [/Verbose]
```

## <a name="remarks"></a>Comentarios

El **detallado** conmutador realiza lo siguiente:
-   Muestra la versión del archivo para cada DLL relacionada de BITS
-   Comprueba que se puede iniciar el servicio BITS.
-   Muestra los valores de directiva de grupo de BITS (sólo Windows Vista)

## <a name="BKMK_examples"></a>Ejemplos

El siguiente ejemplo la versión del servicio BITS.
```
C:\>bitsadmin /Util /Version
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)