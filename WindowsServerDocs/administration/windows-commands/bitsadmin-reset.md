---
title: bitsadmin reset
description: Tema de comandos de Windows para el **restablecimiento de bitsadmin**, que cancela todos los trabajos de la cola de transferencia propiedad del usuario actual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e4f9d1d-072c-493f-8d7a-f6d713c3ef29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b73bd3b9c66b24330a0f9444836b9c8bd1730722
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123082"
---
# <a name="bitsadmin-reset"></a>bitsadmin reset

Cancela todos los trabajos de la cola de transferencia propiedad del usuario actual. > No se pueden restablecer los trabajos creados por el sistema local. En su lugar, debe ser un administrador y usar el programador de tareas para programar este comando como una tarea con las credenciales del sistema local.

> [!NOTE]
> En BITSAdmin 1,5 y versiones anteriores, si tiene privilegios de administrador, el modificador/RESET cancelará todos los trabajos de la cola. Además, la opción/AllUsers no se admite.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /reset [/allusers]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| /allusers | Opcional. Cancela todos los trabajos de la cola propiedad del usuario actual. Debe tener privilegios de administrador para usar este parámetro. |

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se cancelan todos los trabajos de la cola de transferencia del usuario actual.

```
C:\>bitsadmin /reset
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)