---
title: envío de Telnet
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c217abc-1182-466e-914c-1ff16755021b vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 958e0e507e5a0ae836da98de8d677a116dbb38bd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383632"
---
# <a name="telnet-send"></a>Telnet: enviar

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envía comandos Telnet al servidor Telnet.   
## <a name="syntax"></a>Sintaxis  
```  
sen[d] {ao | ayt | brk | esc | ip | synch | <string>} [?]  
```  
### <a name="parameters"></a>Parámetros  

| Parámetro |                     Descripción                      |
|-----------|------------------------------------------------------|
|    AO     |       Envía la salida de anulación del comando telnet.        |
|    ayt    |       Envía el comando telnet.       |
|    brk    |            Envía el comando telnet BRK.            |
|    ESC    |      Envía el carácter de escape de Telnet actual.      |
|    intelectual     |     Envía el proceso de interrupción del comando de Telnet.     |
|   sincronía   |           Envía la sincronización de comandos de Telnet.           |
| <string>  | Envía cualquier cadena que escriba en el servidor Telnet. |
|     ?     |     Muestra la ayuda asociada a este comando.      |

## <a name="BKMK_Examples"></a>Example  
Envíelo al servidor Telnet.  
```  
sen ayt  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
