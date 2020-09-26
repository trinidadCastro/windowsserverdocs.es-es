---
title: fin de SchTasks
description: Artículo de referencia del comando Schtasks end, que solo detiene las instancias de un programa iniciadas por una tarea programada.
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/16/2020
ms.openlocfilehash: 7075c8689a39c7bc9eba917298678977916e29fa
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91390806"
---
# <a name="schtasks-end"></a>fin de SchTasks

Detiene solo las instancias de un programa iniciadas por una tarea programada. Para detener otros procesos, debe usar el comando [Taskkill](taskkill.md) .

## <a name="syntax"></a>Sintaxis

```
schtasks /end /tn <taskname> [/s <computer> [/u [<domain>\]<user> [/p <password>]]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /TN `<taskname>` | Identifica la tarea que inició el programa. Este parámetro es necesario. |
| modificado `<computer>` | Especifica el nombre o la dirección IP de un equipo remoto (con o sin barras diagonales inversas). La opción predeterminada es el equipo local. |
| /u `[<domain>]` | Ejecuta este comando con los permisos de la cuenta de usuario especificada. De forma predeterminada, el comando se ejecuta con los permisos del usuario actual del equipo local. La cuenta de usuario especificada debe ser miembro del grupo administradores en el equipo remoto. Los parámetros **/u** y **/p** solo son válidos cuando se utiliza **/s**. |
| /p `<password>` | Especifica la contraseña de la cuenta de usuario especificada en el parámetro **/u** . Si usa el parámetro **/u** sin el parámetro **/p** o el argumento password, SchTasks le solicitará una contraseña. Los parámetros **/u** y **/p** solo son válidos cuando se utiliza **/s**. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para detener la instancia de Notepad.exe iniciada por la tarea *mi Bloc de notas* , escriba:

```
schtasks /end /tn "My Notepad"
```

Para detener la instancia de Internet Explorer iniciada por la tarea de *Internet* en el equipo remoto, *Svr01*, escriba:

```
schtasks /end /tn InternetOn /s Svr01
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando de cambio de SchTasks](schtasks-change.md)

- [comando Schtasks Create](schtasks-create.md)

- [comando de eliminación de SchTasks](schtasks-delete.md)

- [comando de consulta SchTasks](schtasks-query.md)

- [comando ejecutar SchTasks](schtasks-run.md)