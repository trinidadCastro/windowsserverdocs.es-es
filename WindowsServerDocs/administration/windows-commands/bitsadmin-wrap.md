---
title: bitsadmin wrap
description: Artículo de referencia del comando bitsadmin Wrap, que ajusta cualquier línea de texto de salida que se extienda más allá del borde derecho de la ventana de comandos a la línea siguiente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 14e57522-539d-4621-ad15-09f7a44ccab7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e47f5e38555eb2464d3febf5f958ce5a6af20452
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927252"
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
