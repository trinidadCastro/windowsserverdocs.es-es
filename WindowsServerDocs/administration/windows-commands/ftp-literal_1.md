---
title: literal_1 FTP
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fb81aa2d-07fa-4e79-bf44-1fb5526fdf14 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fc4f8aff5a22da93330a12a75e5f368285366216
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725255"
---
# <a name="ftp-literal_1"></a>FTP: literal_1

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 envía argumentos textuales al servidor FTP remoto. Se devuelve un solo código de respuesta FTP.   

## <a name="syntax"></a>Sintaxis  
```  
literal <Argument> [ ]  
```  
#### <a name="parameters"></a>Parámetros  

| Parámetro  |                    Descripción                    |
|------------|---------------------------------------------------|
| <Argument> | Especifica el argumento que se va a enviar al servidor FTP. |

## <a name="remarks"></a>Observaciones  
El comando **literal** es idéntico al comando **Quote** .  
## <a name="examples"></a>Ejemplos  
Envíe un comando **Quit** al servidor FTP remoto.  
```  
literal quit  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [FTP: comillas](ftp-quote.md)  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
