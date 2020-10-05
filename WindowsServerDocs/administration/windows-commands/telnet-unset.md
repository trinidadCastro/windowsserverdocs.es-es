---
title: telnet unset
description: Artículo de referencia para el comando telnet unset, que desactiva las opciones establecidas anteriormente.
ms.topic: reference
ms.assetid: da9a0d99-1930-4858-93c7-0e9c3797ee09
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 51604615e989325b2e5719d02c51f70dfd84bd0c
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91717922"
---
# <a name="telnet-unset"></a>Telnet: no establecido

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Desactiva las opciones establecidas anteriormente.

## <a name="syntax"></a>Sintaxis

```
u {bsasdel | crlf | delasbs | escape | localecho | logging | ntlm} [?]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| bsasdel | Envía el **retroceso** como **retroceso**. |
| CRLF | Envía la tecla **entrar** como CR. También se conoce como modo de avance de línea. |
| delasbs | Envía **Delete** como **Delete**. |
| escape | Quita el valor de carácter de escape. |
| localecho | Desactiva localecho. |
| logging | Desactiva el registro. |
| ntlm | Desactiva la autenticación NTLM. |
| ? | Muestra ayuda para este comando. |

## <a name="example"></a>Ejemplo

Desactive el registro.

```
u logging
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
