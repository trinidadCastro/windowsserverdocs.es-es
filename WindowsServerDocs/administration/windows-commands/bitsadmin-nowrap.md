---
title: bitsadmin nowrap
description: Tema de referencia del comando bitsadmin NoWrap, que trunca cualquier línea de texto de salida que se extienda más allá del borde derecho de la ventana de comandos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 85a47b90-783a-41e4-96f2-81f26ae8ca93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2aac604ec3e13026e322d7cb7a9364df46266a0c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717334"
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
