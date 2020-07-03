---
title: bitsadmin cache y delete
description: Artículo de referencia de la caché de bitsadmin y el comando DELETE, que elimina una entrada de caché específica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 22540273-55a5-46ea-869b-6df2aa6808a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 739215722eac761aed45d6b4dba32b2b001450b3
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927042"
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
