---
title: bitsadmin cache y setexpirationtime
description: Artículo de referencia de la memoria caché de bitsadmin y el comando setexpirationtime, que establece la hora de expiración de la caché.
ms.topic: article
ms.assetid: 00ea6e4e-b707-4b31-88dd-b61a78565c8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 367088525366707797296d844f134dcf8a02def7
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894737"
---
# <a name="bitsadmin-cache-and-setexpirationtime"></a>bitsadmin cache y setexpirationtime

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Establece la hora de expiración de la caché.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /cache /setexpirationtime secs
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| segundos | El número de segundos hasta que expire la memoria caché. |

## <a name="examples"></a>Ejemplos

Para establecer que la memoria caché expire en 60 segundos:

```
bitsadmin /cache / setexpirationtime 60
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando caché de bitsadmin](bitsadmin-cache.md)
