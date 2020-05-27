---
title: no establecido Telnet
description: Tema de referencia para telnet unset, que desactiva las opciones establecidas anteriormente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da9a0d99-1930-4858-93c7-0e9c3797ee09 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c121d6501f87ae1218381871bcbf536d9407ba31
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821045"
---
# <a name="telnet-unset"></a>Telnet: no establecido

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

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
|logging|Desactiva el registro.|
|ntlm|Desactiva la autenticación NTLM.|
|?|Muestra ayuda para este comando.|
## <a name="examples"></a>Ejemplos
Desactive el registro.
```
u logging
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
