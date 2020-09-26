---
title: eliminar SchTasks
description: Artículo de referencia para el comando Schtasks Delete, que elimina una tarea programada de la programación.
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/16/2020
ms.openlocfilehash: a5b090ac9520aeae5e603e0c47c0c225b8bd0eff
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91390814"
---
# <a name="schtasks-delete"></a>eliminar SchTasks

Elimina una tarea programada de la programación. Este comando no elimina el programa que ejecuta la tarea o interrumpe un programa en ejecución.

## <a name="syntax"></a>Sintaxis

```
schtasks /delete /tn {<taskname> | *} [/f] [/s <computer> [/u [<domain>\]<user> [/p <password>]]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /TN `{<taskname> | *}` | Identifica la tarea que se va a eliminar. Si usa `*` , este comando elimina todas las tareas programadas para el equipo, no solo las tareas programadas por el usuario actual. |
| /f | Suprime el mensaje de confirmación. La tarea se elimina sin previo aviso. |
| modificado `<computer>` | Especifica el nombre o la dirección IP de un equipo remoto (con o sin barras diagonales inversas). La opción predeterminada es el equipo local. |
| /u `[<domain>]` | Ejecuta este comando con los permisos de la cuenta de usuario especificada. De forma predeterminada, el comando se ejecuta con los permisos del usuario actual del equipo local. La cuenta de usuario especificada debe ser miembro del grupo administradores en el equipo remoto. Los parámetros **/u** y **/p** solo son válidos cuando se utiliza **/s**. |
| /p `<password>` | Especifica la contraseña de la cuenta de usuario especificada en el parámetro **/u** . Si usa el parámetro **/u** sin el parámetro **/p** o el argumento password, SchTasks le solicitará una contraseña. Los parámetros **/u** y **/p** solo son válidos cuando se utiliza **/s**. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para eliminar la tarea *iniciar correo* de la programación de un equipo remoto.

```
schtasks /delete /tn Start Mail /s Svr16
```

Este comando usa el parámetro **/s** para identificar el equipo remoto.

Para eliminar todas las tareas de la programación del equipo local, incluidas las tareas programadas por otros usuarios.

```
schtasks /delete /tn * /f
```

Este comando usa el parámetro **/tn &#42;** para representar todas las tareas del equipo y el parámetro **/f** para suprimir el mensaje de confirmación.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando de cambio de SchTasks](schtasks-change.md)

- [comando Schtasks Create](schtasks-create.md)

- [comando de fin de SchTasks](schtasks-end.md)

- [comando de consulta SchTasks](schtasks-query.md)

- [comando ejecutar SchTasks](schtasks-run.md)
