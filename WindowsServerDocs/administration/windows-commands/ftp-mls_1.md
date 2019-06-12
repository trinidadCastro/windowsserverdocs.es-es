---
title: ftp mls_1
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 7a379ead9c56af096e121048a8c0f596f6879bb0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438541"
---
# <a name="ftp-mls1"></a>ftp: mls_1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra una lista abreviada de archivos y subdirectorios en un directorio remoto.   
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
- Especificar *archivosRemotos*  
  Escriba un guión ( **-** ) para usar el directorio de trabajo actual en el equipo remoto.  
- Especificar *archivoLocal*  
  Escriba un guión ( **-** ) para mostrar la lista en la pantalla.  
  ## <a name="BKMK_Examples"></a>Ejemplos  
  Mostrar una lista abreviada de archivos y subdirectorios para **dir1** y **dir2**.  
  ```  
  mls dir1 dir2 -  
  ```  
  Guardar una lista abreviada de archivos y subdirectorios para **dir1** y **dir2** en el archivo local **ListaDir.txt**  
  ```  
  mls dir1 dir2 dirlist.txt   
  ```  
  ## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
