---
title: literal_1 FTP
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fb81aa2d-07fa-4e79-bf44-1fb5526fdf14 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 393dea27e8567a72a5bd25c927282ade93e317c9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376274"
---
# <a name="ftp-literal_1"></a>FTP: literal_1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 envían argumentos textuales al servidor FTP remoto. Se devuelve un solo código de respuesta FTP.   

## <a name="syntax"></a>Sintaxis  
```  
literal <Argument> [ ]  
```  
### <a name="parameters"></a>Parámetros  

| Parámetro  |                    Descripción                    |
|------------|---------------------------------------------------|
| <Argument> | Especifica el argumento que se va a enviar al servidor FTP. |

## <a name="remarks"></a>Comentarios  
El comando **literal** es idéntico al comando **Quote** .  
## <a name="BKMK_Examples"></a>Example  
Envíe un comando **Quit** al servidor FTP remoto.  
```  
literal quit  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [FTP: comillas](ftp-quote.md)  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
