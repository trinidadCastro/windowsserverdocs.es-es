---
title: Telnet abierto
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e30ad68c-2366-4754-ac36-311a2392902a vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4528b728c89bbdfc99de94c7fefebb18c8e1ad97
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383648"
---
# <a name="telnet-open"></a>Telnet: abierto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se conecta a un servidor Telnet.    
## <a name="syntax"></a>Sintaxis  
```  
o[pen] <hostname> [<Port>]  
```  
### <a name="parameters"></a>Parámetros  

| Parámetro  |                                        Descripción                                         |
|------------|--------------------------------------------------------------------------------------------|
| <hostname> |                         Especifica el nombre del equipo o la dirección IP.                         |
|  [<Port>]  | Especifica el puerto TCP en el que escucha el servidor Telnet. El valor predeterminado es el puerto TCP 23. |

## <a name="BKMK_Examples"></a>Example  
Conéctese a un servidor Telnet en telnet.microsoft.com.  
```  
o telnet.microsoft.com  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
