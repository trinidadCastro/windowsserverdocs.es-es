---
title: remotehelp_1 FTP
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef23adf3-ead4-44c8-ac1d-c8a6f4b2bf73 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bac6fbe4a55c3fed4caab4e30ba848ec9ea68e21
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376030"
---
# <a name="ftp-remotehelp_1"></a>FTP: remotehelp_1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra la ayuda de los comandos remotos.   
## <a name="syntax"></a>Sintaxis  
```  
remotehelp [<Command>]  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|[<Command>]|Especifica el nombre del comando sobre el que se desea obtener ayuda. Si no se especifica *Command* , **FTP** muestra una lista de todos los comandos remotos.|  
## <a name="remarks"></a>Comentarios  
Puede ejecutar comandos remotos mediante **comillas** o **literales**.  
## <a name="BKMK_Examples"></a>Example  
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
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
