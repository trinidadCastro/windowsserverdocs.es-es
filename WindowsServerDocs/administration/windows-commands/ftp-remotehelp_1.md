---
title: ftp remotehelp
description: Artículo de referencia para el comando FTP remotehelp, que muestra ayuda para comandos remotos.
ms.topic: reference
ms.assetid: ef23adf3-ead4-44c8-ac1d-c8a6f4b2bf73
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c91fda83ea988f79237b27f6df6bd6062748e37c
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639373"
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

- [Guía de FTP adicional](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
