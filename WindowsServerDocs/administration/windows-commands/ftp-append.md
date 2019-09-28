---
title: anexar FTP
description: 'Temas de comandos de Windows para anexar FTP '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 52d16b878ff5fb165fd851b227dcc361c9da3a80
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376636"
---
# <a name="ftp-append"></a>FTP: Append

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

anexa un archivo local a un archivo en el equipo remoto con la configuración de tipo de archivo actual.   
## <a name="syntax"></a>Sintaxis  
```  
append <LocalFile> [remoteFile]  
```  
### <a name="parameters"></a>Parámetros  

|  Parámetro   |                               Descripción                                |
|--------------|--------------------------------------------------------------------------|
| <LocalFile>  |                     Especifica el archivo local que se va a agregar.                     |
| Archivoremoto | Especifica el archivo en el equipo remoto al que se agrega <LocalFile>. |

## <a name="remarks"></a>Comentarios  
Si se omite *archivoremoto* , se usa el nombre de *archivolocal* en lugar del nombre de archivo remoto.  
## <a name="BKMK_Examples"></a>Example  
Anexe archivo1. txt a archivo2. txt en el equipo remoto.  
```  
append file1.txt file2.txt  
```  
Anexe el archivo1. txt local a un archivo denominado archivo1. txt en el equipo remoto.  
```  
append file1.txt  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
