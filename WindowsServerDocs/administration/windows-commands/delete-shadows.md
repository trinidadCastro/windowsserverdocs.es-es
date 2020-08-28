---
title: delete shadows
description: Artículo de referencia para el comando eliminar sombras, que elimina las instantáneas.
ms.topic: reference
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d2613fc978db8c8e5b323df142b204a7270f6bad
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024209"
---
# <a name="delete-shadows"></a>delete shadows

Elimina las instantáneas.

## <a name="syntax"></a>Sintaxis

```
delete shadows [all | volume <volume> | oldest <volume> | set <setID> | id <shadowID> | exposed {<drive> | <mountpoint>}]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ---- | ---- |
| todo | Elimina todas las instantáneas. |
| cantidad `<volume>` | Elimina todas las instantáneas del volumen especificado. |
| menos `<volume>` | Elimina la instantánea más antigua del volumen especificado. |
| conjunto `<setID>` | Elimina las instantáneas del conjunto de instantáneas del ID. especificado. Puede especificar un alias mediante el **%** símbolo si el alias existe en el entorno actual. |
| sesión `<shadowID>` | Elimina una instantánea del ID. especificado. Puede especificar un alias mediante el **%** símbolo si el alias existe en el entorno actual. |
| expuesto {'<drive> | <mountpoint>} |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [eliminar comando](delete.md)
