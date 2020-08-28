---
title: bitsadmin list
description: Artículo de referencia del comando de lista bitsadmin, que enumera los trabajos de transferencia que son propiedad del usuario actual.
ms.topic: reference
ms.assetid: 1416965e-e0e6-49cf-b1d4-b286d3cf8716
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e1d36a9236c834cea76e653b9a2e639c3b8f964e
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024379"
---
# <a name="bitsadmin-list"></a>bitsadmin list

Enumera los trabajos de transferencia que son propiedad del usuario actual.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /list [/allusers][/verbose]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| /allusers | Opcional. Enumera los trabajos de todos los usuarios. Debe tener privilegios de administrador para usar este parámetro. |
| /verbose | Opcional. Proporciona información detallada acerca de cada trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar información acerca de los trabajos que pertenecen al usuario actual.

```
bitsadmin /list
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
