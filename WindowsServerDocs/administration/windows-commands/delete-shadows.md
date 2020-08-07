---
title: delete shadows
description: Artículo de referencia para el comando eliminar sombras, que elimina las instantáneas.
ms.topic: article
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 29d1679b2d05265aa1fb5a089fab9cf99f840cd9
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87891372"
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
| cantidad`<volume>` | Elimina todas las instantáneas del volumen especificado. |
| menos`<volume>` | Elimina la instantánea más antigua del volumen especificado. |
| conjunto`<setID>` | Elimina las instantáneas del conjunto de instantáneas del ID. especificado. Puede especificar un alias mediante el **%** símbolo si el alias existe en el entorno actual. |
| sesión`<shadowID>` | Elimina una instantánea del ID. especificado. Puede especificar un alias mediante el **%** símbolo si el alias existe en el entorno actual. |
| expuesto {'<drive> | <mountpoint>} |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [eliminar comando](delete.md)
