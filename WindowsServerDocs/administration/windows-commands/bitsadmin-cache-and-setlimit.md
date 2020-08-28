---
title: bitsadmin cache y setlimit
description: Artículo de referencia de la memoria caché de bitsadmin y el comando setlimit, que establece el límite de tamaño de caché.
ms.topic: reference
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: edcd83ace72e301471b03ac0c1fc85439a1c4d94
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028793"
---
# <a name="bitsadmin-cache-and-setlimit"></a>bitsadmin cache y setlimit

Establece el límite de tamaño de la memoria caché.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /cache /setlimit percent
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| percent | Límite de caché definido como un porcentaje del espacio total del disco duro. |

## <a name="examples"></a>Ejemplos

Para establecer el límite de tamaño de la caché en 50%:

```
bitsadmin /cache /setlimit 50
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando caché de bitsadmin](bitsadmin-cache.md)
