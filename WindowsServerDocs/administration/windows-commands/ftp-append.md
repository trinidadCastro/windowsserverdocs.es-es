---
title: anexar FTP
description: Temas de comandos de Windows para anexar FTP
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44ce1d6e7259dc8745da35ed462e6378f0fce8ba
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843828"
---
# <a name="ftp-append"></a>FTP: Append

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

anexa un archivo local a un archivo en el equipo remoto con la configuración de tipo de archivo actual.   
## <a name="syntax"></a>Sintaxis  
```  
append <LocalFile> [remoteFile]  
```  
#### <a name="parameters"></a>Parámetros  

|  Parámetro   |                               Descripción                                |
|--------------|--------------------------------------------------------------------------|
| <LocalFile>  |                     Especifica el archivo local que se va a agregar.                     |
| Archivoremoto | Especifica el archivo en el equipo remoto al que se agrega <LocalFile>. |

## <a name="remarks"></a>Comentarios  
Si se omite *archivoremoto* , se usa el nombre de *archivolocal* en lugar del nombre de archivo remoto.  
## <a name="examples"></a><a name=BKMK_Examples></a>Example  
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
