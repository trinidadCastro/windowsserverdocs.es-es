---
title: mls_1 FTP
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4738fd49-0e80-4bdf-a773-0f973db3a710 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca3b8e04dd4a152b2d1bf8ce1ca8006d70186116
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843298"
---
# <a name="ftp-mls_1"></a>FTP: mls_1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra una lista abreviada de archivos y subdirectorios de un directorio remoto.   
## <a name="syntax"></a>Sintaxis  
```  
mls <remoteFile>[ ] <LocalFile>  
```  
#### <a name="parameters"></a>Parámetros  

|  Parámetro   |                       Descripción                       |
|--------------|---------------------------------------------------------|
| <remoteFile> | Especifica el archivo para el que desea ver una lista. |
| <LocalFile>  |  Especifica un archivo local en el que se va a almacenar la lista.  |

## <a name="remarks"></a>Comentarios  
- Especificar *remoteFiles*  
  Escriba un guión ( **-** ) para usar el directorio de trabajo actual en el equipo remoto.  
- Especificar *archivolocal*  
  Escriba un guión ( **-** ) para mostrar la lista en pantalla.  
  ## <a name="examples"></a><a name=BKMK_Examples></a>Example  
  Mostrar una lista abreviada de archivos y subdirectorios para **dir1** y **dir2**.  
  ```  
  mls dir1 dir2 -  
  ```  
  Guardar una lista abreviada de archivos y subdirectorios para **dir1** y **dir2** en el archivo local **dirlist. txt**  
  ```  
  mls dir1 dir2 dirlist.txt   
  ```  
  ## <a name="additional-references"></a>Referencias adicionales  
- - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
