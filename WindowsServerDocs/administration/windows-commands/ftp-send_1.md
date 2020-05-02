---
title: send_1 FTP
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 000aa80a-60a0-4b51-815f-3237a4f3e0f4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 04617246c05edde127db01ce1a0fe692eb0aceb1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725114"
---
# <a name="ftp-send_1"></a>FTP: send_1

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Copia un archivo local en el equipo remoto mediante el tipo de transferencia de archivos actual.   
## <a name="syntax"></a>Sintaxis  
```  
send <LocalFile> [<remoteFile>]  
```  
#### <a name="parameters"></a>Parámetros  

|  Parámetro   |                    Descripción                    |
|--------------|---------------------------------------------------|
| <LocalFile>  |         Especifica el archivo local que se va a copiar.         |
| <remoteFile> | Especifica el nombre que se va a usar en el equipo remoto. |

## <a name="remarks"></a>Observaciones  
- El comando **send** es idéntico al comando **Put** .  
- Si no se especifica *archivoremoto* , el archivo recibe el nombre *archivolocal* .  
  ## <a name="examples"></a>Ejemplos  
  Copie el archivo local **Test. txt** y asígnele el nombre **prueba1. txt** en el equipo remoto.  
  ```  
  send test.txt test1.txt  
  ```  
  Copie el archivo local **Program. exe** en el equipo remoto.  
  ```  
  send program.exe  
  ```  
  ## <a name="additional-references"></a>Referencias adicionales  
- - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
