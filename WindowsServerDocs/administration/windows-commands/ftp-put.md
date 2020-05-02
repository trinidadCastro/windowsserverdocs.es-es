---
title: Put de FTP
description: Temas de comandos de Windows para FTP-Put
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 95cc1e3f-523d-4374-98b8-16e6c276b2ca vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/30/2020
ms.openlocfilehash: d86ba8418f4c43c26f3745a9f70e676ca790c640
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725178"
---
# <a name="ftp-put"></a>FTP: put

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Copia un archivo local en el equipo remoto mediante el tipo de transferencia de archivos actual.
## <a name="syntax"></a>Sintaxis
```
put <LocalFile> [<remoteFile>]
```
#### <a name="parameters"></a>Parámetros

|    Parámetro     |                    Descripción                    |
|------------------|---------------------------------------------------|
|   `<LocalFile>`  |         Especifica el archivo local que se va a copiar.         |
| `[<remoteFile>]` | Especifica el nombre que se va a usar en el equipo remoto. |

## <a name="remarks"></a>Observaciones
- El comando **Put** es idéntico al comando **send** .
- Si no se especifica *archivoremoto* , el archivo recibe el nombre *archivolocal* .
  ## <a name="examples"></a>Ejemplos
  Copie el archivo local **Test. txt** y asígnele el nombre **prueba1. txt** en el equipo remoto.
  ```
  put test.txt test1.txt
  ```
  Copie el archivo local **Program. exe** en el equipo remoto.
  ```
  put program.exe
  ```
  ## <a name="additional-references"></a>Referencias adicionales
- [FTP: ASCII](ftp-ascii.md)
- [FTP: binario](ftp-binary.md)
- - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
