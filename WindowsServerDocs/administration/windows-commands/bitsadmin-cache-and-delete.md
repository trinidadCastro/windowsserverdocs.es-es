---
title: bitsadmin cache y delete
description: Artículo de referencia de la caché de bitsadmin y el comando DELETE, que elimina una entrada de caché específica.
ms.topic: reference
ms.assetid: 22540273-55a5-46ea-869b-6df2aa6808a1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3dd83b05b0b4964fe8dbaa52a03a651153d9a94d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632641"
---
# <a name="bitsadmin-cache-and-delete"></a>bitsadmin cache y delete

Elimina una entrada de caché específica.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /cache /delete recordID
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| recordID | GUID asociado a la entrada de caché. |

## <a name="examples"></a>Ejemplos

Para eliminar la entrada de caché con el RecordID de {6511FB02-E195-40A2-B595-E8E2F8F47702}:

```
bitsadmin /cache /delete {6511FB02-E195-40A2-B595-E8E2F8F47702}
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando caché de bitsadmin](bitsadmin-cache.md)
