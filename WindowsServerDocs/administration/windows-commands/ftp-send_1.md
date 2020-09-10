---
title: ftp send
description: Artículo de referencia para el comando FTP Send, que copia un archivo local en el equipo remoto mediante el tipo de transferencia de archivos actual.
ms.topic: reference
ms.assetid: 000aa80a-60a0-4b51-815f-3237a4f3e0f4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: bd2451ba12e2b8017c50bd22df2f61bba253808a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89621607"
---
# <a name="ftp-send"></a>ftp send

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Copia un archivo local en el equipo remoto mediante el tipo de transferencia de archivos actual.

> [!NOTE]
> Este comando es el mismo que el [comando put de FTP](ftp-put.md).

## <a name="syntax"></a>Sintaxis

```
send <localfile> [<remotefile>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<localfile>` | Especifica el archivo local que se va a copiar. |
| `<remotefile>` | Especifica el nombre que se va a usar en el equipo remoto. Si no especifica un *archivoremoto*, el archivo obtendrá el nombre *archivolocal* . |

### <a name="examples"></a>Ejemplos

Para copiar el archivo local *test.txt* y asignarle el nombre *test1.txt* en el equipo remoto, escriba:

```
send test.txt test1.txt
```

Para copiar el archivo local *program.exe* en el equipo remoto, escriba:

```
send program.exe
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Guía de FTP adicional](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
