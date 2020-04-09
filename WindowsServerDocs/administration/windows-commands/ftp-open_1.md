---
title: open_1 FTP
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4b61926a-dc60-4b4c-96d3-64e5c91c18ba vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8bd3063a52908d65f336afcda6b6982d5bc9bf94
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843188"
---
# <a name="ftp-open_1"></a>FTP: open_1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se conecta al servidor FTP especificado.   
## <a name="syntax"></a>Sintaxis  
```  
open <computer> [<Port>]  
```  
#### <a name="parameters"></a>Parámetros  

| Parámetro  |                                           Descripción                                            |
|------------|--------------------------------------------------------------------------------------------------|
| <computer> |                Especifica el equipo remoto al que está intentando conectarse.                 |
|  [<Port>]  | Especifica el número de puerto TCP que se va a utilizar para conectarse a un servidor FTP. De forma predeterminada, se usa el puerto TCP 21. |

## <a name="remarks"></a>Comentarios  
Puede usar una dirección IP o un nombre de equipo (en cuyo caso debe estar disponible un servidor DNS o un archivo hosts) para especificar el **equipo**.  
## <a name="examples"></a><a name=BKMK_Examples></a>Example  
Conéctese al servidor FTP en **FTP.Microsoft.com**.  
```  
Open ftp.microsoft.com  
```  
Conéctese al servidor FTP en **FTP.Microsoft.com** que escucha en el puerto TCP 755.  
```  
open ftp.microsoft.com 755  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
