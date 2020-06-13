---
title: nslookup server
description: Tema de referencia del comando Nslookup Server, que cambia el servidor predeterminado al dominio del sistema de nombres de dominio (DNS) especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1a153bb39e3c7c4114334e7fa16b0f287b8b7fe8
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721628"
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
