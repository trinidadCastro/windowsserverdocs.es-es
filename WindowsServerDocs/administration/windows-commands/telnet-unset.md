---
title: no establecido Telnet
description: Windows Commands topic for Telnet unset, que desactiva las opciones establecidas anteriormente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da9a0d99-1930-4858-93c7-0e9c3797ee09 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 34d52eedb2a5547ad0e3f2912dbe2a250eaf8fc5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833148"
---
# <a name="telnet-unset"></a>Telnet: no establecido

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Desactiva las opciones establecidas anteriormente.   

## <a name="syntax"></a>Sintaxis  
```  
u[nset] {bsasdel | crlf | delasbs | escape | localecho | logging | ntlm} [?]  
```  
#### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|bsasdel|Envía el **retroceso** como **retroceso**.|  
|CRLF|Envía la tecla **entrar** como CR. También se conoce como modo de avance de línea.|  
|delasbs|Envía **Delete** como **Delete**.|  
|escape|quita el valor de carácter de escape.|  
|localecho|Desactiva localecho.|  
|registro|Desactiva el registro.|  
|ntlm|Desactiva la autenticación NTLM.|  
|?|Muestra ayuda para este comando.|  
## <a name="examples"></a><a name=BKMK_Examples></a>Example  
Desactive el registro.  
```  
u logging  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
