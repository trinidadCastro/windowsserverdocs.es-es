---
title: ftp get
description: Artículo de referencia para el comando FTP get, que copia un archivo remoto en el equipo local mediante el tipo de transferencia de archivos actual.
ms.topic: reference
ms.assetid: d70355c4-58ef-43e0-916b-c7ecf77e6ee4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 749c32160c58e2d59563b2355fa0b4c6e6a4c82a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89621772"
---
# <a name="ftp-get"></a>ftp get

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Copia un archivo remoto en el equipo local mediante el tipo de transferencia de archivos actual.

> [!NOTE]
> Este comando es el mismo que el [comando recepción de FTP](ftp-recv.md).

## <a name="syntax"></a>Sintaxis

```
get <remotefile> [<localfile>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<remotefile>` | Especifica el archivo remoto que se va a copiar. |
| `[<localfile>]` | Especifica el nombre del archivo que se va a usar en el equipo local. Si no se especifica *archivolocal* , el archivo recibe el nombre del *archivoremoto*. |

### <a name="examples"></a>Ejemplos

Para copiar *test.txt* en el equipo local mediante la transferencia de archivos actual, escriba:

```
get test.txt
```

Para copiar *test.txt* en el equipo local como *test1.txt* mediante la transferencia de archivos actual, escriba:

```
get test.txt test1.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando RECV de FTP](ftp-recv.md)

- [comando ASCII de FTP](ftp-ascii.md)

- [comando binario de FTP](ftp-binary.md)

- [Guía de FTP adicional](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
