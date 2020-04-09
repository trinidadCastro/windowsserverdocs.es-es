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
ms.openlocfilehash: ecd579a313fe1cad1b8a5b4a622aaaec2d6a6d63
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843138"
---
# <a name="ftp-put"></a>FTP: put

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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

## <a name="remarks"></a>Comentarios
- El comando **Put** es idéntico al comando **send** .
- Si no se especifica *archivoremoto* , el archivo recibe el nombre *archivolocal* .
  ## <a name="examples"></a><a name="BKMK_Examples"></a>Example
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
