---
title: ftp ls_1
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5e03c7db-1e2b-419c-acb2-8a68f3db9615 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6abf8466f90ac29846f2e1ee7d305e7e4280231e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438632"
---
# <a name="ftp-ls1"></a>ftp: ls_1

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> 
> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra una lista abreviada de archivos y subdirectorios desde el equipo remoto.   
## <a name="syntax"></a>Sintaxis  
```  
ls [<remotedirectory>] [<LocalFile>]  
```  
### <a name="parameters"></a>Parámetros  

|      Parámetro      |                                                                       Descripción                                                                        |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| [<remotedirectory>] | Especifica el directorio para el que desea ver una lista. Si no se especifica ningún directorio, se usa el directorio de trabajo actual en el equipo remoto. |
|    [<LocalFile>]    |               Especifica un archivo local en el que se va a almacenar la lista. Si no se especifica un archivo local, los resultados se muestran en la pantalla.               |

## <a name="BKMK_Examples"></a>Ejemplos  
Mostrar una lista abreviada de archivos y subdirectorios desde el equipo remoto.  
```  
ls  
```  
Obtener un listado de directorios abreviada de **dir1** en el equipo remoto y guardarlo en un archivo local denominado **ListaDir.txt**  
```  
ls dir1 dirlist.txt   
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
