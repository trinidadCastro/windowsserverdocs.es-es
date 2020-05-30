---
title: Telnet abierto
description: Tema de referencia de Telnet Open, que se conecta a un servidor Telnet.
ms.prod: windows-server
ms.technology: manage-windows-commands
s.topic: article
ms.assetid: e30ad68c-2366-4754-ac36-311a2392902a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0be8aca4cb061df55674a405eb1cf2a5e643cb4f
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222715"
---
# <a name="telnet-open"></a>Telnet: abierto

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Se conecta a un servidor Telnet.

## <a name="syntax"></a>Sintaxis
```
o[pen] <hostname> [<Port>]
```
#### <a name="parameters"></a>Parámetros

| Parámetro  |                                        Descripción                                         |
|------------|--------------------------------------------------------------------------------------------|
| <hostname> |                         Especifica el nombre del equipo o la dirección IP.                         |
|  [<Port>]  | Especifica el puerto TCP en el que escucha el servidor Telnet. El valor predeterminado es el puerto TCP 23. |

## <a name="examples"></a>Ejemplos
Conéctese a un servidor Telnet en telnet.microsoft.com.
```
o telnet.microsoft.com
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
