---
title: DIR FTP
description: Temas de comandos de Windows para el directorio FTP
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c47971c52135d79ce62f935bfed981f6eefcecaa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376442"
---
# <a name="ftp-dir"></a>FTP: dir

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra una lista de los archivos de directorio y los subdirectorios de un equipo remoto.   
## <a name="syntax"></a>Sintaxis  
```  
dir [<remotedirectory>] [<LocalFile>]  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|[<remotedirectory>]|Especifica el directorio para el que desea ver una lista. Si no se especifica ningún directorio, se utiliza el directorio de trabajo actual en el equipo remoto.|  
|[<LocalFile>]|Especifica un archivo local en el que se va a almacenar la lista de directorios. Si no se especifica un archivo local, los resultados se muestran en la pantalla.|  
## <a name="BKMK_Examples"></a>Example  
Mostrar una lista de directorios para **dir1** en el equipo remoto.  
```  
dir dir1  
```  
Guarde una lista del directorio actual en el equipo remoto en el archivo local **dirlist. txt**.  
```  
dir . dirlist.txt  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
