---
title: nslookup lserver
description: Artículo de referencia del comando Nslookup lserver, que cambia el servidor inicial al dominio del sistema de nombres de dominio (DNS) especificado.
ms.topic: reference
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d1f507b1a61e9fd4fc506fb4e74f869c83e95335
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635678"
---
# <a name="nslookup-lserver"></a>nslookup lserver

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia el servidor inicial al dominio del sistema de nombres de dominio (DNS) especificado.

Este comando usa el servidor inicial para buscar la información sobre el dominio DSN especificado. Si desea buscar información mediante el servidor predeterminado actual, use el comando [nslookup Server](nslookup-server.md) .

## <a name="syntax"></a>Sintaxis

```
lserver <DNSdomain>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<DNSdomain>` | Especifica el dominio DNS del servidor inicial. |
| /? | Muestra la ayuda en el símbolo del sistema. |
| /help | Muestra la ayuda en el símbolo del sistema. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [nslookup server](nslookup-server.md)
