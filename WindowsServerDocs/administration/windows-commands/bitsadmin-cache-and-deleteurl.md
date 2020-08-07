---
title: memoria caché de bitsadmin y deleteURL
description: Artículo de referencia de la caché de bitsadmin y el comando deleteURL, que elimina todas las entradas de la memoria caché de la dirección URL especificada.
ms.topic: article
ms.assetid: e108b76b-fae9-4c16-bf4c-d74c9f025953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d1a21a1994711e2548e9e08094f88f46edafe481
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894839"
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
| Dirección URL | Localizador uniforme de recursos que identifica un archivo remoto. |

## <a name="examples"></a>Ejemplos

Para eliminar todas las entradas de la memoria caché para `https://www.contoso.com/en/us/default.aspx` :

```
bitsadmin /deleteURL https://www.contoso.com/en/us/default.aspx
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando caché de bitsadmin](bitsadmin-cache.md)
