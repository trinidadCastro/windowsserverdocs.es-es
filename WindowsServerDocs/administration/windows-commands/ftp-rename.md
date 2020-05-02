---
title: cambiar nombre de FTP
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5dc5006c82df8417a8652a9c0ba20f7f1a002e7f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725115"
---
# <a name="ftp-rename"></a>FTP: Rename

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

cambia el nombre de los archivos remotos.   
## <a name="syntax"></a>Sintaxis  
```  
rename <FileName> <NewFileName>  
```  
#### <a name="parameters"></a>Parámetros  

|   Parámetro   |                 Descripción                 |
|---------------|---------------------------------------------|
|  <FileName>   | Especifica el archivo cuyo nombre desea cambiar. |
| <NewFileName> |        Especifica el nuevo nombre de archivo.         |

## <a name="examples"></a>Ejemplos  
Cambie el nombre del archivo remoto **example. txt** a **ejemplo1. txt** .  
```  
rename example.txt example1.txt  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
