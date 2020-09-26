---
title: cambio de SchTasks
description: Artículo de referencia para el comando de cambio de Schtasks, que programa comandos y programas para que se ejecuten periódicamente o en un momento determinado, agrega y quita tareas de la programación, inicia y detiene tareas a petición, y muestra y cambia las tareas programadas.
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/16/2020
ms.openlocfilehash: 06cbceaaa0e428743a725127d9495759dd0097e1
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91390815"
---
# <a name="schtasks-change"></a>cambio de SchTasks

Cambia una o varias de las siguientes propiedades de una tarea:

- El programa que ejecuta la tarea (**/TR**)

- La cuenta de usuario en la que se ejecuta la tarea (**/RU**)

- La contraseña de la cuenta de usuario (**/RP**)

- Agrega la propiedad solo Interactive a la tarea (**/it**)

## <a name="required-permissions"></a>Permisos necesarios

- Para programar, ver y cambiar todas las tareas en el equipo local, debe ser miembro del grupo administradores.

- Para programar, ver y cambiar todas las tareas en el equipo remoto, debe ser miembro del grupo administradores en el equipo remoto o debe usar el parámetro **/u** para proporcionar las credenciales de un administrador del equipo remoto.

- Puede usar el parámetro **/u** en una operación **/Create** o **/Change** si los equipos locales y remotos están en el mismo dominio, o si el equipo local se encuentra en un dominio en el que confía el dominio del equipo remoto. De lo contrario, el equipo remoto no puede autenticar la cuenta de usuario especificada y no puede comprobar que la cuenta es miembro del grupo administradores.

- La tarea que tiene previsto ejecutar debe tener el permiso adecuado. Estos permisos varían según la tarea. De forma predeterminada, las tareas se ejecutan con los permisos del usuario actual del equipo local o con los permisos del usuario especificado mediante el parámetro **/u** , si se incluye alguno. o ejecutar una tarea con permisos de una cuenta de usuario diferente o con permisos del sistema, use el parámetro **/RU** .

## <a name="syntax"></a>Sintaxis

