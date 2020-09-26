---
title: Schtasks (consulta)
description: Artículo de referencia del comando de consulta Schtasks, que enumera todas las tareas programadas para ejecutarse en el equipo.
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/16/2020
ms.openlocfilehash: c1bff666f762b21e8dbcf2bff59383d49cab6409
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91390803"
---
# <a name="schtasks-query"></a>Schtasks (consulta)

Muestra todas las tareas programadas para ejecutarse en el equipo.

Sintaxis

```
schtasks [/query] [/fo {TABLE | LIST | CSV}] [/nh] [/v] [/s <computer> [/u [<domain>\]<user> [/p <password>]]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /Query | Opcionalmente, especifica el nombre de la operación. El uso de esta consulta sin parámetros realiza una consulta. |
| /FO `<format>` | Especifica el formato de salida. Los valores válidos son *TABLE*, *List*o *CSV*. |
| /NH | Quita los encabezados de columna de la presentación de la tabla. Este parámetro es válido con los formatos de salida de *tabla* o *CSV* . |
| /v | Agrega las propiedades avanzadas de la tarea a la pantalla. Este parámetro es válido con los formatos de salida *List* o *CSV* . |
| modificado `<computer>` | Especifica el nombre o la dirección IP de un equipo remoto (con o sin barras diagonales inversas). La opción predeterminada es el equipo local. |
| /u `[<domain>]` | Ejecuta este comando con los permisos de la cuenta de usuario especificada. De forma predeterminada, el comando se ejecuta con los permisos del usuario actual del equipo local. La cuenta de usuario especificada debe ser miembro del grupo administradores en el equipo remoto. Los parámetros **/u** y **/p** solo son válidos cuando se utiliza **/s**. |
| /p `<password>` | Especifica la contraseña de la cuenta de usuario especificada en el parámetro **/u** . Si usa el parámetro **/u** sin el parámetro **/p** o el argumento password, SchTasks le solicitará una contraseña. Los parámetros **/u** y **/p** solo son válidos cuando se utiliza **/s**. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para enumerar todas las tareas programadas para el equipo local, escriba:

```
schtasks
schtasks /query
```

Estos comandos generan el mismo resultado y se pueden usar indistintamente.

Para solicitar una presentación detallada de las tareas en el equipo local, escriba:

```
schtasks /query /fo LIST /v
```

Este comando usa el parámetro **/v** para solicitar una presentación detallada (detallada) y el parámetro **/FO List** para dar formato a la presentación como una lista para facilitar la lectura. Puede usar este comando para comprobar que una tarea que ha creado tiene el patrón de periodicidad previsto.

Para solicitar una lista de tareas programadas para un equipo remoto y agregar las tareas a un archivo de registro separado por comas en el equipo local, escriba:

```
schtasks /query /s Reskit16 /fo csv /nh >> \\svr01\data\tasklogs\p0102.csv
```

Puede usar este formato de comando para recopilar y realizar un seguimiento de las tareas programadas para varios equipos. Este comando usa el parámetro **/s** para identificar el equipo remoto, *Reskit16*, el parámetro **/FO** para especificar el formato y el parámetro **/NH** para suprimir los encabezados de columna. El **>>** Símbolo Append redirige la salida al registro de tareas *p0102.csv*, en el equipo local, *Svr01*. Dado que el comando se ejecuta en el equipo remoto, la ruta de acceso del equipo local debe ser completa.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando de cambio de SchTasks](schtasks-change.md)

- [comando Schtasks Create](schtasks-create.md)

- [comando de eliminación de SchTasks](schtasks-delete.md)

- [comando de fin de SchTasks](schtasks-end.md)

- [comando ejecutar SchTasks](schtasks-run.md)