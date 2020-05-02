---
title: mls_1 FTP
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4738fd49-0e80-4bdf-a773-0f973db3a710 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 27f74d4c1d03cb4d9f665566f69485e80f8eccdc
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725207"
---
# <a name="ftp-mls_1"></a>FTP: mls_1

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

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

## <a name="remarks"></a>Observaciones  
- Especificar *remoteFiles*  
  Escriba un guión (**-**) para usar el directorio de trabajo actual en el equipo remoto.  
- Especificar *archivolocal*  
  Escriba un guion (**-**) para mostrar la lista en pantalla.  
  ## <a name="examples"></a>Ejemplos  
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
