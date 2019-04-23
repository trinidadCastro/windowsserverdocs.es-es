---
title: telnet send
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 32345b22395107f4a2c3d88894126d4e5e0875a1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842366"
---
# <a name="telnet-send"></a>telnet: send

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envía comandos de telnet en el servidor telnet.   
## <a name="syntax"></a>Sintaxis  
```  
sen[d] {ao | ayt | brk | esc | ip | synch | <string>} [?]  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|ao|Envía el comando de telnet anular salida.|  
|ayt|Envía el comando de telnet ¿estás ahí.|  
|interrupción|Envía la interrupción de comando de telnet.|  
|esc|Envía el carácter de escape de telnet actual.|  
|IP|Envía el comando de telnet interrumpir proceso.|  
|sincronización|Envía el comando synch.|  
|<string>|Cualquier cadena que haya escrito se envía al servidor telnet.|  
|?|Muestra la Ayuda asociado a este comando.|  
## <a name="BKMK_Examples"></a>Ejemplos  
Envío eres existe en el servidor telnet.  
```  
sen ayt  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  