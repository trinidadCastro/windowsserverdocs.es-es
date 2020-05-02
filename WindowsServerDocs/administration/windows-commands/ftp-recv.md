---
title: recepción de FTP
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f249ce61-247d-421b-9b93-48bce5108800 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5ece259f2d48e18f6a789d51b1df7089490f2fa1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725125"
---
# <a name="ftp-recv"></a>FTP: RECV

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Copia un archivo remoto en el equipo local mediante el tipo de transferencia de archivos actual.   
## <a name="syntax"></a>Sintaxis  
```  
recv <remoteFile> [<LocalFile>]  
```  
#### <a name="parameters"></a>Parámetros  

|   Parámetro   |                   Descripción                    |
|---------------|--------------------------------------------------|
| <remoteFile>  |        Especifica el archivo remoto que se va a copiar.        |
| [<LocalFile>] | Especifica el nombre que se va a usar en el equipo local. |

## <a name="remarks"></a>Observaciones  
- El comando **RECV** es idéntico al comando **Get** .  
- Si no se especifica *archivolocal* , el archivo recibe el nombre de *archivoremoto* .  
  ## <a name="examples"></a>Ejemplos  
  Copie **Test. txt** en el equipo local con el tipo de transferencia de archivos actual.  
  ```  
  recv test.txt  
  ```  
  Copie **Test. txt** en el equipo local como **Test1. txt** con el tipo de transferencia de archivo actual.  
  ```  
  recv test.txt test1.txt  
  ```  
  ## <a name="additional-references"></a>Referencias adicionales  
- [FTP: ASCII](ftp-ascii.md)  
- [FTP: binario](ftp-binary.md)  
- [FTP: get](ftp-get.md)  
- - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
