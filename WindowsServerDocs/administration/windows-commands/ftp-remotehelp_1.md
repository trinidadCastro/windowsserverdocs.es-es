---
title: ftp remotehelp
description: Artículo de referencia para el comando FTP remotehelp, que muestra ayuda para comandos remotos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef23adf3-ead4-44c8-ac1d-c8a6f4b2bf73
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 54d784751291515a022a561ca9600e3e967a9d8e
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85925743"
---
# <a name="ftp-remotehelp"></a>ftp remotehelp

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra la ayuda de los comandos remotos.

## <a name="syntax"></a>Sintaxis

```
remotehelp [<command>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ------- | -------- |
| `[<command>]` | Especifica el nombre del comando sobre el que se desea obtener ayuda. Si `<command>` no se especifica, este comando muestra una lista de todos los comandos remotos. También puede ejecutar comandos remotos mediante la [cotización FTP](ftp-quote.md) o el [literal FTP](ftp-literal_1.md). |

### <a name="examples"></a>Ejemplos

Para mostrar una lista de comandos remotos, escriba:

```
remotehelp
```

Para mostrar la sintaxis del comando remoto *feat* , escriba:

```
remotehelp feat
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [ftp quote](ftp-quote.md)

- [ftp literal](ftp-literal_1.md)

- [Guía de FTP adicional](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
