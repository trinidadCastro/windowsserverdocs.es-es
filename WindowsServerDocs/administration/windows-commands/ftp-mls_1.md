---
title: mls_1 FTP
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4738fd49-0e80-4bdf-a773-0f973db3a710 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a84a0f8f3121ea19876744e9ef04bebf5f9fcb08
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376256"
---
# <a name="ftp-mls_1"></a>FTP: mls_1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra una lista abreviada de archivos y subdirectorios de un directorio remoto.   
## <a name="syntax"></a>Sintaxis  
```  
mls <remoteFile>[ ] <LocalFile>  
```  
### <a name="parameters"></a>Parámetros  

|  Parámetro   |                       Descripción                       |
|--------------|---------------------------------------------------------|
| <remoteFile> | Especifica el archivo para el que desea ver una lista. |
| <LocalFile>  |  Especifica un archivo local en el que se va a almacenar la lista.  |

## <a name="remarks"></a>Comentarios  
- Especificar *remoteFiles*  
  Escriba un guión ( **-** ) para usar el directorio de trabajo actual en el equipo remoto.  
- Especificar *archivolocal*  
  Escriba un guión ( **-** ) para mostrar la lista en pantalla.  
  ## <a name="BKMK_Examples"></a>Example  
  Mostrar una lista abreviada de archivos y subdirectorios para **dir1** y **dir2**.  
  ```  
  mls dir1 dir2 -  
  ```  
  Guardar una lista abreviada de archivos y subdirectorios para **dir1** y **dir2** en el archivo local **dirlist. txt**  
  ```  
  mls dir1 dir2 dirlist.txt   
  ```  
  ## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
