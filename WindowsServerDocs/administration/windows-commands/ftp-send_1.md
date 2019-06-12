---
title: send_1 de FTP
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 000aa80a-60a0-4b51-815f-3237a4f3e0f4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 39423aff3c64f41c4fc0f8998484e6dcc38f822e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438439"
---
# <a name="ftp-send1"></a>FTP: send_1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia un archivo local en el equipo remoto mediante el tipo de transferencia de archivos actual.   
## <a name="syntax"></a>Sintaxis  
```  
send <LocalFile> [<remoteFile>]  
```  
### <a name="parameters"></a>Parámetros  

|  Parámetro   |                    Descripción                    |
|--------------|---------------------------------------------------|
| <LocalFile>  |         Especifica el archivo local para copiar.         |
| <remoteFile> | Especifica el nombre que se usará en el equipo remoto. |

## <a name="remarks"></a>Comentarios  
- El **enviar** comando es idéntico a la **colocar** comando.  
- Si *archivoRemoto* no se especifica, el archivo tendrá el *archivoLocal* nombre.  
  ## <a name="BKMK_Examples"></a>Ejemplos  
  Copie el archivo local **test.txt** y asígnele el nombre **test1.txt** en el equipo remoto.  
  ```  
  send test.txt test1.txt  
  ```  
  Copie el archivo local **program.exe** al equipo remoto.  
  ```  
  send program.exe  
  ```  
  ## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
