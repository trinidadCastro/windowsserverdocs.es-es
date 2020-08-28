---
title: ftp open
description: Artículo de referencia para el comando FTP Open, que se conecta al servidor FTP especificado.
ms.topic: reference
ms.assetid: 4b61926a-dc60-4b4c-96d3-64e5c91c18ba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a7599eb4728a46655b4c3274a9d7708061c3a9ee
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032823"
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
