---
title: envío FTP
description: Tema de referencia del comando FTP Send, que copia un archivo local en el equipo remoto mediante el tipo de transferencia de archivos actual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 000aa80a-60a0-4b51-815f-3237a4f3e0f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 12ce45a0eb26e1aa4a0d7daace831751e1b67f4a
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820305"
---
# <a name="ftp-send"></a>envío FTP

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

Para copiar el archivo local *Test. txt* y asignarle el nombre *prueba1. txt* en el equipo remoto, escriba:

```
send test.txt test1.txt
```

Para copiar el archivo local *Program. exe* en el equipo remoto, escriba:

```
send program.exe
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Guía de FTP adicional](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
