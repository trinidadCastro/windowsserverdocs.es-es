---
title: nslookup server
description: Artículo de referencia del comando Nslookup Server, que cambia el servidor predeterminado al dominio del sistema de nombres de dominio (DNS) especificado.
ms.topic: reference
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 64d1383dd5c4fa86a62fad91bae0ed5e4eb1454b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635620"
---
# <a name="nslookup-server"></a>nslookup server

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia el servidor predeterminado al dominio del sistema de nombres de dominio (DNS) especificado.

Este comando usa el servidor predeterminado actual para buscar la información sobre el dominio DSN especificado. Si desea buscar información mediante el servidor inicial, use el comando [nslookup lserver](nslookup-lserver.md) .

## <a name="syntax"></a>Sintaxis

```
server <DNSdomain>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<DNSdomain>` | Especifica el dominio DNS del servidor predeterminado. |
| /? | Muestra la ayuda en el símbolo del sistema. |
| /help | Muestra la ayuda en el símbolo del sistema. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [nslookup lserver](nslookup-lserver.md)
