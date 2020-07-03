---
title: bitsadmin list
description: Artículo de referencia del comando de lista bitsadmin, que enumera los trabajos de transferencia que son propiedad del usuario actual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1416965e-e0e6-49cf-b1d4-b286d3cf8716
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9d9a2c86536ff0910b4e0a8bea15ec43d9371087
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926560"
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
