---
title: usuario de FTP
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0a77bfeb-27a9-4f2f-a3c4-2fef529fb569 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63281a0ffdd646d3652eb3a442a8edd9acec9cce
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375871"
---
# <a name="ftp-user"></a>FTP: usuario

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Especifica un usuario para el equipo remoto.   
## <a name="syntax"></a>Sintaxis  
```  
user <UserName> [<Password>] [<Account>]  
```  
### <a name="parameters"></a>Parámetros  

|  Parámetro   |                                                                      Descripción                                                                      |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <UserName>  |                                          Especifica un nombre de usuario con el que iniciar sesión en el equipo remoto.                                           |
| [<Password>] |               Especifica la contraseña del *nombre de usuario*. Si no se especifica una contraseña pero es necesaria, **FTP** solicita la contraseña.               |
| [<Account>]  | Especifica una cuenta con la que se iniciará sesión en el equipo remoto. Si no se especifica una *cuenta* pero es necesaria, **FTP** solicita la cuenta. |

## <a name="BKMK_Examples"></a>Example  
Especifique user1 con la contraseña Contraseña1.  
```  
user User1 Password1  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
