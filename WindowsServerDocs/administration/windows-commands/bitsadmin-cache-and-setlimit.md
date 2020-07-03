---
title: bitsadmin cache y setlimit
description: Artículo de referencia de la memoria caché de bitsadmin y el comando setlimit, que establece el límite de tamaño de caché.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de218990d9176336e779b551bfacc0897df5d114
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923211"
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
