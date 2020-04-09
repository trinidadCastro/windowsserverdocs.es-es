---
title: MDIR FTP
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90eec45b-558b-4b8d-bbe4-b56d98e1ca70 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: afb024d063761f1e817f02fdd7301376dd167846
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842718"
---
# <a name="ftp-mdir"></a>FTP: MDIR

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra una lista de directorios de archivos y subdirectorios de un directorio remoto.   
## <a name="syntax"></a>Sintaxis  
```  
mdir <remoteFile>[ ] <LocalFile>  
```  
#### <a name="parameters"></a>Parámetros  

|  Parámetro   |                               Descripción                                |
|--------------|--------------------------------------------------------------------------|
| <remoteFile> |   Especifica el directorio o el archivo para el que desea ver una lista.   |
| <LocalFile>  | Especifica un archivo local para almacenar la lista. Este parámetro es necesario. |

## <a name="remarks"></a>Comentarios  
- Puede usar **MDIR** para especificar varios archivos.  
- Especificar *archivoremoto*  
  Escriba un guión ( **-** ) para usar el directorio de trabajo actual en el equipo remoto.  
- Especificar un *archivolocal*  
  Escriba un guión ( **-** ) para mostrar la lista en pantalla.  
  ## <a name="examples"></a><a name=BKMK_Examples></a>Example  
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
