---
title: envío de Telnet
description: Temas de comandos de Windows para el envío de Telnet, que envía comandos Telnet al servidor Telnet.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c217abc-1182-466e-914c-1ff16755021b vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6fef48ca04a3817f58d063bc8b23f5c11c4ea197
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833288"
---
# <a name="telnet-send"></a>Telnet: enviar

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envía comandos Telnet al servidor Telnet.   

## <a name="syntax"></a>Sintaxis  
```  
sen[d] {ao | ayt | brk | esc | ip | synch | <string>} [?]  
```  
#### <a name="parameters"></a>Parámetros  

| Parámetro |                     Descripción                      |
|-----------|------------------------------------------------------|
|    AO     |       Envía la salida de anulación del comando telnet.        |
|    ayt    |       Envía el comando telnet.       |
|    brk    |            Envía el comando telnet BRK.            |
|    ESC    |      Envía el carácter de escape de Telnet actual.      |
|    intelectual     |     Envía el proceso de interrupción del comando de Telnet.     |
|   sincronización   |           Envía la sincronización de comandos de Telnet.           |
| <string>  | Envía cualquier cadena que escriba en el servidor Telnet. |
|     ?     |     Muestra la ayuda asociada a este comando.      |

## <a name="examples"></a><a name=BKMK_Examples></a>Example  
Envíelo al servidor Telnet.  
```  
sen ayt  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
