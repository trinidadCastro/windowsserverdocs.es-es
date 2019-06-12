---
title: bitsadmin rawreturn
description: Tema de los comandos de Windows para **rawreturn bitsadmin** -devuelve adecuado para el análisis de datos.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4e12c8e621021d35ac618b4592515fe38c36be0e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434892"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

Devuelve los datos adecuados para el análisis.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /RawReturn
```

## <a name="remarks"></a>Comentarios

Caracteres de nueva línea de bandas y el formato de la salida.

Por lo general, use este comando junto con el **crear** y **obtener\\** * modificadores para recibir solo el valor. Debe especificar este modificador antes otros modificadores.

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente recupera los datos sin procesar para el estado del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /RawReturn /GetState myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)