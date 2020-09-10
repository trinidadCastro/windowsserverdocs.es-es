---
title: delete shadows
description: Artículo de referencia para el comando eliminar sombras, que elimina las instantáneas.
ms.topic: reference
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f8fe95ac21c36f4605a544c97036c0a02bddaf42
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89628850"
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
