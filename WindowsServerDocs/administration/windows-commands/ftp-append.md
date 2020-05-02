---
title: anexar FTP
description: Tema de referencia para anexar FTP
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b53228473b8ea16a0955c244d60fae77cf4f7d7f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725404"
---
# <a name="ftp-append"></a>FTP: Append

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

anexa un archivo local a un archivo en el equipo remoto con la configuración de tipo de archivo actual.   
## <a name="syntax"></a>Sintaxis  
```  
append <LocalFile> [remoteFile]  
```  
#### <a name="parameters"></a>Parámetros  

|  Parámetro   |                               Descripción                                |
|--------------|--------------------------------------------------------------------------|
| <LocalFile>  |                     Especifica el archivo local que se va a agregar.                     |
| Archivoremoto | Especifica el archivo en el equipo remoto al que <LocalFile> se agrega. |

## <a name="remarks"></a>Observaciones  
Si se omite *archivoremoto* , se usa el nombre de *archivolocal* en lugar del nombre de archivo remoto.  
## <a name="examples"></a>Ejemplos  
Anexe archivo1. txt a archivo2. txt en el equipo remoto.  
```  
append file1.txt file2.txt  
```  
Anexe el archivo1. txt local a un archivo denominado archivo1. txt en el equipo remoto.  
```  
append file1.txt  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
