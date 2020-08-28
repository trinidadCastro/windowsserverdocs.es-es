---
title: nslookup lserver
description: Artículo de referencia del comando Nslookup lserver, que cambia el servidor inicial al dominio del sistema de nombres de dominio (DNS) especificado.
ms.topic: reference
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 99d3dbeac4073b35abd540c185e4cf69b723b61e
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89023479"
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
