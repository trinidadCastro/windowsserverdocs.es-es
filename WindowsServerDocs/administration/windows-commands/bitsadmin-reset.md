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
ms.openlocfilehash: ed1dcf9bce06af527ffb5b6a79d76d860d78450c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849798"
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

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se cancelan todos los trabajos de la cola de transferencia del usuario actual.

```
C:\>bitsadmin /reset
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)