---
title: telnet open
description: Artículo de referencia para telnet Open, que se conecta a un servidor Telnet.
s.topic: article
ms.assetid: e30ad68c-2366-4754-ac36-311a2392902a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 11498b2d2f38b96725608e6e72d30d0d563fd88a
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87881707"
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
