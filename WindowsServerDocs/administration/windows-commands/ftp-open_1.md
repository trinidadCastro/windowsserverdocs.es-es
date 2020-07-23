---
title: ftp open
description: Artículo de referencia para el comando FTP Open, que se conecta al servidor FTP especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4b61926a-dc60-4b4c-96d3-64e5c91c18ba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 265668809040d7644d698c8b9bc0da5371fb5499
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86957637"
---
# <a name="ftp-open"></a>ftp open

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Se conecta al servidor FTP especificado.

## <a name="syntax"></a>Sintaxis

```
open <computer> [<port>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<computer>` | Especifica el equipo remoto al que está intentando conectarse. Puede usar una dirección IP o un nombre de equipo (en cuyo caso debe haber disponible un servidor DNS o un archivo hosts). |
| `[<port>]` | Especifica el número de puerto TCP que se va a utilizar para conectarse a un servidor FTP. De forma predeterminada, se usa el puerto TCP 21. |

### <a name="examples"></a>Ejemplos

Para conectarse al servidor FTP en *FTP.Microsoft.com*, escriba:

```
open ftp.microsoft.com
```

Para conectarse al servidor FTP en *FTP.Microsoft.com* que escucha en el puerto TCP *755*, escriba:

```
open ftp.microsoft.com 755
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Guía de FTP adicional](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
