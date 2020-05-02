---
title: DIR FTP
description: Tema de referencia para el directorio FTP
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a29a92a5-7b79-4e6e-95cf-2ccb38bb6fb2 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0f2b9c610abe50bf662439a84d9bcbcb17b1a5bb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725315"
---
# <a name="ftp-dir"></a>FTP: dir

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra una lista de los archivos de directorio y los subdirectorios de un equipo remoto.   
## <a name="syntax"></a>Sintaxis  
```  
dir [<remotedirectory>] [<LocalFile>]  
```  
#### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|[<remotedirectory>]|Especifica el directorio para el que desea ver una lista. Si no se especifica ningún directorio, se utiliza el directorio de trabajo actual en el equipo remoto.|  
|[<LocalFile>]|Especifica un archivo local en el que se va a almacenar la lista de directorios. Si no se especifica un archivo local, los resultados se muestran en la pantalla.|  
## <a name="examples"></a>Ejemplos  
Mostrar una lista de directorios para **dir1** en el equipo remoto.  
```  
dir dir1  
```  
Guarde una lista del directorio actual en el equipo remoto en el archivo local **dirlist. txt**.  
```  
dir . dirlist.txt  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
