---
title: comandos SchTasks
description: Artículo de referencia para los comandos Schtasks, que programa comandos y programas para que se ejecuten periódicamente o en un momento determinado, agrega y quita tareas de la programación, inicia y detiene tareas a petición, y muestra y cambia las tareas programadas.
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/16/2020
ms.openlocfilehash: 50b0bd7f7a55a7aa5889c39e4bf9f4e582af7f3b
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388641"
---
# <a name="schtasks-commands"></a>comandos SchTasks

Programa comandos y programas para que se ejecuten periódicamente o en un momento determinado, agrega y quita tareas de la programación, inicia y detiene tareas a petición, y muestra y cambia las tareas programadas.

> [!NOTE]
> La herramienta **schtasks.exe** realiza las mismas operaciones que **las tareas programadas** en el **Panel de control**. Puede usar estas herramientas juntas e indistintamente.

## <a name="required-permissions"></a>Permisos necesarios

- Para programar, ver y cambiar todas las tareas en el equipo local, debe ser miembro del grupo administradores.

- Para programar, ver y cambiar todas las tareas en el equipo remoto, debe ser miembro del grupo administradores en el equipo remoto o debe usar el parámetro **/u** para proporcionar las credenciales de un administrador del equipo remoto.

- Puede usar el parámetro **/u** en una operación **/Create** o **/Change** si los equipos locales y remotos están en el mismo dominio, o si el equipo local se encuentra en un dominio en el que confía el dominio del equipo remoto. De lo contrario, el equipo remoto no puede autenticar la cuenta de usuario especificada y no puede comprobar que la cuenta es miembro del grupo administradores.

- La tarea que tiene previsto ejecutar debe tener el permiso adecuado. Estos permisos varían según la tarea. De forma predeterminada, las tareas se ejecutan con los permisos del usuario actual del equipo local o con los permisos del usuario especificado mediante el parámetro **/u** , si se incluye alguno. o ejecutar una tarea con permisos de una cuenta de usuario diferente o con permisos del sistema, use el parámetro **/RU** .

## <a name="syntax"></a>Sintaxis

```
schtasks change
schtasks create
schtasks delete
schtasks end
schtasks query
schtasks run
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| [cambio de SchTasks](schtasks-change.md) | Cambia una o varias de las siguientes propiedades de una tarea:<ul><li>El programa que ejecuta la tarea (/TR)</li><li>La cuenta de usuario en la que se ejecuta la tarea (/RU)</li><li>La contraseña de la cuenta de usuario (/RP)</li><li>Agrega la propiedad solo Interactive a la tarea (/it)</li></ul> |
| [crear SchTasks](schtasks-create.md) | Programa una nueva tarea.|
| [eliminar SchTasks](schtasks-delete.md) | Elimina una tarea programada. |
| [fin de SchTasks](schtasks-end.md) | Detiene un programa iniciado por una tarea. |
| [Schtasks (consulta)](schtasks-query.md) | Muestra las tareas programadas para ejecutarse en el equipo. |
| [ejecución de SchTasks](schtasks-run.md) | Inicia inmediatamente una tarea programada. La operación de **ejecución** omite la programación, pero utiliza la ubicación del archivo de programa, la cuenta de usuario y la contraseña guardadas en la tarea para ejecutar la tarea inmediatamente. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
