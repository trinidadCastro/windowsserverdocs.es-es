---
title: telnet open
description: Artículo de referencia para telnet Open, que se conecta a un servidor Telnet.
s.topic: article
ms.assetid: e30ad68c-2366-4754-ac36-311a2392902a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 892d797c4b56acb46e8119237fd38296e4ae411c
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636782"
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
