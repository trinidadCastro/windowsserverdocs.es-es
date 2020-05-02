---
title: MDIR FTP
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90eec45b-558b-4b8d-bbe4-b56d98e1ca70 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 54adcd1d28079487c5e238c72413d8269447944e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725025"
---
# <a name="ftp-mdir"></a>FTP: MDIR

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra una lista de directorios de archivos y subdirectorios de un directorio remoto.   
## <a name="syntax"></a>Sintaxis  
```  
mdir <remoteFile>[ ] <LocalFile>  
```  
#### <a name="parameters"></a>Parámetros  

|  Parámetro   |                               Descripción                                |
|--------------|--------------------------------------------------------------------------|
| <remoteFile> |   Especifica el directorio o el archivo para el que desea ver una lista.   |
| <LocalFile>  | Especifica un archivo local para almacenar la lista. Este parámetro es obligatorio. |

## <a name="remarks"></a>Observaciones  
- Puede usar **MDIR** para especificar varios archivos.  
- Especificar *archivoremoto*  
  Escriba un guión (**-**) para usar el directorio de trabajo actual en el equipo remoto.  
- Especificar un *archivolocal*  
  Escriba un guion (**-**) para mostrar la lista en pantalla.  
  ## <a name="examples"></a>Ejemplos  
  Mostrar una lista de directorios de **dir1** y **dir2** en la pantalla  
  ```  
  mdir dir1 dir2 -  
  ```  
  Guarde la lista de directorios combinados de **dir1** y **dir2** en un archivo local denominado **dirlist. txt.**  
  ```  
  mdir dir1 dir2 dirlist.txt  
  ```  
  ## <a name="additional-references"></a>Referencias adicionales  
- - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
