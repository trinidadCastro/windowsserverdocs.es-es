---
title: bitsadmin cache y setexpirationtime
description: Artículo de referencia de la memoria caché de bitsadmin y el comando setexpirationtime, que establece la hora de expiración de la caché.
ms.topic: reference
ms.assetid: 00ea6e4e-b707-4b31-88dd-b61a78565c8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e07e862d8577c33daec24bbc93fe5859f13944db
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028813"
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
