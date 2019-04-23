---
title: telnet unset
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 37c4d84d1664fdc13ea7ffec60bf981b264dba00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853896"
---
# <a name="telnet-unset"></a>telnet: unset

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Desactiva previamente un conjunto de opciones.   
## <a name="syntax"></a>Sintaxis  
```  
u[nset] {bsasdel | crlf | delasbs | escape | localecho | logging | ntlm} [?]  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|bsasdel|Envía **retroceso** como un **retroceso**.|  
|crlf|Envía el **ENTRAR** clave como un CR. También conocido como modo de avance de línea.|  
|delasbs|Envía **eliminar** como **eliminar**.|  
|escape|Quita el valor de carácter de escape.|  
|localecho|Desactiva localecho.|  
|logging|Desactiva el registro.|  
|ntlm|Desactiva la autenticación NTLM.|  
|?|Muestra ayuda para este comando.|  
## <a name="BKMK_Examples"></a>Ejemplos  
Desactivar el registro.  
```  
u logging  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
