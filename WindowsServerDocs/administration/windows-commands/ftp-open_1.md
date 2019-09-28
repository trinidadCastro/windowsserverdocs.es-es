---
title: open_1 FTP
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b61926a-dc60-4b4c-96d3-64e5c91c18ba vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c5da1c73362c0396300f712b2e45b906d1652604
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376186"
---
# <a name="ftp-open_1"></a>FTP: open_1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se conecta al servidor FTP especificado.   
## <a name="syntax"></a>Sintaxis  
```  
open <computer> [<Port>]  
```  
### <a name="parameters"></a>Parámetros  

| Parámetro  |                                           Descripción                                            |
|------------|--------------------------------------------------------------------------------------------------|
| <computer> |                Especifica el equipo remoto al que está intentando conectarse.                 |
|  [<Port>]  | Especifica el número de puerto TCP que se va a utilizar para conectarse a un servidor FTP. De forma predeterminada, se usa el puerto TCP 21. |

## <a name="remarks"></a>Comentarios  
Puede usar una dirección IP o un nombre de equipo (en cuyo caso debe estar disponible un servidor DNS o un archivo hosts) para especificar el **equipo**.  
## <a name="BKMK_Examples"></a>Example  
Conéctese al servidor FTP en **FTP.Microsoft.com**.  
```  
Open ftp.microsoft.com  
```  
Conéctese al servidor FTP en **FTP.Microsoft.com** que escucha en el puerto TCP 755.  
```  
open ftp.microsoft.com 755  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
