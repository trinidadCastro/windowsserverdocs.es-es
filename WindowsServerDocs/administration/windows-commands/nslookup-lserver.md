---
title: nslookup lserver
description: Artículo de referencia del comando Nslookup lserver, que cambia el servidor inicial al dominio del sistema de nombres de dominio (DNS) especificado.
ms.topic: article
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab7c478b6c9f3ae299559a556629e53f9eb852ee
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87885795"
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
