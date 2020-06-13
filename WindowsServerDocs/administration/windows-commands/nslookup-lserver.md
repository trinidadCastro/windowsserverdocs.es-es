---
title: nslookup lserver
description: Tema de referencia del comando Nslookup lserver, que cambia el servidor inicial al dominio del sistema de nombres de dominio (DNS) especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 868142f251d62ebc3efd7913aded8e22aa077bd3
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721608"
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
