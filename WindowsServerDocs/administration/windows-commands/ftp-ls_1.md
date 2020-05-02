---
title: ls_1 FTP
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5e03c7db-1e2b-419c-acb2-8a68f3db9615 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0dd661742288bdd43299f379f4eb04016b2ac799
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725239"
---
# <a name="ftp-ls_1"></a>FTP: ls_1

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012
> 
> 
> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra una lista abreviada de archivos y subdirectorios del equipo remoto.   
## <a name="syntax"></a>Sintaxis  
```  
ls [<remotedirectory>] [<LocalFile>]  
```  
#### <a name="parameters"></a>Parámetros  

|      Parámetro      |                                                                       Descripción                                                                        |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| [<remotedirectory>] | Especifica el directorio para el que desea ver una lista. Si no se especifica ningún directorio, se utiliza el directorio de trabajo actual en el equipo remoto. |
|    [<LocalFile>]    |               Especifica un archivo local en el que se va a almacenar la lista. Si no se especifica un archivo local, los resultados se muestran en la pantalla.               |

## <a name="examples"></a>Ejemplos  
Mostrar una lista abreviada de archivos y subdirectorios del equipo remoto.  
```  
ls  
```  
Obtener una lista abreviada de directorios de **dir1** en el equipo remoto y guardarlo en un archivo local denominado **dirlist. txt**  
```  
ls dir1 dirlist.txt   
```  
## <a name="additional-references"></a>Referencias adicionales  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
