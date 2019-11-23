---
title: bitsadmin rawreturn
description: 'Tema de comandos de Windows para **bitsadmin rawreturn** : devuelve datos adecuados para el análisis.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bbe97130-26f6-4cdd-84f1-baf530ce38b7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 86d769de460538acda696194348980de5752d6d8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380879"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

Devuelve los datos adecuados para el análisis.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /RawReturn
```

## <a name="remarks"></a>Observaciones

Quita los caracteres de nueva línea y el formato de la salida.

Normalmente, este comando se usa junto con los modificadores **Create** y **Get\\** * para recibir solo el valor. Debe especificar este modificador antes que otros modificadores.

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se recuperan los datos sin procesar para el estado del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /RawReturn /GetState myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)