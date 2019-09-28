---
title: no establecido Telnet
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da9a0d99-1930-4858-93c7-0e9c3797ee09 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e6f0eb98c4168d2f664780dad42ca1aea5463d24
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383482"
---
# <a name="telnet-unset"></a>Telnet: no establecido

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Desactiva las opciones establecidas anteriormente.   
## <a name="syntax"></a>Sintaxis  
```  
u[nset] {bsasdel | crlf | delasbs | escape | localecho | logging | ntlm} [?]  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|bsasdel|Envía el **retroceso** como **retroceso**.|  
|CRLF|Envía la tecla **entrar** como CR. También se conoce como modo de avance de línea.|  
|delasbs|Envía **Delete** como **Delete**.|  
|Salida|quita el valor de carácter de escape.|  
|localecho|Desactiva localecho.|  
|logging|Desactiva el registro.|  
|NTLM|Desactiva la autenticación NTLM.|  
|?|Muestra ayuda para este comando.|  
## <a name="BKMK_Examples"></a>Example  
Desactive el registro.  
```  
u logging  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
