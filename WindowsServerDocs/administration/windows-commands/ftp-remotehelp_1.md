---
title: remotehelp_1 FTP
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef23adf3-ead4-44c8-ac1d-c8a6f4b2bf73 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3c4a4ffec01fce5cde8b2aa9dd1fa0704f3a85ff
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843058"
---
# <a name="ftp-remotehelp_1"></a>FTP: remotehelp_1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra la ayuda de los comandos remotos.   
## <a name="syntax"></a>Sintaxis  
```  
remotehelp [<Command>]  
```  
#### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|[<Command>]|Especifica el nombre del comando sobre el que se desea obtener ayuda. Si no se especifica *Command* , **FTP** muestra una lista de todos los comandos remotos.|  
## <a name="remarks"></a>Comentarios  
Puede ejecutar comandos remotos mediante **comillas** o **literales**.  
## <a name="examples"></a><a name=BKMK_Examples></a>Example  
Mostrar una lista de comandos remotos.  
```  
remotehelp  
```  
Muestra la sintaxis del comando remoto **feat** .  
```  
remotehelp feat  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [FTP: comillas](ftp-quote.md)  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
