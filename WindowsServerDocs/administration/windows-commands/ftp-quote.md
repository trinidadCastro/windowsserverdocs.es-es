---
title: comilla de FTP
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4500a1d3-c091-42c7-a909-f61df7f2e993 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65660cf7311713295dae8a94c9174229f5ee44be
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376078"
---
# <a name="ftp-quote"></a>FTP: comillas

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envía argumentos textuales al servidor FTP remoto. Se devuelve un solo código de respuesta FTP.   
## <a name="syntax"></a>Sintaxis  
```  
quote <Argument>[ ]  
```  
### <a name="parameters"></a>Parámetros  

| Parámetro  |                    Descripción                    |
|------------|---------------------------------------------------|
| <Argument> | Especifica el argumento que se va a enviar al servidor FTP. |

## <a name="remarks"></a>Comentarios  
El comando **Quote** es idéntico al comando **literal** .  
## <a name="BKMK_Examples"></a>Example  
Envíe un comando **Quit** al servidor FTP remoto.  
```  
quote quit  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [FTP: literal_1](ftp-literal_1.md)  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