```
schtasks /change /tn <Taskname> [/s <computer> [/u [<domain>\]<user> [/p <password>]]] [/ru <username>] [/rp <password>] [/tr <Taskrun>] [/st <Starttime>] [/ri <interval>] [{/et <Endtime> | /du <duration>} [/k]] [/sd <Startdate>] [/ed <Enddate>] [/{ENABLE | DISABLE}] [/it] [/z]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /TN `<Taskname>` | Identifica la tarea que se va a cambiar. Escriba el nombre de la tarea. |
| modificado `<computer>` | Especifica el nombre o la dirección IP de un equipo remoto (con o sin barras diagonales inversas). La opción predeterminada es el equipo local. |
| /u `[<domain>]` | Ejecuta este comando con los permisos de la cuenta de usuario especificada. De forma predeterminada, el comando se ejecuta con los permisos del usuario actual del equipo local. La cuenta de usuario especificada debe ser miembro del grupo administradores en el equipo remoto. Los parámetros **/u** y **/p** solo son válidos cuando se utiliza **/s**. |
| /p `<password>` | Especifica la contraseña de la cuenta de usuario especificada en el parámetro **/u** . Si usa el parámetro **/u** sin el parámetro **/p** o el argumento password, SchTasks le solicitará una contraseña. Los parámetros **/u** y **/p** solo son válidos cuando se utiliza **/s**. |
| /RU `<username>` | Cambia el nombre de usuario con el que se ejecuta la tarea programada. En la cuenta del sistema, los valores válidos son *""*, *"NT AUTHORITY\SYSTEM"* o *"System"*. |
| /RP `<password>` | Especifica una nueva contraseña para la cuenta de usuario existente o la cuenta de usuario especificada por el parámetro **/RU** . Este parámetro se omite cuando se usa con la cuenta de sistema local. |
| /TR `<Taskrun>` | Cambia el programa que ejecuta la tarea. Escriba la ruta de acceso completa y el nombre de archivo de un archivo ejecutable, un archivo de script o un archivo por lotes. Si no agrega la ruta de acceso, **SchTasks** supone que el archivo se encuentra en el `<systemroot>\System32` directorio. El programa especificado reemplaza el programa original ejecutado por la tarea. |
| /St `<Starttime>` | Especifica la hora de inicio de la tarea, con el formato de hora de 24 horas, HH: mm. Por ejemplo, un valor de 14:30 es equivalente a la hora de 12 horas de 2:30 PM. |
| /RI `<interval>` | Especifica el intervalo de repetición de la tarea programada, en minutos. El intervalo válido es 1-599940 (599940 minutos = 9999 horas). Si se especifican los parámetros **/et** o **/du** , el valor predeterminado es **10 minutos**. |
| /et `<Endtime>` | Especifica la hora de finalización de la tarea, con el formato de hora de 24 horas, HH: mm. Por ejemplo, un valor de 14:30 es equivalente a la hora de 12 horas de 2:30 PM. |
| /du `<duration>` | Valor que especifica la duración para ejecutar la tarea. El formato de hora es HH: mm (hora de 24 horas). Por ejemplo, un valor de 14:30 es equivalente a la hora de 12 horas de 2:30 PM. |
| /k | Detiene el programa que ejecuta la tarea en el momento especificado por **/et** o **/du**. Sin **/k**, Schtasks no vuelve a iniciar el programa después de alcanzar el tiempo especificado por **/et** o **/du** , ni detiene el programa si aún se está ejecutando. Este parámetro es opcional y solo es válido con una programación por minuto o hora. |
| /SD `<Startdate>` | Especifica la primera fecha en la que se debe ejecutar la tarea. El formato de fecha es MM/DD/AAAA. |
| /Ed `<Enddate>` | Especifica la última fecha en la que se debe ejecutar la tarea. El formato es MM/DD/AAAA. |
| /ENABLE | Especifica que se va a habilitar la tarea programada. |
| /DISABLE | Especifica que se deshabilite la tarea programada. |
| /It | Especifica que solo se ejecute la tarea programada cuando el usuario de ejecución (la cuenta de usuario en la que se ejecuta la tarea) ha iniciado sesión en el equipo. Este parámetro no tiene ningún efecto en las tareas que se ejecutan con permisos del sistema o tareas que ya tienen establecida la propiedad solo Interactive. No se puede usar un comando de cambio para quitar la propiedad solo interactiva de una tarea. De forma predeterminada, ejecutar como usuario es el usuario actual del equipo local cuando la tarea está programada o la cuenta especificada por el parámetro **/u** , si se usa una. Sin embargo, si el comando incluye el parámetro **/RU** , el usuario de ejecución es la cuenta especificada por el parámetro **/RU** . |
| /z | Especifica la eliminación de la tarea tras la finalización de su programación. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Los parámetros **/TN** y **/s** identifican la tarea. Los parámetros **/TR**, **/RU**y **/RP** especifican las propiedades de la tarea que puede cambiar.

- Los parámetros **/RU** y **/RP** especifican los permisos en los que se ejecuta la tarea. Los parámetros **/u** y **/p** especifican los permisos que se usan para cambiar la tarea.

- Para cambiar las tareas de un equipo remoto, el usuario debe haber iniciado sesión en el equipo local con una cuenta que sea miembro del grupo administradores en el equipo remoto.

- Para ejecutar un comando **/Change** con los permisos de un usuario diferente (**/u**, **/p**), el equipo local debe estar en el mismo dominio que el equipo remoto o debe estar en un dominio en el que confíe el dominio del equipo remoto.

- La cuenta del sistema no tiene derechos de inicio de sesión interactivo. Los usuarios no ven y no pueden interactuar con los programas que se ejecutan con permisos del sistema.
Para identificar las tareas con la propiedad **/it** , use una consulta detallada (**/query/v**). En una presentación de consulta detallada de una tarea con **/it**, el campo modo de inicio de sesión tiene un valor de solo interactivo.

## <a name="examples"></a>Ejemplos

Para cambiar el programa que se ejecuta en la tarea de comprobación de virus desde *VirusCheck.exe* a *VirusCheck2.exe*, escriba:

```
schtasks /change /tn Virus Check /tr C:\VirusCheck2.exe
```

Este comando usa el parámetro **/TN** para identificar la tarea y el parámetro **/TR** para especificar el nuevo programa para la tarea. (No se puede cambiar el nombre de la tarea).

Para cambiar la contraseña de la cuenta de usuario de la tarea *RemindMe* en el equipo remoto, *Svr01*, escriba:

```
schtasks /change /tn RemindMe /s Svr01 /rp p@ssWord3
```

Este procedimiento es necesario siempre que expire o cambie la contraseña de una cuenta de usuario. Si la contraseña guardada en una tarea ya no es válida, la tarea no se ejecuta. El comando usa el parámetro **/TN** para identificar la tarea y el parámetro **/s** para especificar el equipo remoto. Usa el parámetro **/RP** para especificar la nueva contraseña, *p@ssWord3* .

Para cambiar la tarea ChkNews, que comienza Notepad.exe cada mañana a las 9:00 A.M., para iniciar Internet Explorer en su lugar, escriba:

```
schtasks /change /tn ChkNews /tr c:\program files\Internet Explorer\iexplore.exe /ru DomainX\Admin01
```

El comando usa el parámetro **/TN** para identificar la tarea. Usa el parámetro **/TR** para cambiar el programa que ejecuta la tarea y el parámetro/ru para cambiar la cuenta de usuario en la que se ejecuta la tarea. No se usan los parámetros **/RU** y **/RP** , que proporcionan la contraseña de la cuenta de usuario. Debe proporcionar una contraseña para la cuenta, pero puede usar el parámetro **/RU** y **/RP** y escribir la contraseña en texto no cifrado, o bien esperar a que SchTasks.exe le pida una contraseña y, a continuación, escribir la contraseña en texto oculto.

Para cambiar la tarea SecurityScript de modo que se ejecute con permisos de la cuenta del sistema, escriba:

```
schtasks /change /tn SecurityScript /ru
```

El comando usa el parámetro **/RU** para indicar la cuenta del sistema. Dado que las tareas que se ejecutan con permisos de cuenta del sistema no requieren una contraseña, SchTasks.exe no solicita una.

Para agregar la propiedad solo Interactive a MyApp, una tarea existente, escriba:

```
schtasks /change /tn MyApp /it
```

Esta propiedad garantiza que la tarea se ejecute solo cuando el usuario de ejecución, es decir, la cuenta de usuario en la que se ejecuta la tarea, inicie sesión en el equipo. El comando usa el parámetro **/TN** para identificar la tarea y el parámetro **/it** para agregar la propiedad solo interactiva a la tarea. Dado que la tarea ya se ejecuta con los permisos de mi cuenta de usuario, no es necesario cambiar el parámetro **/RU** de la tarea.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Schtasks Create](schtasks-create.md)

- [comando de eliminación de SchTasks](schtasks-delete.md)

- [comando de fin de SchTasks](schtasks-end.md)

- [comando de consulta SchTasks](schtasks-query.md)

- [comando ejecutar SchTasks](schtasks-run.md)
