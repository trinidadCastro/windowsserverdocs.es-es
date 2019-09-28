---
title: eliminación de FTP
description: Temas de comandos de Windows para la eliminación FTP
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 067c45f3-e4e8-4450-b8b6-836994f6adfe vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1b566da14751921e2f38acd922ececbbc972daa6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376449"
---
# <a name="ftp-delete"></a>FTP: eliminar

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

elimina archivos en equipos remotos.   
## <a name="syntax"></a>Sintaxis  
```  
delete <remoteFile>  
```  
### <a name="parameters"></a>Parámetros  

|  Parámetro   |          Descripción          |
|--------------|-------------------------------|
| <remoteFile> | Especifica el archivo que se va a eliminar. |

## <a name="BKMK_Examples"></a>Example  
Elimine el archivo test. txt del equipo remoto.  
```  
delete test.txt  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
