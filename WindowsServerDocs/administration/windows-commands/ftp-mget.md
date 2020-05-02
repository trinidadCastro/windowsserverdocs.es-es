---
title: mget FTP
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a71fbfb60ae012b5e65af04e6f3e21ec796996a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725233"
---
# <a name="ftp-mget"></a>FTP: mget

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Copia los archivos remotos en el equipo local mediante el tipo de transferencia de archivos actual.   
## <a name="syntax"></a>Sintaxis  
```  
mget <remoteFile>[ ]  
```  
#### <a name="parameters"></a>Parámetros  

|  Parámetro   |                        Descripción                        |
|--------------|-----------------------------------------------------------|
| <remoteFile> | Especifica los archivos remotos que se van a copiar en el equipo local. |

## <a name="examples"></a>Ejemplos  
Copie los archivos remotos a **. exe** y **b. exe** en el equipo local con el tipo de transferencia de archivos actual.  
```  
mget a.exe b.exe  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [FTP: ASCII](ftp-ascii.md)  
-   [FTP: binario](ftp-binary.md)  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
