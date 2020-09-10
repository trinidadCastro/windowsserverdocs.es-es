---
title: ftp open
description: Artículo de referencia para el comando FTP Open, que se conecta al servidor FTP especificado.
ms.topic: reference
ms.assetid: 4b61926a-dc60-4b4c-96d3-64e5c91c18ba
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 24fabf61e57a0dda00837a15702dfe9ef5ee6862
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635286"
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
