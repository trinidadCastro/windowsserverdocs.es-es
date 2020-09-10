---
title: memoria caché de bitsadmin y deleteURL
description: Artículo de referencia de la caché de bitsadmin y el comando deleteURL, que elimina todas las entradas de la memoria caché de la dirección URL especificada.
ms.topic: reference
ms.assetid: e108b76b-fae9-4c16-bf4c-d74c9f025953
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6e1c3af4cfbb6c1ef21a86ead6d3975bf1f44cf6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632625"
---
# <a name="bitsadmin-cache-and-deleteurl"></a>memoria caché de bitsadmin y deleteURL

Elimina todas las entradas de la memoria caché de la dirección URL especificada.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /deleteURL URL
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| URL | Localizador uniforme de recursos que identifica un archivo remoto. |

## <a name="examples"></a>Ejemplos

Para eliminar todas las entradas de la memoria caché para `https://www.contoso.com/en/us/default.aspx` :

```
bitsadmin /deleteURL https://www.contoso.com/en/us/default.aspx
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando caché de bitsadmin](bitsadmin-cache.md)
