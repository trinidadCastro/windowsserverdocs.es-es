---
title: ejecución de SchTasks
description: Artículo de referencia para el comando de ejecución de Schtasks, que
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/16/2020
ms.openlocfilehash: 9304e41c21899ce49bd6d4d4409ae3b47d3746aa
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91390802"
---
# <a name="schtasks-run"></a>ejecución de SchTasks

Inicia inmediatamente una tarea programada. La operación de ejecución omite la programación, pero utiliza la ubicación del archivo de programa, la cuenta de usuario y la contraseña guardadas en la tarea para ejecutar la tarea inmediatamente. La ejecución de una tarea no afecta a la programación de tareas y no cambia la siguiente hora de ejecución programada para la tarea.

## <a name="syntax"></a>Sintaxis

```
schtasks /run /tn <taskname> [/s <computer> [/u [<domain>\]<user> [/p <password>]]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /TN `<taskname>` | Identifica la tarea que se va a iniciar. Este parámetro es necesario. |
| modificado `<computer>` | Especifica el nombre o la dirección IP de un equipo remoto (con o sin barras diagonales inversas). La opción predeterminada es el equipo local. |
| /u `[<domain>]` | Ejecuta este comando con los permisos de la cuenta de usuario especificada. De forma predeterminada, el comando se ejecuta con los permisos del usuario actual del equipo local. La cuenta de usuario especificada debe ser miembro del grupo administradores en el equipo remoto. Los parámetros **/u** y **/p** solo son válidos cuando se utiliza **/s**. |
| /p `<password>` | Especifica la contraseña de la cuenta de usuario especificada en el parámetro **/u** . Si usa el parámetro **/u** sin el parámetro **/p** o el argumento password, SchTasks le solicitará una contraseña. Los parámetros **/u** y **/p** solo son válidos cuando se utiliza **/s**. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Use esta operación para probar sus tareas. Si una tarea no se ejecuta, compruebe si hay errores en el registro de transacciones del servicio de Programador de tareas `<Systemroot>\SchedLgU.txt` .

- Para ejecutar una tarea de forma remota, se debe programar la tarea en el equipo remoto. Al ejecutar la tarea, solo se ejecuta en el equipo remoto. Para comprobar que una tarea se ejecuta en un equipo remoto, use el administrador de tareas o el registro de transacciones del servicio de Programador de tareas, `<Systemroot>\SchedLgU.txt` .

## <a name="examples"></a>Ejemplos

Para iniciar la tarea *script de seguridad* , escriba:

```
schtasks /run /tn Security Script
```

Para iniciar la tarea de *actualización* en un equipo remoto, Svr01, escriba:

```
schtasks /run /tn Update /s Svr01
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando de cambio de SchTasks](schtasks-change.md)

- [comando Schtasks Create](schtasks-create.md)

- [comando de eliminación de SchTasks](schtasks-delete.md)

- [comando de fin de SchTasks](schtasks-end.md)

- [comando de consulta SchTasks](schtasks-query.md)