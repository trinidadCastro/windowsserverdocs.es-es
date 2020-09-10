---
title: telnet unset
description: Artículo de referencia para telnet unset, que desactiva las opciones establecidas anteriormente.
ms.topic: reference
ms.assetid: da9a0d99-1930-4858-93c7-0e9c3797ee09
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 96b8126d758d5277129f88f193cfea4b25e80584
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640818"
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
