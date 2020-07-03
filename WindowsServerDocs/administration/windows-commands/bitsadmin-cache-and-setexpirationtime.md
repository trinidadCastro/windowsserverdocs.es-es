---
title: bitsadmin cache y setexpirationtime
description: Artículo de referencia de la memoria caché de bitsadmin y el comando setexpirationtime, que establece la hora de expiración de la caché.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 00ea6e4e-b707-4b31-88dd-b61a78565c8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60372a74acc6ea5312afb5dafcb3ab7b3299f4d2
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923258"
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
