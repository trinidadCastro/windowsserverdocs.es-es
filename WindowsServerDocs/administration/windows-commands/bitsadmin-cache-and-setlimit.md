---
title: bitsadmin cache y setlimit
description: Artículo de referencia de la memoria caché de bitsadmin y el comando setlimit, que establece el límite de tamaño de caché.
ms.topic: reference
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7547a2a51104285b10af6b02c1962c89f75d51fa
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632488"
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
