---
title: usuario de FTP
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ef3b943491a90078dab453aaf3a037bd4ccf1825
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887496"
---
# <a name="ftp-user"></a>FTP: usuario

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Especifica un usuario en el equipo remoto.   
## <a name="syntax"></a>Sintaxis  
```  
user <UserName> [<Password>] [<Account>]  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|<UserName>|Especifica un nombre de usuario con la que se va a iniciar sesión en el equipo remoto.|  
|[<Password>]|Especifica la contraseña de *UserName*. Si no se especifica una contraseña, pero es necesaria, **ftp** pide la contraseña.|  
|[<Account>]|Especifica una cuenta con la que se va a iniciar sesión en el equipo remoto. Si un *cuenta* no se especifica, pero es necesario, **ftp** solicitará la cuenta.|  
## <a name="BKMK_Examples"></a>Ejemplos  
Especifique User1 con la contraseña Password1.  
```  
user User1 Password1  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
