---
title: remotehelp_1 FTP
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef23adf3-ead4-44c8-ac1d-c8a6f4b2bf73 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc4affb3f04eadaa4e0005e5edce0f564156f64a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725134"
---
# <a name="ftp-remotehelp_1"></a>FTP: remotehelp_1

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra la ayuda de los comandos remotos.   
## <a name="syntax"></a>Sintaxis  
```  
remotehelp [<Command>]  
```  
#### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|[<Command>]|Especifica el nombre del comando sobre el que se desea obtener ayuda. Si no se especifica *Command* , **FTP** muestra una lista de todos los comandos remotos.|  
## <a name="remarks"></a>Observaciones  
Puede ejecutar comandos remotos mediante **comillas** o **literales**.  
## <a name="examples"></a>Ejemplos  
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
