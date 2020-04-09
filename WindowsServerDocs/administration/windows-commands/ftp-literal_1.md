---
title: literal_1 FTP
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fb81aa2d-07fa-4e79-bf44-1fb5526fdf14 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f502bb56c94734870962f56cfb85dcc17ddc3f93
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843398"
---
# <a name="ftp-literal_1"></a>FTP: literal_1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 envía argumentos textuales al servidor FTP remoto. Se devuelve un solo código de respuesta FTP.   

## <a name="syntax"></a>Sintaxis  
```  
literal <Argument> [ ]  
```  
#### <a name="parameters"></a>Parámetros  

| Parámetro  |                    Descripción                    |
|------------|---------------------------------------------------|
| <Argument> | Especifica el argumento que se va a enviar al servidor FTP. |

## <a name="remarks"></a>Comentarios  
El comando **literal** es idéntico al comando **Quote** .  
## <a name="examples"></a><a name=BKMK_Examples></a>Example  
Envíe un comando **Quit** al servidor FTP remoto.  
```  
literal quit  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [FTP: comillas](ftp-quote.md)  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
