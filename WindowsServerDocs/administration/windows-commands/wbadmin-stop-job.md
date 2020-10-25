---
title: wbadmin stop job
description: Artículo de referencia del comando Wbadmin STOP Job, que cancela la operación de copia de seguridad o recuperación que se está ejecutando actualmente.
ms.topic: reference
ms.assetid: 3b83b398-39c7-4410-bf17-5c1fb1a4f46d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9952f28f674da6eae295dcffc0357cade19fdd1c
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524710"
---
# <a name="wbadmin-stop-job"></a>wbadmin stop job

Cancela la operación de copia de seguridad o de recuperación que se está ejecutando actualmente.

> [!IMPORTANT]
> Las operaciones canceladas no se pueden reiniciar. Debe ejecutar una copia de seguridad cancelada o una operación de recuperación desde el principio de nuevo.

Para detener una copia de seguridad o una operación de recuperación con este comando, debe ser miembro del grupo **operadores de copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados, haciendo clic con el botón secundario en **símbolo del sistema**y, a continuación, seleccionando **Ejecutar como administrador**.

## <a name="syntax"></a>Sintaxis

```
wbadmin stop job [-quiet]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| -quiet | Ejecuta el comando sin preguntar al usuario. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)
