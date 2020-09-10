---
title: bitsadmin cache y setexpirationtime
description: Artículo de referencia de la memoria caché de bitsadmin y el comando setexpirationtime, que establece la hora de expiración de la caché.
ms.topic: reference
ms.assetid: 00ea6e4e-b707-4b31-88dd-b61a78565c8d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0d9bc67292796afdbcec695ea2e65966b5b5e46d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632530"
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
