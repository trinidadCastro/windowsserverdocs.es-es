---
title: mdelete_1 FTP
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8a80a8f5-e880-40a8-abc9-29a41836844f vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b64fa691a9ac890aad0e8d8dc93bd73e53478fc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842728"
---
# <a name="ftp-mdelete_1"></a>FTP: mdelete_1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

elimina archivos del equipo remoto.   
## <a name="syntax"></a>Sintaxis  
```  
mdelete <remoteFile>[ ]  
```  
#### <a name="parameters"></a>Parámetros  

|  Parámetro   |             Descripción              |
|--------------|--------------------------------------|
| <remoteFile> | Especifica el archivo remoto que se va a eliminar. |

## <a name="examples"></a><a name=BKMK_Examples></a>Example  
Elimine los archivos remotos **a. exe** y **b. exe**.  
```  
mdelete a.exe b.exe  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
