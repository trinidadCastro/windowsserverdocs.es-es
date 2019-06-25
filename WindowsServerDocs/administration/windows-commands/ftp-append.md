---
title: Anexar FTP
description: 'Anexe el tema de los comandos de Windows para ftp '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9580d725120bb32a9b915d37cdbc173bfb17b859
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438845"
---
# <a name="ftp-append"></a>FTP: anexar

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

anexa un archivo local a un archivo en el equipo remoto mediante la configuración de tipo de archivo actual.   
## <a name="syntax"></a>Sintaxis  
```  
append <LocalFile> [remoteFile]  
```  
### <a name="parameters"></a>Parámetros  

|  Parámetro   |                               Descripción                                |
|--------------|--------------------------------------------------------------------------|
| <LocalFile>  |                     Especifica el archivo local para agregar.                     |
| [remoteFile] | Especifica el archivo en el equipo remoto al que <LocalFile> se agrega. |

## <a name="remarks"></a>Comentarios  
Si *archivoRemoto* se omite, el *archivoLocal* nombre se usa en lugar del nombre de archivo remoto.  
## <a name="BKMK_Examples"></a>Ejemplos  
Anexar file1.txt a file2.txt en el equipo remoto.  
```  
append file1.txt file2.txt  
```  
Anexe el file1.txt local a un archivo denominado file1.txt en el equipo remoto.  
```  
append file1.txt  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  