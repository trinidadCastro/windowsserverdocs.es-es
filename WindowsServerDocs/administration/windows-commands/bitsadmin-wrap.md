---
title: bitsadmin wrap
description: Artículo de referencia del comando bitsadmin Wrap, que ajusta cualquier línea de texto de salida que se extienda más allá del borde derecho de la ventana de comandos a la línea siguiente.
ms.topic: article
ms.assetid: 14e57522-539d-4621-ad15-09f7a44ccab7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5d17678ec735f9e7d6319368b0b35a67b47ea576
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87880760"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Ajusta cualquier línea de texto de salida que se extienda más allá del borde derecho de la ventana de comandos a la línea siguiente. Debe especificar este modificador antes que cualquier otro modificador.

De forma predeterminada, todos los conmutadores excepto el modificador de la [monitor bitsadmin](bitsadmin-monitor.md) ajustan el texto de salida.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /wrap <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ---------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar información del trabajo denominado *myDownloadJob* y ajustar el texto de salida:

```
bitsadmin /wrap /info myDownloadJob /verbose
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
