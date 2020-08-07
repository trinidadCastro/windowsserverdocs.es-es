---
title: bitsadmin nowrap
description: Artículo de referencia del comando bitsadmin NoWrap, que trunca cualquier línea de texto de salida que se extienda más allá del borde derecho de la ventana de comandos.
ms.topic: article
ms.assetid: 85a47b90-783a-41e4-96f2-81f26ae8ca93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 72c50bdf7d19ea4603fc232dcf54acdfd97d3f17
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893649"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

Trunca cualquier línea de texto de salida que se extienda más allá del borde derecho de la ventana de comandos. De forma predeterminada, todos los modificadores, excepto el modificador **monitor** , encapsulan el resultado. Especifique el modificador **noWrap** antes de otros modificadores.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /nowrap
```

## <a name="examples"></a>Ejemplos

Para recuperar el estado del trabajo denominado *myDownloadJob* mientras no se ajusta la salida:

```
bitsadmin /nowrap /getstate myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
