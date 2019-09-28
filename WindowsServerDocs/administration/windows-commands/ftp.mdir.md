---
title: MDIR FTP
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 90eec45b-558b-4b8d-bbe4-b56d98e1ca70 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 08aa5bb216a3d0155c100c761e476bb963e59311
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375850"
---
# <a name="ftp-mdir"></a>FTP: MDIR

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra una lista de directorios de archivos y subdirectorios de un directorio remoto.   
## <a name="syntax"></a>Sintaxis  
```  
mdir <remoteFile>[ ] <LocalFile>  
```  
### <a name="parameters"></a>Parámetros  

|  Parámetro   |                               Descripción                                |
|--------------|--------------------------------------------------------------------------|
| <remoteFile> |   Especifica el directorio o el archivo para el que desea ver una lista.   |
| <LocalFile>  | Especifica un archivo local para almacenar la lista. Este parámetro es obligatorio. |

## <a name="remarks"></a>Comentarios  
- Puede usar **MDIR** para especificar varios archivos.  
- Especificar *archivoremoto*  
  Escriba un guión ( **-** ) para usar el directorio de trabajo actual en el equipo remoto.  
- Especificar un *archivolocal*  
  Escriba un guión ( **-** ) para mostrar la lista en pantalla.  
  ## <a name="BKMK_Examples"></a>Example  
  Mostrar una lista de directorios de **dir1** y **dir2** en la pantalla  
  ```  
  mdir dir1 dir2 -  
  ```  
  Guarde la lista de directorios combinados de **dir1** y **dir2** en un archivo local denominado **dirlist. txt.**  
  ```  
  mdir dir1 dir2 dirlist.txt  
  ```  
  ## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
