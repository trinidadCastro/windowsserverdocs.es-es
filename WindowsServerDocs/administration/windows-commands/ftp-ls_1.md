---
title: ls_1 FTP
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5e03c7db-1e2b-419c-acb2-8a68f3db9615 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5ff63272fe3c5e59965b04bef258a06fee2da0c4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843368"
---
# <a name="ftp-ls_1"></a>FTP: ls_1

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> 
> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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

## <a name="examples"></a><a name=BKMK_Examples></a>Example  
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
