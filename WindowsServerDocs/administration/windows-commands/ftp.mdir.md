---
title: ftp mdir
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0ac1e7cd50fe4d9325c272f74a7b81971c8bb12a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878216"
---
# <a name="ftp-mdir"></a>ftp: mdir

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra una lista de directorios de archivos y subdirectorios en un directorio remoto.   
## <a name="syntax"></a>Sintaxis  
```  
mdir <remoteFile>[ ] <LocalFile>  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|<remoteFile>|Especifica el directorio o archivo para el que desea ver una lista.|  
|<LocalFile>|Especifica un archivo local para almacenar la lista. Este parámetro es obligatorio.|  
## <a name="remarks"></a>Comentarios  
-   Puede usar **mdir** para especificar varios archivos.  
-   Especificar *archivoRemoto*  
    Escriba un guión (**-**) para usar el directorio de trabajo actual en el equipo remoto.  
-   Especificar un *archivoLocal*  
    Escriba un guión (**-**) para mostrar la lista en la pantalla.  
## <a name="BKMK_Examples"></a>Ejemplos  
Mostrar una lista de directorios de **dir1** y **dir2** en la pantalla  
```  
mdir dir1 dir2 -  
```  
Guardar la lista de directorios combinada de **dir1** y **dir2** en un archivo local denominado **ListaDir.txt**  
```  
mdir dir1 dir2 dirlist.txt  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
