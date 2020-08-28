---
title: nslookup server
description: Artículo de referencia del comando Nslookup Server, que cambia el servidor predeterminado al dominio del sistema de nombres de dominio (DNS) especificado.
ms.topic: reference
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32450197fe7d3c04258b7fb3f77f8e17cd1c113e
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038766"
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
