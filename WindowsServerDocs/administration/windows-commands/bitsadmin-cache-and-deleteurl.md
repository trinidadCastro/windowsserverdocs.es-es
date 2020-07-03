---
title: memoria caché de bitsadmin y deleteURL
description: Artículo de referencia de la caché de bitsadmin y el comando deleteURL, que elimina todas las entradas de la memoria caché de la dirección URL especificada.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e108b76b-fae9-4c16-bf4c-d74c9f025953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8d1ed4710bfeeefa721308c54075ddc8da5c5216
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923332"
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
