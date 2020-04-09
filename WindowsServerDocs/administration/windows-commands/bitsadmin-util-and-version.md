---
title: versión y versión de bitsadmin
description: Tema de comandos de Windows para el util y la versión de bitsadmin, que muestra la versión del servicio BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 087cc1033166ab93e7496caaa7335433cafd6249
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848838"
---
# <a name="bitsadmin-util-and-version"></a>versión y versión de bitsadmin

Muestra la versión del servicio BITS (por ejemplo, 2,0).

**BITSAdmin 1,5 y versiones anteriores**: no se admiten.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Util /Version [/Verbose]
```

## <a name="remarks"></a>Comentarios

El modificador **verbose** realiza lo siguiente:
-   Muestra la versión del archivo para cada archivo DLL relacionado con BITS
-   Comprueba que se puede iniciar el servicio BITS
-   Muestra los valores de directiva de grupo BITS (solo Windows Vista)

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el siguiente ejemplo se trata de la versión del servicio BITS.
```
C:\>bitsadmin /Util /Version
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)