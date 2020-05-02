---
title: bitsadmin reset
description: Tema de referencia del comando bitsadmin RESET, que cancela todos los trabajos de la cola de transferencia propiedad del usuario actual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e4f9d1d-072c-493f-8d7a-f6d713c3ef29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a6aea1d3cb0a89def1e23f42272bf0503022ac54
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717004"
---
# <a name="bitsadmin-reset"></a>bitsadmin reset

Cancela todos los trabajos de la cola de transferencia propiedad del usuario actual. No se pueden restablecer los trabajos creados por el sistema local. En su lugar, debe ser un administrador y usar el programador de tareas para programar este comando como una tarea con las credenciales del sistema local.

> [!NOTE]
> Si tiene privilegios de administrador en BITSAdmin 1,5 y versiones anteriores, el modificador/RESET cancelará todos los trabajos de la cola. Además, la opción/AllUsers no se admite.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /reset [/allusers]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| /allusers | Opcional. Cancela todos los trabajos de la cola propiedad del usuario actual. Debe tener privilegios de administrador para usar este parámetro. |

## <a name="examples"></a>Ejemplos

Para cancelar todos los trabajos de la cola de transferencia del usuario actual.

```
bitsadmin /reset
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
