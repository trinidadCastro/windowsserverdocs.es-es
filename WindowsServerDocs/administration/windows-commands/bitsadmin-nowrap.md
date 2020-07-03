---
title: bitsadmin nowrap
description: Artículo de referencia del comando bitsadmin NoWrap, que trunca cualquier línea de texto de salida que se extienda más allá del borde derecho de la ventana de comandos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 85a47b90-783a-41e4-96f2-81f26ae8ca93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8f55faeea79e5f2cf02fde0732ed82ae13e902f
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923022"
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
