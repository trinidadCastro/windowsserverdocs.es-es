---
title: mdelete_1 FTP
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8a80a8f5-e880-40a8-abc9-29a41836844f vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ffbb06cff9e177316ea60e281c25d7640a7f981
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375922"
---
# <a name="ftp-mdelete_1"></a>FTP: mdelete_1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

elimina archivos del equipo remoto.   
## <a name="syntax"></a>Sintaxis  
```  
mdelete <remoteFile>[ ]  
```  
### <a name="parameters"></a>Parámetros  

|  Parámetro   |             Descripción              |
|--------------|--------------------------------------|
| <remoteFile> | Especifica el archivo remoto que se va a eliminar. |

## <a name="BKMK_Examples"></a>Example  
Elimine los archivos remotos **a. exe** y **b. exe**.  
```  
mdelete a.exe b.exe  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
