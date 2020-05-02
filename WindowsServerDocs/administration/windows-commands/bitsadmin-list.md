---
title: bitsadmin list
description: Tema de referencia del comando de lista bitsadmin, que enumera los trabajos de transferencia que son propiedad del usuario actual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1416965e-e0e6-49cf-b1d4-b286d3cf8716
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 262589293a147cc1bae98da8fdca047c5f914094
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717431"
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
