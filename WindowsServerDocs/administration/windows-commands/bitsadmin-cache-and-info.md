---
title: bitsadmin cache e info
description: Artículo de referencia para el comando bitsadmin cache y info, que vuelca una entrada específica de la memoria caché.
ms.topic: reference
ms.assetid: 15975cbf-dba6-49ca-a725-d15ce1952de5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cce96d25b3968c1f975b6426ce8c25e7420f7bd3
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028893"
---
# <a name="bitsadmin-cache-and-info"></a>bitsadmin cache e info

Vuelca una entrada específica de la memoria caché.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /cache /info recordID [/verbose]
```

### <a name="parameters"></a>Parámetros

| Paramreter | Descripción |
| -------------- | -------------- |
| recordID | GUID asociado a la entrada de caché. |

## <a name="examples"></a>Ejemplos

Para volcar la entrada de caché con el valor recordID de {6511FB02-E195-40A2-B595-E8E2F8F47702}:

```
bitsadmin /cache /info {6511FB02-E195-40A2-B595-E8E2F8F47702}
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando caché de bitsadmin](bitsadmin-cache.md)
