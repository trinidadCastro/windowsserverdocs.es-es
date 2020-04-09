---
title: cambiar nombre de FTP
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbe159f2833ce52921b46e46881a1d7aed8c5df8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843028"
---
# <a name="ftp-rename"></a>FTP: Rename

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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

## <a name="examples"></a><a name=BKMK_Examples></a>Example  
Cambie el nombre del archivo remoto **example. txt** a **ejemplo1. txt** .  
```  
rename example.txt example1.txt  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
