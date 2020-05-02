---
title: mdelete_1 FTP
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8a80a8f5-e880-40a8-abc9-29a41836844f vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f578a50207439f9bfb21c2607f0aa60a20fad292
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725038"
---
# <a name="ftp-mdelete_1"></a>FTP: mdelete_1

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

elimina archivos del equipo remoto.   
## <a name="syntax"></a>Sintaxis  
```  
mdelete <remoteFile>[ ]  
```  
#### <a name="parameters"></a>Parámetros  

|  Parámetro   |             Descripción              |
|--------------|--------------------------------------|
| <remoteFile> | Especifica el archivo remoto que se va a eliminar. |

## <a name="examples"></a>Ejemplos  
Elimine los archivos remotos **a. exe** y **b. exe**.  
```  
mdelete a.exe b.exe  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
