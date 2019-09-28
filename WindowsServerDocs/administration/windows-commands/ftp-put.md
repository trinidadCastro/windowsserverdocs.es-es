---
title: Put de FTP
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95cc1e3f-523d-4374-98b8-16e6c276b2ca vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 15c1734322d3642ebc85891b71c6ad68100d514d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376073"
---
# <a name="ftp-put"></a>FTP: put

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia un archivo local en el equipo remoto mediante el tipo de transferencia de archivos actual.   
## <a name="syntax"></a>Sintaxis  
```  
put <LocalFile> [<remoteFile>]  
```  
### <a name="parameters"></a>Parámetros  

|   Parámetro    |                    Descripción                    |
|----------------|---------------------------------------------------|
|  <LocalFile>   |         Especifica el archivo local que se va a copiar.         |
| [<remoteFile>] | Especifica el nombre que se va a usar en el equipo remoto. |

## <a name="remarks"></a>Comentarios  
- El comando **Put** es idéntico al comando **send** .  
- Si no se especifica *archivoremoto* , el archivo recibe el nombre *archivolocal* .  
  ## <a name="BKMK_Examples"></a>Example  
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
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
