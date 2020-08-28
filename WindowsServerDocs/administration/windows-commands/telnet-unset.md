---
title: telnet unset
description: Artículo de referencia para telnet unset, que desactiva las opciones establecidas anteriormente.
ms.topic: reference
ms.assetid: da9a0d99-1930-4858-93c7-0e9c3797ee09
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e6e15e3f4b5a74c77f4a184c6641d0c14a18662
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038303"
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
|el registro|Desactiva el registro.|
|ntlm|Desactiva la autenticación NTLM.|
|?|Muestra ayuda para este comando.|
## <a name="examples"></a>Ejemplos
Desactive el registro.
```
u logging
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
