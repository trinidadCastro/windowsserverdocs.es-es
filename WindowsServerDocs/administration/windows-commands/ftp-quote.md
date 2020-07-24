---
title: ftp quote
description: Artículo de referencia para el comando presupuesto de FTP, que envía argumentos textuales al servidor FTP remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4500a1d3-c091-42c7-a909-f61df7f2e993
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 83e2ca9e40120c78f11f3d6e1bfaeb1db161b796
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86957527"
---
# <a name="ftp-quote"></a>ftp quote

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Envía argumentos textuales al servidor FTP remoto. Se devuelve un solo código de respuesta FTP.

> [!NOTE]
> Este comando es el mismo que el [comando literal de FTP](ftp-literal_1.md).

## <a name="syntax"></a>Sintaxis

```
quote <argument>[ ]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<argument>` | Especifica el argumento que se va a enviar al servidor FTP. |

### <a name="examples"></a>Ejemplos

Para enviar un comando **Quit** al servidor FTP remoto, escriba:

```
quote quit
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando literal de FTP](ftp-literal_1.md)

- [Guía de FTP adicional](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
