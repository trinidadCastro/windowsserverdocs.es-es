---
title: abrir Telnet
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 186664a75978f589a9a26047c72b9db74dd2dc4d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441120"
---
# <a name="telnet-open"></a>Telnet: abrir

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se conecta a un servidor telnet.    
## <a name="syntax"></a>Sintaxis  
```  
o[pen] <hostname> [<Port>]  
```  
### <a name="parameters"></a>Parámetros  

| Parámetro  |                                        Descripción                                         |
|------------|--------------------------------------------------------------------------------------------|
| <hostname> |                         Especifica el nombre de equipo o dirección IP.                         |
|  [<Port>]  | Especifica el puerto TCP que está escuchando el servidor telnet. El valor predeterminado es el puerto TCP 23. |

## <a name="BKMK_Examples"></a>Ejemplos  
Conectarse a un servidor telnet en telnet.microsoft.com.  
```  
o telnet.microsoft.com  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
