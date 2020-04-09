---
title: bitsadmin nowrap
description: Tema de comandos de Windows para **bitsadmin NoWrap**, que trunca cualquier línea de texto de salida que se extienda más allá del borde derecho de la ventana de comandos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 85a47b90-783a-41e4-96f2-81f26ae8ca93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f9f1db370d8a8917aa03a414a27623a1024df192
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850188"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

Trunca cualquier línea de texto de salida que se extienda más allá del borde derecho de la ventana de comandos. De forma predeterminada, todos los modificadores, excepto el modificador **monitor** , encapsulan el resultado. Especifique el modificador **noWrap** antes de otros modificadores.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /nowrap
```

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recupera el estado del trabajo denominado *myDownloadJob* y no se encapsula el resultado.

```
C:\>bitsadmin /nowrap /getstate myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)