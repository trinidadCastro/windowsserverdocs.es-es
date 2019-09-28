---
title: cambiar nombre de FTP
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 977baa042a6b0d9c23db7cb398bee997c2049227
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376016"
---
# <a name="ftp-rename"></a>FTP: Rename

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia el nombre de los archivos remotos.   
## <a name="syntax"></a>Sintaxis  
```  
rename <FileName> <NewFileName>  
```  
### <a name="parameters"></a>Parámetros  

|   Parámetro   |                 Descripción                 |
|---------------|---------------------------------------------|
|  <FileName>   | Especifica el archivo cuyo nombre desea cambiar. |
| <NewFileName> |        Especifica el nuevo nombre de archivo.         |

## <a name="BKMK_Examples"></a>Example  
Cambie el nombre del archivo remoto **example. txt** a **ejemplo1. txt** .  
```  
rename example.txt example1.txt  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
