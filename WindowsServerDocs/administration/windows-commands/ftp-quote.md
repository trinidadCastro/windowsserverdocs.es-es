---
title: comilla de FTP
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4500a1d3-c091-42c7-a909-f61df7f2e993 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1101dd6a5fa163df8d43d182e9d0dfe66e340b60
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725140"
---
# <a name="ftp-quote"></a>FTP: comillas

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Envía argumentos textuales al servidor FTP remoto. Se devuelve un solo código de respuesta FTP.   
## <a name="syntax"></a>Sintaxis  
```  
quote <Argument>[ ]  
```  
#### <a name="parameters"></a>Parámetros  

| Parámetro  |                    Descripción                    |
|------------|---------------------------------------------------|
| <Argument> | Especifica el argumento que se va a enviar al servidor FTP. |

## <a name="remarks"></a>Observaciones  
El comando **Quote** es idéntico al comando **literal** .  
## <a name="examples"></a>Ejemplos  
Envíe un comando **Quit** al servidor FTP remoto.  
```  
quote quit  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [FTP: literal_1](ftp-literal_1.md)  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
