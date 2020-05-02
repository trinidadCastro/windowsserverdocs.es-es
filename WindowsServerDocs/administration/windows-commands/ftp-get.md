---
title: FTP Get
description: Tema de referencia de FTP Get
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d70355c4-58ef-43e0-916b-c7ecf77e6ee4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 37423f81fd72e79cebcdf169160ad3e8033b69b2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725293"
---
# <a name="ftp-get"></a>FTP: get

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Copia un archivo remoto en el equipo local mediante el tipo de transferencia de archivos actual.   
## <a name="syntax"></a>Sintaxis  
```  
get <remoteFile> [<LocalFile>]  
```  
#### <a name="parameters"></a>Parámetros  

|   Parámetro   |                                                              Descripción                                                               |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------|
| <remoteFile>  |                                                   Especifica el archivo remoto que se va a copiar.                                                   |
| [<LocalFile>] | Especifica el nombre del archivo que se va a usar en el equipo local. Si no se especifica *archivolocal* , el archivo recibe el nombre de *archivoremoto* . |

## <a name="remarks"></a>Observaciones  
El comando **Get** es idéntico al comando **RECV** .  
## <a name="examples"></a>Ejemplos  
Copie **Test. txt** en el equipo local con el tipo de transferencia de archivos actual.  
```  
get test.txt  
```  
Copie **Test. txt** en el equipo local como **Test1. txt** con el tipo de transferencia de archivo actual.  
```  
Get test.txt test1.txt  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [FTP: ASCII](ftp-ascii.md)  
-   [FTP: binario](ftp-binary.md)  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
