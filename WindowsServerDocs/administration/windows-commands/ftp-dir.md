---
title: dir de FTP
description: Tema de los comandos de Windows para dir de ftp
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a29a92a5-7b79-4e6e-95cf-2ccb38bb6fb2 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 78ac8ac5e9fc4894f55401bb234aa98de981adf7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835256"
---
# <a name="ftp-dir"></a>ftp: dir

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra una lista de archivos del directorio y subdirectorios en un equipo remoto.   
## <a name="syntax"></a>Sintaxis  
```  
dir [<remotedirectory>] [<LocalFile>]  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|[<remotedirectory>]|Especifica el directorio para el que desea ver una lista. Si no se especifica ningún directorio, se usa el directorio de trabajo actual en el equipo remoto.|  
|[<LocalFile>]|Especifica un archivo local en el que se va a almacenar la lista de directorios. Si no se especifica un archivo local, los resultados se muestran en la pantalla.|  
## <a name="BKMK_Examples"></a>Ejemplos  
Mostrar una lista de directorios de **dir1** en el equipo remoto.  
```  
dir dir1  
```  
Guardar una lista del directorio actual en el equipo remoto en el archivo local **ListaDir.txt**.  
```  
dir . dirlist.txt  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
