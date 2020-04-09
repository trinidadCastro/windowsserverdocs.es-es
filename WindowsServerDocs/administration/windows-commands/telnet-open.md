---
title: Telnet abierto
description: Comando comandos de Windows para telnet abierto, que se conecta a un servidor Telnet.
ms.prod: windows-server
ms.technology: manage-windows-commands
s.topic: article
ms.assetid: e30ad68c-2366-4754-ac36-311a2392902a vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b100d2b53a340a083f22d4fd88c42363642d5da5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833338"
---
# <a name="telnet-open"></a>Telnet: abierto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se conecta a un servidor Telnet.    

## <a name="syntax"></a>Sintaxis  
```  
o[pen] <hostname> [<Port>]  
```  
#### <a name="parameters"></a>Parámetros  

| Parámetro  |                                        Descripción                                         |
|------------|--------------------------------------------------------------------------------------------|
| <hostname> |                         Especifica el nombre del equipo o la dirección IP.                         |
|  [<Port>]  | Especifica el puerto TCP en el que escucha el servidor Telnet. El valor predeterminado es el puerto TCP 23. |

## <a name="examples"></a><a name=BKMK_Examples></a>Example  
Conéctese a un servidor Telnet en telnet.microsoft.com.  
```  
o telnet.microsoft.com  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
