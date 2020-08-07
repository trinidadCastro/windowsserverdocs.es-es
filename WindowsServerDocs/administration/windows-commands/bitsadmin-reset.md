---
title: bitsadmin reset
description: Artículo de referencia del comando bitsadmin RESET, que cancela todos los trabajos de la cola de transferencia propiedad del usuario actual.
ms.topic: article
ms.assetid: 0e4f9d1d-072c-493f-8d7a-f6d713c3ef29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b28ad601a40a9646ad64268aaca0320ea5165efd
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893340"
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
