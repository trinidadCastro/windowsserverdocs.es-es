---
title: schtasks
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2e713203-3dd8-491b-b9e1-9423618dc7e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76ae264ded3cd0611c06b56c1074a2d916513da4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441614"
---
# <a name="schtasks"></a>schtasks



Programa comandos y programas para ejecutarse periódicamente o en un momento determinado. Agrega y quita tareas de la programación, se inicia y detiene las tareas a petición y muestra y cambia las tareas programadas.

Para ver la sintaxis del comando, haga clic en uno de los siguientes comandos:
-   [schtasks create](#BKMK_create)
-   [schtasks change](#BKMK_change)
-   [schtasks run](#BKMK_run)
-   [SCHTASKS final](#BKMK_end)
-   [schtasks delete](#BKMK_delete)
-   [schtasks query](#BKMK_query)

## <a name="remarks"></a>Comentarios

- **SchTasks.exe** realiza las mismas operaciones que **tareas programadas** en **Panel de Control**. Puede usar estas herramientas juntos e indistintamente.
- **SCHTASKS** reemplaza **At.exe**, una herramienta incluida en versiones anteriores de Windows. Aunque **At.exe** aún se incluye en la familia Windows Server 2003, **schtasks** es la tarea de línea de comandos recomendada herramienta de programación.
- Los parámetros en un **schtasks** comando puede aparecer en cualquier orden. Escriba **schtasks** sin ningún parámetro realiza una consulta.
- Permisos para **schtasks**  
  -   Debe tener permiso para ejecutar el comando. Cualquier usuario puede programar una tarea en el equipo local, y pueden ver y cambiar las tareas que programó. Los miembros del grupo administradores pueden programar, ver y cambiar todas las tareas en el equipo local.
  -   Para programar, ver o cambiar una tarea en un equipo remoto, debe ser miembro del grupo Administradores en el equipo remoto, o debe usar el **/u** parámetro para proporcionar las credenciales de administrador del equipo remoto.
  -   Puede usar el **/u** parámetro en un **/ crear** o **/cambiar** operación solo cuando los equipos locales y remotos están en el mismo dominio o el equipo local está en un dominio que las confianzas de dominio del equipo remoto. En caso contrario, el equipo remoto no puede autenticar la cuenta de usuario especificada y no puede comprobar que la cuenta es miembro del grupo Administradores.
  -   La tarea debe tener permiso para ejecutar. Los permisos necesarios varían con la tarea. De forma predeterminada, las tareas ejecutan con los permisos del usuario actual del equipo local o con los permisos del usuario especificado por el **/u** parámetro, si uno está incluido. Para ejecutar una tarea con permisos de una cuenta de usuario diferente o con permisos del sistema, use la **/ru** parámetro.
- Para comprobar que se ejecutó una tarea programada o para averiguar por qué no se ejecutó una tarea programada, consulte el registro de transacciones del servicio Programador de tareas, *SystemRoot*\SchedLgU.txt. Entradas del registro ha intentado ejecuciones iniciadas por todas las herramientas que usan el servicio, incluidos **tareas programadas** y **SchTasks.exe**.
- En raras ocasiones, los archivos de tareas dañados. No se ejecutan tareas dañadas. Cuando se intenta realizar una operación en tareas dañadas, **SchTasks.exe** muestra el mensaje de error siguiente:  
  ```
  ERROR: The data is invalid.
  ```  
  No se puede recuperar tareas dañadas. Para restaurar las características del sistema de programación de tareas, use **SchTasks.exe** o **tareas programadas** para eliminar las tareas del sistema y programarlos.

## <a name="BKMK_create"></a>SCHTASKS crear

Programa una tarea.

**SCHTASKS** utiliza varias combinaciones de parámetros para cada tipo de programación. Para ver la sintaxis combinada para la creación de tareas o para ver la sintaxis para crear una tarea con un tipo de programación determinado, haga clic en una de las siguientes opciones.
-   [Descripciones de sintaxis y los parámetros combinadas](#BKMK_syntax)
-   [Para programar una tarea que se ejecuta cada N minutos](#BKMK_minutes)
-   [Para programar una tarea que se ejecuta cada N horas](#BKMK_hours)
-   [Para programar una tarea que se ejecuta cada N días](#BKMK_days)
-   [Para programar una tarea que se ejecuta cada N semanas](#BKMK_weeks)
-   [Para programar una tarea que se ejecuta cada N meses](#BKMK_months)
-   [Para programar una tarea que se ejecuta en un determinado día de la semana](#BKMK_spec_day)
-   [Para programar una tarea que se ejecuta en un semana determinada del mes](#BKMK_spec_week)
-   [Para programar una tarea que se ejecuta en una fecha específica de cada mes](#BKMK_spec_date)
-   [Para programar una tarea que se ejecuta en el último día del mes](#BKMK_last_day)
-   [Para programar una tarea que se ejecuta una vez](#BKMK_once)
-   [Para programar una tarea que se ejecuta cada vez que se inicia el sistema](#BKMK_startup)
-   [Para programar una tarea que se ejecuta cuando un usuario inicia sesión](#BKMK_logon)
-   [Para programar una tarea que se ejecuta cuando el sistema está inactivo](#BKMK_idle)
-   [Para programar una tarea que se ejecuta ahora](#BKMK_now)
-   [Para programar una tarea que se ejecuta con permisos diferentes](#BKMK_diff_perms)
-   [Para programar una tarea que se ejecuta con permisos del sistema](#BKMK_sys_perms)
-   [Para programar una tarea que se ejecuta más de un programa](#BKMK_multi_progs)
-   [Para programar una tarea que se ejecuta en un equipo remoto](#BKMK_remote)

### <a name="BKMK_syntax"></a>Descripciones de sintaxis y los parámetros combinadas

#### <a name="syntax"></a>Sintaxis

```
schtasks /create /sc <ScheduleType> /tn <TaskName> /tr <TaskRun> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/mo <Modifier>] [/d <Day>[,<Day>...] | *] [/m <Month>[,<Month>...]] [/i <IdleTime>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/it] [/z] [/f]
```

#### <a name="parameters"></a>Parámetros

##### <a name="sc-scheduletype"></a>/SC \<ScheduleType >

Especifica el tipo de programación. Los valores válidos son minutos, cada hora, DIARIAMENTE, SEMANALMENTE, MENSUALMENTE, una vez, ONSTART, al iniciar la sesión, ONIDLE.

|Tipo de programación|Descripción|
|-------------|-----------|
|MINUTO, CADA HORA, DIARIA, SEMANAL, MENSUAL|Especifica la unidad de tiempo para la programación.|
|UNA VEZ|La tarea se ejecuta una vez en una fecha y hora especificadas.|
|ONSTART|La tarea se ejecuta cada vez que se inicia el sistema. Puede especificar una fecha de inicio, o ejecutar la tarea la próxima vez que se inicia el sistema.|
|AL INICIAR LA SESIÓN|La tarea se ejecuta cada vez que un usuario (cualquier usuario) inicia sesión. Puede especificar una fecha o ejecutar la tarea la próxima vez que el usuario inicia sesión.|
|ONIDLE|La tarea se ejecuta cada vez que el sistema está inactivo durante un período de tiempo especificado. Puede especificar una fecha o ejecutar la tarea la próxima vez que el sistema está inactivo.|

##### <a name="tn-taskname"></a>/tn \<TaskName>

Especifica un nombre para la tarea. Cada tarea en el sistema debe tener un nombre único. El nombre debe cumplir las reglas de los nombres de archivo y no debe superar los 238 caracteres. Utilice las comillas para delimitar los nombres que incluyen espacios.

##### <a name="tr-taskrun"></a>/tr \<TaskRun>

Especifica el programa o comando que ejecuta la tarea. Escriba la ruta de acceso y el nombre completo de un archivo ejecutable, un archivo de script o un archivo por lotes. El nombre de ruta de acceso no debe superar los 262 caracteres. Si se omite la ruta de acceso, **schtasks** supone que el archivo está en el *SystemRoot*directorio \System32.

##### <a name="s-computer"></a>/s \<equipo >

Programa una tarea en el equipo remoto especificado. Escriba el nombre o dirección IP de un equipo remoto (con o sin barras diagonales inversas). El valor predeterminado es el equipo local. El **/u** y **/p** parámetros son válidos únicamente cuando usa **/s**.

##### <a name="u-domainuser"></a>/u [\<Domain>\]<User>

Este comando se ejecuta con los permisos de la cuenta de usuario especificado. El valor predeterminado es el permiso del usuario actual del equipo local. El **/u** y **/p** parámetros solo son válidos para programar una tarea en un equipo remoto ( **/s**).

Los permisos de la cuenta especificada se usan para programar la tarea y para ejecutar la tarea. Para ejecutar la tarea con los permisos de un usuario diferente, use el **/ru** parámetro.

La cuenta de usuario debe ser miembro del grupo Administradores en el equipo remoto. Además, el equipo local debe estar en el mismo dominio que el equipo remoto, o debe estar en un dominio de confianza para el dominio del equipo remoto.

##### <a name="p-password"></a>/p \<contraseña >

Proporciona la contraseña de la cuenta de usuario especificada en el **/u** parámetro. Si usas el **/u** parámetro, pero omita la **/p** parámetro o el argumento de contraseña, **schtasks** le pide una contraseña y oculta el texto que escriba.

El **/u** y **/p** parámetros solo son válidos para programar una tarea en un equipo remoto ( **/s**).

##### <a name="ru-domainuser--system"></a>/RU {[\<dominio >\] <User> | Sistema}

Ejecuta la tarea con permisos de la cuenta de usuario especificado. De forma predeterminada, la tarea se ejecuta con los permisos del usuario actual del equipo local o con el permiso del usuario especificado por el **/u** parámetro, si uno está incluido. El **/ru** parámetro es válido al programar tareas en equipos locales o remotos.


|       Valor        |                                                    Descripción                                                    |
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| [\<Dominio >\]<User> |                                       Especifica una cuenta de usuario alternativa.                                        |
|    Sistema o ""    | Especifica la cuenta de sistema local, una cuenta con privilegios elevados, utilizada por el sistema operativo y los servicios del sistema. |

##### <a name="rp-password"></a>/RP \<contraseña >

Proporciona la contraseña de la cuenta de usuario que se especifica en el **/ru** parámetro. Si se omite este parámetro al especificar una cuenta de usuario **SchTasks.exe** le pide la contraseña y oculta el texto que escriba.

No utilice el **/rp** parámetro para las tareas de ejecución con las credenciales de cuenta de sistema ( **/ru sistema**). La cuenta del sistema no tiene una contraseña y **SchTasks.exe** no solicita uno.

##### <a name="mo-modifier"></a>/ mes \<modificador >

Especifica la frecuencia con la tarea se ejecuta dentro de su tipo de programación. Este parámetro es válido, pero opcional, durante un minuto, cada hora, diaria, semanal y mensual de programación. El valor predeterminado es 1.

|Tipo de programación|Valores de modificador|Descripción|
|-------------|---------------|-----------|
|MINUTO|1 - 1439|La tarea se ejecuta cada \<N > minutos.|
|POR HORA|1 - 23|La tarea se ejecuta cada \<N > horas.|
|DIARIA|1 - 365|La tarea se ejecuta cada \<N > días.|
|SEMANAL|1 - 52|La tarea se ejecuta cada \<N > semanas.|
|UNA VEZ|Sin modificadores.|La tarea se ejecuta una vez.|
|ONSTART|Sin modificadores.|La tarea se ejecute al inicio.|
|AL INICIAR LA SESIÓN|Sin modificadores.|La tarea se ejecuta cuando el usuario especificado por el **/u** parámetro inicia sesión.|
|ONIDLE|Sin modificadores.|La tarea se ejecuta después de que el sistema está inactivo durante el número de minutos especificado por el **/i** parámetro, que es necesario para su uso con ONIDLE.|
|MENSUALMENTE|1 - 12|La tarea se ejecuta cada \<N > meses.|
|MENSUALMENTE|LASTDAY|La tarea se ejecuta en el último día del mes.|
|MENSUALMENTE|PRIMERO, SEGUNDO, TERCERO, CUARTA, ÚLTIMA|Usar con el **/d**\<día > parámetro para ejecutar una tarea en una determinada semana y día. Por ejemplo, en el tercer miércoles del mes.|

##### <a name="d-dayday--"></a>Día /d [,... días] | *

Especifica un día o días de la semana o un día (o días) de un mes. Válido únicamente con una programación semanal o MENSUALMENTE.


| Tipo de programación |              Modificador              |     Los valores de día (/ d)      |                                                                                                 Descripción                                                                                                 |
|---------------|------------------------------------|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    SEMANAL     |               1 - 52               | MON - SUN[,MON - SUN...] |                                                                                                     \*                                                                                                      |
|    MENSUALMENTE    | PRIMERO, SEGUNDO, TERCERO, CUARTA, ÚLTIMA |        MON - SUN         |                                                                                   Se requiere para una programación de la semana específica.                                                                                    |
|    MENSUALMENTE    |          Ninguno o {1-12}          |          1 - 31          | Opcional y válido únicamente con ningún modificador ( **/mes**) parámetro (programación de una fecha concreta) o cuando el **/mes** es 1-12 (un "cada \<N > meses" programación). El valor predeterminado es el día 1 (el primer día del mes). |

##### <a name="m-monthmonth"></a>/m mes [, mes...]

Especifica un mes o los meses del año durante el cual se debe ejecutar la tarea programada. Los valores válidos son Ene - Dic y * (cada mes). El **/m** parámetro solo es válido con una programación mensual. Es necesario cuando se usa el modificador LASTDAY. En caso contrario, es opcional y el valor predeterminado es * (cada mes).

##### <a name="i-idletime"></a>/i \<IdleTime>

Especifica cuántos minutos el equipo está inactivo antes de iniciar la tarea. Un valor válido es un número entero comprendido entre 1 y 999. Este parámetro solo es válido con una programación ONIDLE y, a continuación, es necesario.

##### <a name="st-starttime"></a>/ST \<StartTime >

Especifica la hora del día en que la tarea se inicia (cada vez que se inicie) en \<formato de hora hh: mm > 24-. El valor predeterminado es la hora actual en el equipo local. El **/st** parámetro es válido con minuto, cada hora, DIARIAMENTE, SEMANALMENTE, MENSUALMENTE y se programa una vez. Es necesario para una programación de una vez.

##### <a name="ri-interval"></a>/RI \<intervalo >

Especifica el intervalo de repetición en minutos. Esto no es aplicable para los tipos de programación: MINUTO, cada hora, ONSTART, al iniciar la sesión y ONIDLE. El intervalo válido es de 1 a 599940 minutos (599940 minutos = 9999 horas). Si se especifica /ET o /DU, a continuación, el intervalo de repetición predeterminado es 10 minutos.

##### <a name="et-endtime"></a>/et \<EndTime>

Especifica la hora del día que finaliza una programación de la tarea por hora o minuto en \<formato de hora hh: mm > 24-. Después de la hora de finalización especificada, **schtasks** no se inicia la tarea de nuevo hasta que se repite la hora de inicio. De forma predeterminada, las programaciones de tareas no tienen ninguna hora de finalización. Este parámetro es opcional y válido sólo con una programación por horas o minutos.

Para obtener un ejemplo, consulte:
-   "Para programar una tarea que ejecuta cada 100 minutos durante el horario no comercial" el **para programar una tarea que se ejecuta cada** \<N > **minutos** sección.

##### <a name="du-duration"></a>/du \<duración >

Especifica una longitud máxima de tiempo para un minuto o la programación por hora en \<HHHH: mm > 24 - formato de hora. Después de transcurre el tiempo especificado, **schtasks** no se inicia la tarea de nuevo hasta que se repite la hora de inicio. De forma predeterminada, las programaciones de tareas no tienen duración máxima. Este parámetro es opcional y válido sólo con una programación por horas o minutos.

Para obtener un ejemplo, consulte:
-   "Para programar una tarea que ejecuta cada 3 horas durante 10 horas" en el **para programar una tarea que se ejecuta cada** \<N > **horas** sección.

##### <a name="k"></a>/k

Detiene el programa que se ejecuta la tarea en el momento especificado por **/et** o **/du**. Sin **/k**, **schtasks** no se inicia el programa de nuevo una vez que llega la hora especificada por **/et** o **/du**, pero no se detiene el programa si aún se está ejecutando. Este parámetro es opcional y válido sólo con una programación por horas o minutos.

Para obtener un ejemplo, consulte:
-   "Para programar una tarea que ejecuta cada 100 minutos durante el horario no comercial" el **para programar una tarea que se ejecuta cada** \<N > **minutos** sección.

##### <a name="sd-startdate"></a>/ SD \<StartDate >

Especifica la fecha en que comienza la programación de tareas. El valor predeterminado es la fecha actual en el equipo local. El **/SD** parámetro es opcional y válido para todos los tipos de esquema.

El formato de *StartDate* varía en función de la configuración regional seleccionada para el equipo local en **regional e idioma** en **Panel de Control**. Solo formato es válido para cada configuración regional.

En la tabla siguiente, se enumeran los formatos de fecha válido. Use el formato más parecido al formato seleccionado para **fecha corta** en **regional e idioma** en **Panel de Control** en el equipo local.


|       Valor       |                                        Descripción                                         |
|-------------------|--------------------------------------------------------------------------------------------|
| \<MM>/<DD>/<YYYY> | Usar formatos de mes en primer lugar, como **inglés (Estados Unidos)** y **español (Panamá)** . |
| \<DD>/<MM>/<YYYY> |       Usar formatos de día en primer lugar, como **búlgaro** y **neerlandés (Países Bajos)** .        |
| \<AAAA &GT; /<MM>/<DD> |          Usar formatos de año en primer lugar, como **sueco** y **francés (Canadá)** .          |

/Ed \<EndDate >

Especifica la fecha en que finaliza la programación. Este parámetro es opcional. No es válido en una programación ONIDLE, ONSTART, al iniciar la sesión o una vez. De forma predeterminada, las programaciones no tengan ninguna fecha de finalización.

El formato de *EndDate* varía en función de la configuración regional seleccionada para el equipo local en **regional e idioma** en **Panel de Control**. Solo formato es válido para cada configuración regional.

En la tabla siguiente, se enumeran los formatos de fecha válido. Use el formato más parecido al formato seleccionado para **fecha corta** en **regional e idioma** en **Panel de Control** en el equipo local.


|       Valor       |                                        Descripción                                         |
|-------------------|--------------------------------------------------------------------------------------------|
| \<MM>/<DD>/<YYYY> | Usar formatos de mes en primer lugar, como **inglés (Estados Unidos)** y **español (Panamá)** . |
| \<DD>/<MM>/<YYYY> |       Usar formatos de día en primer lugar, como **búlgaro** y **neerlandés (Países Bajos)** .        |
| \<AAAA &GT; /<MM>/<DD> |          Usar formatos de año en primer lugar, como **sueco** y **francés (Canadá)** .          |

##### <a name="it"></a>/it

Especifica que se ejecute la tarea solo cuando el usuario "Ejecutar como" (la cuenta de usuario que se ejecuta la tarea) ha iniciado sesión en el equipo. Este parámetro tiene ningún efecto en tareas que se ejecutan con permisos del sistema.

De forma predeterminada, el usuario "Ejecutar como" es el usuario actual del equipo local cuando se programa la tarea o la cuenta especificada por el **/u** parámetro, si se usa alguno. Sin embargo, si el comando incluye el **/ru** parámetro, a continuación, el usuario "Ejecutar como" es la cuenta especificada por el **/ru** parámetro.

Para obtener ejemplos, vea:
-   "Para programar una tarea que se ejecuta cada 70 días si el usuario ha iniciado sesión" en el **para programar una tarea que se ejecuta cada** *N* **días** sección.
-   "Ejecutar una tarea solo cuando un usuario determinado ha iniciado sesión" el **para programar una tarea que se ejecuta con permisos diferentes** sección.

##### <a name="z"></a>/z

Especifica que se elimine la tarea tras la finalización de su programación.

##### <a name="f"></a>/f

Especifica que se crea la tarea y suprimir advertencias si la tarea especificada ya existe.

##### <a name=""></a>/?

Muestra la ayuda en el símbolo del sistema.

### <a name="BKMK_minutes"></a>Para programar una tarea que se ejecuta cada N minutos

#### <a name="minute-schedule-syntax"></a>Sintaxis de programación por minuto

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc minute [/mo {1 - 1439}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentarios

En una programación por minuto, el **/sc minuto** el parámetro es obligatorio. El **/mes** parámetro (modificador) es opcional y especifica el número de minutos entre cada ejecución de la tarea. El valor predeterminado de **/mes** es 1 (cada minuto). El **/et** (hora de finalización) y **/du** parámetros (duración) son opcionales y pueden usarse con o sin el **/k** parámetro (Finalizar tarea).

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-that-runs-every-20-minutes"></a>Para programar una tarea que se ejecuta cada 20 minutos

El siguiente comando programa una secuencia de comandos de seguridad, Sec.vbs, para ejecutar cada 20 minutos. El comando usa el **/sc** parámetro para especificar una programación por minuto y el **/mes** parámetro para especificar un intervalo de 20 minutos.

Dado que el comando no incluye una fecha de inicio u hora, la tarea inicia 20 minutos después de completarse el comando y se ejecuta cada 20 minutos a partir de entonces, siempre que se está ejecutando el sistema. Tenga en cuenta que el archivo de origen de la secuencia de comandos de seguridad se encuentra en un equipo remoto, pero que la tarea está programada y se ejecuta en el equipo local.
```
schtasks /create /sc minute /mo 20 /tn "Security Script" /tr \\central\data\scripts\sec.vbs
```

#### <a name="to-schedule-a-task-that-runs-every-100-minutes-during-non-business-hours"></a>Para programar una tarea que se ejecuta cada 100 minutos durante el horario no comercial

El siguiente comando programa una secuencia de comandos de seguridad, Sec.vbs, para ejecutarse en el equipo local cada 100 minutos entre las 5:00 P.M. y las 7:59 A.M. cada día. El comando usa el **/sc** parámetro para especificar una programación por minuto y el **/mes** parámetro para especificar un intervalo de 100 minutos. Usa el **/st** y **/et** parámetros para especificar la hora de inicio y finalización de la programación de cada día. También usa el **/k** parámetro para detener la secuencia de comandos si todavía se está ejecutando a las 7:59 A.M. Sin **/k**, **schtasks** no se iniciaría la secuencia de comandos después de 7:59 A.M., pero si la instancia se inicia a las 6:20 A.M. estaba aún se está ejecutando, no desea detener.
```
schtasks /create /tn "Security Script" /tr sec.vbs /sc minute /mo 100 /st 17:00 /et 08:00 /k
```

### <a name="BKMK_hours"></a>Para programar una tarea que se ejecuta cada N horas

#### <a name="hourly-schedule-syntax"></a>Sintaxis de programación por hora

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc hourly [/mo {1 - 23}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentarios

En una programación por hora, el **/sc hourly** el parámetro es obligatorio. El **/mes** parámetro (modificador) es opcional y especifica el número de horas entre cada ejecución de la tarea. El valor predeterminado de **/mes** es 1 (cada hora). El **/k** parámetro (Finalizar tarea) es opcional y puede usarse con ninguno **/et** (fin a la hora especificada) o **/du** (end después del intervalo especificado).

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-that-runs-every-five-hours"></a>Para programar una tarea que se ejecuta cada cinco horas

El siguiente comando programa el programa MyApp para ejecutarse cada cinco horas a partir del primer día de marzo de 2002. Usa el **/mes** parámetro para especificar el intervalo y la **/SD** parámetro para especificar la fecha de inicio. Dado que el comando no especifica una hora de inicio, la hora actual se usa como la hora de inicio.

Dado que el equipo local está configurado para usar el **inglés (Zimbabue)** opción **regional e idioma** en **Panel de Control**, el formato de la fecha de inicio es MM/DD/AAAA (03/01/2002).
```
schtasks /create /sc hourly /mo 5 /sd 03/01/2002 /tn "My App" /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-hour-at-five-minutes-past-the-hour"></a>Para programar una tarea que se ejecuta cada hora en cinco minutos después de la hora

El siguiente comando programa el programa MyApp para ejecutarse cada hora empezando en cinco minutos pasada medianoche. Dado que el **/mes** parámetro se omite, el comando utiliza el valor predeterminado para la programación por hora, que es cada hora (1). Si este comando se ejecuta después de 12:05 h, el programa no se ejecuta hasta el día siguiente.
```
schtasks /create /sc hourly /st 00:05 /tn "My App" /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-3-hours-for-10-hours"></a>Para programar una tarea que se ejecute cada 3 horas durante 10 horas

El siguiente comando programa el programa MyApp para ejecutarse cada 3 horas durante 10 horas.

El comando usa el **/sc** parámetro para especificar una programación por hora y el **/mes** parámetro para especificar el intervalo de 3 horas. Usa el **/st** parámetro para iniciar la programación a medianoche y el **/du** parámetro para finalizar las repeticiones después de 10 horas. Dado que el programa se ejecuta durante unos minutos, el **/k** parámetro, que detiene el programa si aún se está ejecutando cuando expire la duración, no es necesario.
```
schtasks /create /tn "My App" /tr myapp.exe /sc hourly /mo 3 /st 00:00 /du 0010:00
```
En este ejemplo, la tarea se ejecuta a las 12:00 A.M., 3:00 a. M., 6:00 A.M. y 9:00 A.M. Dado que la duración es de 10 horas, no se ejecuta la tarea de nuevo a 12:00 P.M. En su lugar, se inicia nuevamente a 12:00 A.M. el día siguiente.

### <a name="BKMK_days"></a>Para programar una tarea que se ejecuta cada N días

#### <a name="daily-schedule-syntax"></a>Sintaxis de programación diaria

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc daily [/mo {1 - 365}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentarios

En una programación diaria, el **/sc diariamente** el parámetro es obligatorio. El **/mes** parámetro (modificador) es opcional y especifica el número de días entre cada ejecución de la tarea. El valor predeterminado de **/mes** es 1 (cada día).

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-that-runs-every-day"></a>Para programar una tarea que se ejecuta cada día

En el siguiente ejemplo se programa MyApp para ejecutar una vez al día, cada día, a las 8:00 A.M. hasta el 31 de diciembre de 2002. Como se omite el **/mes** parámetro, el intervalo predeterminado de 1 se usa para ejecutar el comando cada día.

En este ejemplo, porque el sistema del equipo local está establecido en el **inglés (Reino Unido)** opción **regional e idioma** en **Panel de Control**, el formato de la fecha de finalización es MM/DD/AAAA (31/12/2002)
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc daily /st 08:00 /ed 31/12/2002
```

#### <a name="to-schedule-a-task-that-runs-every-12-days"></a>Para programar una tarea que se ejecuta cada 12 días

En el siguiente ejemplo se programa MyApp para ejecutarse cada doce días a la 1:00 P.M. (13:00) a partir del 31 de diciembre de 2002. El comando usa el **/mes** parámetro para especificar un intervalo de dos (2) días y el **/SD** y **/st** parámetros para especificar la fecha y hora.

En este ejemplo, porque el sistema está establecido en el **inglés (Zimbabue)** opción **regional e idioma** en **Panel de Control**, el formato de la fecha de finalización es MM/DD / AAAA (31/12/2002)
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc daily /mo 12 /sd 12/31/2002 /st 13:00
```

#### <a name="to-schedule-a-task-that-runs-every-70-days-if-i-am-logged-on"></a>Para programar una tarea que se ejecuta cada 70 días si el usuario ha iniciado sesión

El siguiente comando programa una secuencia de comandos de seguridad, Sec.vbs, para ejecutar cada 70 días. El comando usa el **/mes** parámetro para especificar un intervalo de 70 días. También usa el **/it** parámetro para especificar que la tarea se ejecuta solo cuando el usuario bajo cuya cuenta se ejecuta la tarea se registra en el equipo. Dado que la tarea se ejecutará con los permisos de mi cuenta de usuario, la tarea se ejecutará sólo cuando el usuario ha iniciado sesión.
```
schtasks /create /tn "Security Script" /tr sec.vbs /sc daily /mo 70 /it
```

> [!NOTE]
> Para identificar las tareas con los sólo interactiva ( **/it**) propiedad, utilice una consulta detallada **(/ consulta /v**). En una presentación de la consulta detallada de una tarea con **/it**, **modo de inicio de sesión** campo tiene un valor de **sólo interactivo**.

### <a name="BKMK_weeks"></a>Para programar una tarea que se ejecuta cada N semanas

#### <a name="weekly-schedule-syntax"></a>Sintaxis de programación semanal

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/mo {1 - 52}] [/d {<MON - SUN>[,MON - SUN...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentarios

En una programación semanal, el **/sc semanalmente** el parámetro es obligatorio. El **/mes** parámetro (modificador) es opcional y especifica el número de semanas entre cada ejecución de la tarea. El valor predeterminado de **/mes** es 1 (cada semana).

Las programaciones semanales también tienen un elemento opcional **/d** parámetro para programar la tarea se ejecute determinados días de la semana, o todos los días ( *). El valor predeterminado es MON (lunes). El diario (* ) opción es equivalente a la programación de una tarea diaria.

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-that-runs-every-six-weeks"></a>Para programar una tarea que se ejecuta cada seis semanas

El siguiente comando programa el programa MyApp para ejecutarse en un equipo remoto cada seis semanas. El comando usa el **/mes** parámetro para especificar el intervalo. Dado que el comando omite el **/d** parámetro, la tarea se ejecuta los lunes.

Este comando también utiliza el **/s** parámetro para especificar el equipo remoto y el **/u** parámetro para ejecutar el comando con los permisos de la cuenta de usuario administrador. Dado que el **/p** se omite el parámetro, **SchTasks.exe** pide al usuario la contraseña de cuenta de administrador.

También, ya que el comando se ejecuta de forma remota, todas las rutas de acceso en el comando, incluida la ruta de acceso a MyApp.exe, hacer referencia a rutas de acceso en el equipo remoto.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 6 /s Server16 /u Admin01
```

#### <a name="to-schedule-a-task-that-runs-every-other-week-on-friday"></a>Para programar una tarea que se ejecuta el viernes cada dos semanas

El siguiente comando programa una tarea para ejecutar todos los demás viernes. Usa el **/mes** parámetro para especificar el intervalo de dos semanas y el **/d** parámetro para especificar el día de la semana. Para programar una tarea que se ejecuta todos los viernes, omita el **/mes** parámetro o establézcala en 1.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 2 /d FRI
```

### <a name="BKMK_months"></a>Para programar una tarea que se ejecuta cada N meses

#### <a name="syntax"></a>Sintaxis

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly [/mo {1 - 12}] [/d {1 - 31}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentarios

En este tipo de programación, el **/sc mensualmente** el parámetro es obligatorio. El **/mes** parámetro (modificador), que especifica el número de meses entre cada ejecución de la tarea, es opcional y el valor predeterminado es 1 (cada mes). Este tipo de programación también tiene un elemento opcional **/d** parámetro para programar la tarea se ejecute en una fecha determinada del mes. El valor predeterminado es 1 (el primer día del mes).

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-that-runs-on-the-first-day-of-every-month"></a>Para programar una tarea que se ejecuta en el primer día de cada mes

El siguiente comando programa el programa MyApp para ejecutarse en el primer día de cada mes. Dado que un valor de 1 es el valor predeterminado para ambos el **/mes** parámetro (modificador) y el **/d** parámetro (día), estos parámetros se omiten en el comando.
```
schtasks /create /tn "My App" /tr myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-that-runs-every-three-months"></a>Para programar una tarea que se ejecuta cada tres meses

El siguiente comando programa el programa MyApp para ejecutarse cada tres meses. Usa el **/mes** parámetro para especificar el intervalo.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo 3
```

#### <a name="to-schedule-a-task-that-runs-at-midnight-on-the-21st-day-of-every-other-month"></a>Para programar una tarea que se ejecuta en la medianoche del día 21 de cada dos meses

El siguiente comando programa el programa MyApp para que se ejecute cada dos meses el día 21 del mes a medianoche. El comando Especifica que se debe ejecutar esta tarea durante un año de 2 de julio de 2002 a 30 de junio de 2003.

El comando usa el **/mes** parámetro para especificar el intervalo mensual (cada dos meses), el **/d** parámetro para especificar la fecha y el **/st** para especificar el tiempo. También usa el **/SD** y **/ed** parámetros para especificar el inicio de fecha y fecha de finalización, respectivamente. Dado que el equipo local está configurado para el **inglés (Sudáfrica)** opción **regional e idioma** en **Panel de Control**, las fechas se especifican en el formato local , AAAA/MM/DD.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo 2 /d 21 /st 00:00 /sd 2002/07/01 /ed 2003/06/30 
```

### <a name="BKMK_spec_day"></a>Para programar una tarea que se ejecuta en un determinado día de la semana

#### <a name="weekly-schedule-syntax"></a>Sintaxis de programación semanal

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/d {<MON - SUN>[,MON - SUN...] | *}] [/mo {1 - 52}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentarios

La programación "día de la semana" es una variación de la programación semanal. En una programación semanal, el **/sc semanalmente** el parámetro es obligatorio. El **/mes** parámetro (modificador) es opcional y especifica el número de semanas entre cada ejecución de la tarea. El valor predeterminado de **/mes** es 1 (cada semana). El **/d** parámetro, que es opcional, programa la tarea se ejecute determinados días de la semana, o todos los días (<em>). El valor predeterminado es MON (lunes). La opción de cada día (</em>* /d \***) es equivalente a la programación de una tarea diaria.

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-that-runs-every-wednesday"></a>Para programar una tarea que se ejecuta todos los miércoles

El siguiente comando programa el programa MyApp para que se ejecuta el miércoles de cada semana. El comando usa el **/d** parámetro para especificar el día de la semana. Dado que el comando omite el **/mes** parámetro, la tarea ejecuta cada semana.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /d WED
```

#### <a name="to-schedule-a-task-that-runs-every-eight-weeks-on-monday-and-friday"></a>Para programar una tarea que se ejecuta cada ocho semanas los lunes y el viernes

El siguiente comando programa una tarea para ejecutarse los lunes y el viernes de cada ocho semanas. Usa el **/d** parámetro para especificar los días y el **/mes** parámetro para especificar el intervalo de ocho semanas.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 8 /d MON,FRI
```

### <a name="BKMK_spec_week"></a>Para programar una tarea que se ejecuta en un semana determinada del mes

#### <a name="specific-week-syntax"></a>Sintaxis de la semana específico

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo {FIRST | SECOND | THIRD | FOURTH | LAST} /d MON - SUN [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentarios

En este tipo de programación, el **/sc mensualmente** parámetro, el **/mes** parámetro (modificador) y el **/d** parámetro (día) son necesarios. El **/mes** parámetro (modificador) especifica la semana en el que se ejecuta la tarea. El **/d** parámetro especifica el día de la semana. (Puede especificar solo un día de la semana para este tipo de programación). Esta programación tiene también un elemento opcional **/m** parámetro (mes) que le permite programar la tarea durante meses determinados o todos los meses (<em>). El valor predeterminado para el * */m</em>* parámetro es cada mes (* ).

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-for-the-second-sunday-of-every-month"></a>Para programar una tarea para el segundo domingo de cada mes

El siguiente comando programa el programa MyApp para ejecutarse en el segundo domingo de cada mes. Usa el **/mes** parámetro para especificar la segunda semana del mes y el **/d** parámetro para especificar el día.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo SECOND /d SUN
```

#### <a name="to-schedule-a-task-for-the-first-monday-in-march-and-september"></a>Para programar una tarea para el primer lunes de marzo y septiembre

El siguiente comando programa el programa MyApp para ejecutarse en el primer lunes de marzo y septiembre. Usa el **/mes** parámetro para especificar la primera semana del mes y el **/d** parámetro para especificar el día. Lo usa **/m** parámetro para especificar el mes, separando los argumentos del mes con una coma.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo FIRST /d MON /m MAR,SEP
```

### <a name="BKMK_spec_date"></a>Para programar una tarea que se ejecuta en una fecha específica de cada mes

#### <a name="specific-date-syntax"></a>Sintaxis de fecha específica

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /d {1 - 31} [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentarios

En el tipo de programación de fecha específica, la **/sc mensualmente** parámetro y el **/d** parámetro (día) son necesarios. El **/d** parámetro especifica una fecha del mes (1-31), no un día de la semana. Puede especificar solo un día en la programación. El **/mes** (modificador) parámetro no es válido con este tipo de programación.

El **/m** parámetro (mes) es opcional para este tipo de programación y el valor predeterminado es cada mes (<em>). **Schtasks</em>*  no le permite programar una tarea para una fecha que no se produce en un mes especificado por el **/m** parámetro. Sin embargo, si omite la **/m** parámetro y programar una tarea para una fecha que no aparece en cada mes, por ejemplo, el día 31, la tarea no se ejecuta en los meses más cortos. Para programar una tarea para el último día del mes, use el último tipo de programación del día.

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-for-the-first-day-of-every-month"></a>Para programar una tarea para el primer día de cada mes

El siguiente comando programa el programa MyApp para ejecutarse en el primer día de cada mes. Dado que el modificador predeterminado es none (ningún modificador), el día predeterminado es 1 y el mes predeterminado es cada mes, el comando no necesita ningún parámetro adicional.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-for-the-15th-days-of-may-and-june"></a>Para programar una tarea para el día 15 de mayo y junio

El siguiente comando programa MyApp para ejecutarse en 15 de mayo y el 15 de junio a las 3:00 P.M. (15:00). Usa el **/m** parámetro para especificar la fecha y la **/m** parámetro para especificar los meses. También usa el **/st** parámetro para especificar la hora de inicio.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /d 15 /m MAY,JUN /st 15:00
```

### <a name="BKMK_last_day"></a>Para programar una tarea que se ejecuta en el último día del mes

#### <a name="last-day-syntax"></a>Sintaxis de último día

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo LASTDAY /m {JAN - DEC[,JAN - DEC...] | *} [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentarios

En el último tipo de programación del día, la **/sc mensualmente** parámetro, el **/mes LASTDAY** parámetro (modificador) y el **/m** parámetro (mes) son necesarios. El **/d** (día) parámetro no es válido.

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-for-the-last-day-of-every-month"></a>Para programar una tarea para el último día del mes

El siguiente comando programa el programa MyApp para ejecutarse en el último día de cada mes. Usa el **/mes** parámetro para especificar el último día y el **/m** parámetro con el carácter comodín (*) para indicar que el programa ejecuta cada mes.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo lastday /m *
```

#### <a name="to-schedule-a-task-at-600-pm-on-the-last-days-of-february-and-march"></a>Para programar una tarea a las 6:00 P.M. en los últimos días de febrero y marzo

El siguiente comando programa MyApp para ejecutarse en el último día del mes de febrero y el último día de marzo a las 6:00 P.M. Usa el **/mes** parámetro para especificar el último día, la **/m** parámetro para especificar los meses y el **/st** parámetro para especificar la hora de inicio.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo lastday /m FEB,MAR /st 18:00
```

### <a name="BKMK_once"></a>Para programar una tarea que se ejecuta una vez

#### <a name="syntax"></a>Sintaxis

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once /st <HH:MM> [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentarios

En el tipo de programación de una sola ejecución, el **/sc una vez** el parámetro es obligatorio. El **/st** parámetro, que especifica el tiempo que se ejecuta la tarea, es necesario. El **/SD** parámetro, que especifica la fecha en que se ejecuta la tarea, es opcional. El **/mes** (modificador) y **/ed** parámetros de (fecha de finalización) no son válidos para este tipo de programación.

**SCHTASKS** no le permite programar una tarea se ejecute una vez si la fecha y hora especificadas están en el pasado, según la hora del equipo local. Para programar una tarea que se ejecuta una vez en un equipo remoto en una zona horaria distinta, debe programarla antes de esa fecha y hora se produce en el equipo local.

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-that-runs-one-time"></a>Para programar una tarea que se ejecuta una vez

El siguiente comando programa el programa MyApp para ejecutarse a medianoche del 1 de enero de 2003. Usa el **/sc** parámetro para especificar el tipo de programación y la **/SD** y **st** para especificar la fecha y hora.

Dado que usa el equipo local la **inglés (Estados Unidos)** opción **regional e idioma** en **Panel de Control**, el formato de la fecha de inicio es MM/DD/AAAA.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc once /sd 01/01/2003 /st 00:00
```

### <a name="BKMK_startup"></a>Para programar una tarea que se ejecuta cada vez que se inicia el sistema

#### <a name="syntax"></a>Sintaxis

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onstart [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentarios

En el tipo de programación en Inicio, la **/sc onstart** el parámetro es obligatorio. El **/SD** parámetro (fecha de inicio) es opcional y el valor predeterminado es la fecha actual.

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-that-runs-when-the-system-starts"></a>Para programar una tarea que se ejecuta cuando se inicia el sistema

El siguiente comando programa MyApp para ejecutarse cada vez que se inicia el sistema, a partir del 15 de marzo de 2001:

Dado que el equipo local es que utiliza el **inglés (Estados Unidos)** opción **regional e idioma** en **Panel de Control**, el formato de la fecha de inicio es MM/DD/AAAA .
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc onstart /sd 03/15/2001
```

### <a name="BKMK_logon"></a>Para programar una tarea que se ejecuta cuando un usuario inicia sesión

#### <a name="syntax"></a>Sintaxis

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onlogon [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentarios

El tipo de programación "en el inicio de sesión" programa una tarea que se ejecuta cada vez que cualquier usuario inicia sesión en el equipo. En el tipo de programación "en el inicio de sesión", la **al iniciar la sesión /sc** el parámetro es obligatorio. El **/SD** parámetro (fecha de inicio) es opcional y el valor predeterminado es la fecha actual.

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-that-runs-when-a-user-logs-on-to-a-remote-computer"></a>Para programar una tarea que se ejecuta cuando un usuario inicia sesión en un equipo remoto

El comando siguiente, programa un archivo por lotes para ejecutar cada vez que un usuario (cualquier usuario) inicia sesión en el equipo remoto. Usa el **/s** parámetro para especificar el equipo remoto. Dado que el comando es remoto, todas las rutas de acceso en el comando, incluida la ruta de acceso del archivo por lotes, hacen referencia a una ruta de acceso en el equipo remoto.
```
schtasks /create /tn "Start Web Site" /tr c:\myiis\webstart.bat /sc onlogon /s Server23
```

### <a name="BKMK_idle"></a>Para programar una tarea que se ejecuta cuando el sistema está inactivo

#### <a name="syntax"></a>Sintaxis

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onidle /i {1 - 999} [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Comentarios

La programación "en inactividad" tipo de programa una tarea que se ejecuta cada vez que haya ninguna actividad de usuario durante el tiempo especificado por el **/i** parámetro. En la programación "en inactividad" tipo, el **/sc onidle** parámetro y el **/i** parámetro son necesarios. El **/SD** (fecha de inicio) es opcional y el valor predeterminado es la fecha actual.

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-that-runs-whenever-the-computer-is-idle"></a>Para programar una tarea que se ejecuta cada vez que el equipo está inactivo

El siguiente comando programa el programa MyApp para ejecutarse cada vez que el equipo está inactivo. Usa los **/i** parámetro para especificar que el equipo debe permanecer inactivo durante diez minutos antes de iniciar la tarea.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc onidle /i 10
```

### <a name="BKMK_now"></a>Para programar una tarea que se ejecuta ahora

**SCHTASKS** no tiene un "ejecutar ahora" opción, pero puede simular esa opción mediante la creación de una tarea que se ejecuta una vez y se inicia en unos minutos.

#### <a name="syntax"></a>Sintaxis

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once [/st <HH:MM>] /sd <MM/DD/YYYY> [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-that-runs-a-few-minutes-from-now"></a>Para programar una tarea que ejecuta unos pocos minutos desde ahora.

El siguiente comando programa una tarea se ejecute una vez, el 13 de noviembre de 2002 a 2:18 p. M. hora local.

Dado que el equipo local es que utiliza el **inglés (Estados Unidos)** opción **regional e idioma** en **Panel de Control**, el formato de la fecha de inicio es MM/DD/AAAA .
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc once /st 14:18 /sd 11/13/2002
```

### <a name="BKMK_diff_perms"></a>Para programar una tarea que se ejecuta con permisos diferentes

Puede programar las tareas de todos los tipos que se ejecute con permisos de una cuenta alternativa en el equipo local y un equipo remoto. Además de los parámetros necesarios para el tipo de programación concreto, el **/ru** el parámetro es obligatorio y **/rp** parámetro es opcional.

#### <a name="examples"></a>Ejemplos

#### <a name="to-run-a-task-with-administrator-permissions-on-the-local-computer"></a>Para ejecutar una tarea con permisos de administrador en el equipo local

El siguiente comando programa el programa MyApp para ejecutarse en el equipo local. Usa el **/ru** para especificar que la tarea debe ejecutarse con los permisos de cuenta de administrador del usuario (Admin06). En este ejemplo, la tarea está programada para ejecutarse todos los martes, pero puede usar cualquier tipo de programación para una tarea que se ejecute con permisos alternativos.
```
schtasks /create /tn "My App" /tr myapp.exe /sc weekly /d TUE /ru Admin06
```
En respuesta, **SchTasks.exe** pide la contraseña para la cuenta Admin06 "Ejecutar como" y, a continuación, muestra un mensaje de confirmación.
```
Please enter the run as password for Admin06: ********
SUCCESS: The scheduled task "My App" has successfully been created.
```

#### <a name="to-run-a-task-with-alternate-permissions-on-a-remote-computer"></a>Para ejecutar una tarea con otros permisos en un equipo remoto

El siguiente comando programa el programa MyApp para ejecutarse en el equipo de Marketing cada cuatro días.

El comando usa el **/sc** parámetro para especificar una programación diaria y **/mes** parámetro para especificar un intervalo de cuatro días.

El comando usa el **/s** parámetro para proporcionar el nombre del equipo remoto y el **/u** parámetro para especificar una cuenta con permiso para programar una tarea en el equipo remoto (Admin01 en el Marketing equipo). También usa el **/ru** parámetro para especificar que la tarea debe ejecutarse con los permisos de la cuenta de usuario sin privilegios de administrador (usuario01 en el dominio Reskits). Sin el **/ru** parámetro, la tarea ejecutaría con los permisos de la cuenta especificada por **/u**.
```
schtasks /create /tn "My App" /tr myapp.exe /sc daily /mo 4 /s Marketing /u Marketing\Admin01 /ru Reskits\User01
```
**SCHTASKS** primero solicita la contraseña del usuario designado por el **/u** parámetro (para ejecutar el comando) y, a continuación, se solicita la contraseña del usuario designado por el **/ru** parámetro (para ejecutar la tarea). Después de autenticar las contraseñas, **schtasks** muestra un mensaje que indica que la tarea está programada.
```
Type the password for Marketing\Admin01:********

Please enter the run as password for Reskits\User01: ********

SUCCESS: The scheduled task "My App" has successfully been created.
```

#### <a name="to-run-a-task-only-when-a-particular-user-is-logged-on"></a>Para ejecutar una tarea solo cuando un usuario concreto se ha iniciado sesión

El siguiente comando programa el programa AdminCheck.exe para ejecutarse en el equipo público todos los viernes a las 4:00 A.M., pero solo si el administrador del equipo ha iniciado sesión.

El comando usa el **/sc** parámetro para especificar una programación semanal, el **/d** parámetro para especificar el día y el **/st** parámetro para especificar la hora de inicio.

El comando usa el **/s** parámetro para proporcionar el nombre del equipo remoto y el **/u** parámetro para especificar una cuenta con permiso para programar una tarea en el equipo remoto. También usa el **/ru** parámetro para configurar la tarea se ejecute con los permisos del administrador del equipo público (Public\Admin01) y el **/it** parámetro para indicar que la tarea se ejecuta solo Cuando la cuenta Public\Admin01 haya iniciado sesión.
```
schtasks /create /tn "Check Admin" /tr AdminCheck.exe /sc weekly /d FRI /st 04:00 /s Public /u Domain3\Admin06 /ru Public\Admin01 /it
```
**Nota:**
-   Para identificar las tareas con los sólo interactiva ( **/it**) propiedad, utilice una consulta detallada **(/ consulta /v**). En una presentación de la consulta detallada de una tarea con **/it**, **modo de inicio de sesión** campo tiene un valor de **sólo interactivo**.

### <a name="BKMK_sys_perms"></a>Para programar una tarea que se ejecuta con permisos del sistema

Tareas de todos los tipos pueden ejecutar con permisos de la cuenta del sistema en el equipo local y un equipo remoto. Además de los parámetros necesarios para el tipo de programación concreto, el **sistema /ru** (o **/ru ""** ) el parámetro es obligatorio y la **/rp** parámetro no es válido.

**Importante**
-   La cuenta del sistema no tiene derechos de inicio de sesión interactivo. Los usuarios no pueden ver o interactuar con programas o las tareas se ejecutan con permisos del sistema.
-   El **/ru** parámetro determina los permisos en el que se ejecuta la tarea, no los permisos utilizados para programar la tarea. Solo los administradores pueden programar tareas, independientemente del valor de la **/ru** parámetro.

**Nota:**

Para identificar tareas que se ejecutan con permisos del sistema, utilice una consulta detallada ( **/consulta** **/v**). En una presentación de la consulta detallada de una tarea ejecutada por el sistema, el **ejecutar como usuario** campo tiene un valor de **NT AUTHORITY\SYSTEM** y **modo de inicio de sesión** campo tiene un valor de  **En segundo plano solo**.

#### <a name="examples"></a>Ejemplos

#### <a name="to-run-a-task-with-system-permissions"></a>Para ejecutar una tarea con permisos del sistema

El siguiente comando programa el programa MyApp para ejecutarse en el equipo local con permisos de la cuenta del sistema. En este ejemplo, la tarea está programada para ejecutarse en el decimoquinto día de cada mes, pero puede usar cualquier tipo de programación para una tarea que se ejecute con permisos del sistema.

El comando usa el **/ru System** parámetro para especificar el contexto de seguridad del sistema. Dado que las tareas del sistema no usan una contraseña, el **/rp** se omite el parámetro.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /d 15 /ru System
```
En respuesta, **SchTasks.exe** muestra un mensaje informativo y un mensaje de confirmación. No se solicitará una contraseña.
```
INFO: The task will be created under user name ("NT AUTHORITY\SYSTEM").
SUCCESS: The Scheduled task "My App" has successfully been created.
```

#### <a name="to-run-a-task-with-system-permissions-on-a-remote-computer"></a>Para ejecutar una tarea con permisos del sistema en un equipo remoto

El siguiente comando programa MyApp para ejecutarse en el equipo Finance01 cada mañana a las 4:00 A.M. con permisos del sistema.

El comando usa el **/tn** parámetro un nombre de la tarea y el **/tr** parámetro para especificar la copia remota del programa MyApp. Usa el **/sc** parámetro para especificar una programación diaria, pero omite el **/mes** parámetro porque el predeterminado es 1 (cada día). Usa el **/st** parámetro para especificar la hora de inicio, que también es el tiempo que la tarea ejecutará cada día.

El comando usa el **/s** parámetro para proporcionar el nombre del equipo remoto y el **/u** parámetro para especificar una cuenta con permiso para programar una tarea en el equipo remoto. También usa el **/ru** parámetro para especificar que la tarea debe ejecutarse en la cuenta del sistema. Sin el **/ru** parámetro, la tarea ejecutaría con los permisos de la cuenta especificada por **/u**.
```
schtasks /create /tn "My App" /tr myapp.exe /sc daily /st 04:00 /s Finance01 /u Admin01 /ru System
```
**SCHTASKS** solicita la contraseña del usuario designado por el **/u** parámetro y, después de autenticar la contraseña, se muestra un mensaje que indica que se creó la tarea y que se ejecutará con permisos del sistema cuenta.
```
Type the password for Admin01:**********

INFO: The Schedule Task "My App" will be created under user name ("NT AUTHORITY\
SYSTEM").
SUCCESS: The scheduled task "My App" has successfully been created.
```

### <a name="BKMK_multi_progs"></a>Para programar una tarea que se ejecuta más de un programa

Cada tarea ejecuta un solo programa. Sin embargo, puede crear un archivo por lotes que se ejecuta varios programas y, a continuación, programar una tarea para ejecutar el archivo por lotes. El procedimiento siguiente muestra este método:
1. Cree un archivo por lotes que inicie los programas que desea ejecutar.

   En este ejemplo, cree un archivo por lotes que se inicia el Visor de eventos (Eventvwr.exe) y el Monitor de sistema (Perfmon.exe).  
   - Abra un editor de texto, como el Bloc de notas.
   - Escriba el nombre y ruta de acceso completa al archivo ejecutable para cada programa. En este caso, el archivo incluye las siguientes instrucciones.  
     ```
     C:\Windows\System32\Eventvwr.exe 
     C:\Windows\System32\Perfmon.exe
     ```  
   - Guarde el archivo como MyApps.bat.
2. Use **Schtasks.exe** para crear una tarea que ejecute MyApps.bat.

   El siguiente comando crea la tarea de Monitor, que se ejecuta siempre que cualquier persona inicie sesión. Usa el **/tn** parámetro un nombre de la tarea y el **/tr** parámetro para ejecutar MyApps.bat. Usa el **/sc** parámetro para indicar el tipo de programación al iniciar la sesión y la **/ru** parámetro para ejecutar la tarea con los permisos de la cuenta de usuario administrador.  
   ```
   schtasks /create /tn Monitor /tr C:\MyApps.bat /sc onlogon /ru Reskit\Administrator
   ```  
   Como resultado de este comando, siempre que un usuario inicia sesión en el equipo, iniciar la tarea de Monitor de sistema y de Visor de eventos.

### <a name="BKMK_remote"></a>Para programar una tarea que se ejecuta en un equipo remoto

Para programar una tarea se ejecute en un equipo remoto, debe agregar la tarea a la programación del equipo remoto. Se pueden programar las tareas de todos los tipos en un equipo remoto, pero se deben cumplir las condiciones siguientes.
-   Debe tener permiso para programar la tarea. Por lo tanto, debe haber iniciado sesión en el equipo local con una cuenta que sea miembro del grupo Administradores en el equipo remoto, o debe usar el **/u** parámetro para proporcionar las credenciales de administrador del equipo remoto .
-   Puede usar el **/u** parámetro solo cuando los equipos locales y remotos están en el mismo dominio o el equipo local está en un dominio que confía el dominio del equipo remoto. En caso contrario, el equipo remoto no puede autenticar la cuenta de usuario especificada y no puede comprobar que la cuenta es miembro del grupo Administradores.
-   La tarea debe tener suficientes permisos para ejecutar en el equipo remoto. Los permisos necesarios varían con la tarea. De forma predeterminada, la tarea se ejecuta con el permiso del usuario actual del equipo local o bien, si la **/u** se usa el parámetro, la tarea se ejecuta con el permiso de la cuenta especificada por el **/u** parámetro. Sin embargo, puede usar el **/ru** parámetro para ejecutar la tarea con permisos de una cuenta de usuario diferente o con permisos del sistema.

#### <a name="examples"></a>Ejemplos

#### <a name="an-administrator-schedules-a-task-on-a-remote-computer"></a>Un administrador programa una tarea en un equipo remoto

El siguiente comando programa el programa MyApp para ejecutarse en el equipo remoto SRV01 cada diez días comienza inmediatamente. El comando usa el **/s** parámetro para proporcionar el nombre del equipo remoto. Dado que el usuario actual local es un administrador del equipo remoto, el **/u** parámetro, que proporciona permisos alternativos para programar la tarea, no es necesario.

Tenga en cuenta que al programar tareas en un equipo remoto, todos los parámetros hacen referencia al equipo remoto. Por lo tanto, el archivo ejecutable especificado por el **/tr** parámetro hace referencia a la copia de MyApp.exe en el equipo remoto.
```
schtasks /create /s SRV01 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc daily /mo 10
```
En respuesta, **schtasks** muestra un mensaje de éxito que indica que la tarea está programada.

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-1"></a>Un usuario programa un comando en un equipo remoto (caso 1)

El siguiente comando programa el programa MyApp para ejecutarse en el equipo remoto SRV06 cada tres horas. Dado que se necesitan permisos de administrador para programar una tarea, el comando usa el **/u** y **/p** parámetros para proporcionar las credenciales de administrador del usuario de la cuenta (Admin01 en el Reskits dominio). De forma predeterminada, estos permisos también se usan para ejecutar la tarea. Sin embargo, dado que la tarea no necesita permisos de administrador para ejecutarse, el comando incluye el **/u** y **/rp** parámetros para reemplazar el valor predeterminado y ejecutar la tarea con permiso del usuario cuenta sin privilegios de administrador en el equipo remoto.
```
schtasks /create /s SRV06 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc hourly /mo 3 /u reskits\admin01 /p R43253@4$ /ru SRV06\user03 /rp MyFav!!Pswd
```
En respuesta, **schtasks** muestra un mensaje de éxito que indica que la tarea está programada.

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-2"></a>Un usuario programa un comando en un equipo remoto (caso 2)

El siguiente comando programa el programa MyApp para ejecutarse en el equipo remoto SRV02 el último día de cada mes. Dado que el usuario actual local (user03) no es un administrador del equipo remoto, el comando usa el **/u** parámetro para proporcionar las credenciales de administrador del usuario de la cuenta (Admin01 en el dominio Reskits). Los permisos de cuenta de administrador se usará para programar la tarea y ejecutar la tarea.
```
schtasks /create /s SRV02 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc monthly /mo LASTDAY /m * /u reskits\admin01
```
Dado que el comando no incluía el **/p** parámetro (contraseña), **schtasks** pide la contraseña. A continuación, muestra un mensaje de confirmación y, en este caso, una advertencia.
```
Type the password for reskits\admin01:********

SUCCESS: The scheduled task "My App" has successfully been created.

WARNING: The Scheduled task "My App" has been created, but may not run because
the account information could not be set.
```
Esta advertencia indica que el dominio remoto no pudo autenticar la cuenta especificada por el **/u** parámetro. En este caso, el dominio remoto no pudo autenticar la cuenta de usuario porque el equipo local no es miembro de un dominio que confía el dominio del equipo remoto. Cuando esto ocurre, el trabajo de la tarea aparece en la lista de tareas programadas, pero la tarea está realmente vacía y no se ejecutará.

La siguiente presentación de una consulta detallada expone el problema con la tarea. En la pantalla, tenga en cuenta que el valor de **hora siguiente de ejecución** es **nunca** y que el valor de **ejecutar como usuario** es **no se pudo recuperar del programador de tareas base de datos**.

Este equipo, si hubiera un miembro del mismo dominio o un dominio de confianza, la tarea habría programó correctamente y debían ejecutarse según lo especificado.
```
HostName: SRV44
TaskName: My App
Next Run Time: Never
Status:
Logon mode: Interactive/Background
Last Run Time: Never
Last Result: 0
Creator: user03
Schedule: At 3:52 PM on day 31 of every month, start
 starting 12/14/2001
Task To Run: c:\program files\corpapps\myapp.exe
Start In: myapp.exe
Comment: N/A
Scheduled Task State: Disabled
Scheduled Type: Monthly
Start Time: 3:52:00 PM
Start Date: 12/14/2001
End Date: N/A
Days: 31
Months: JAN,FEB,MAR,APR,MAY,JUN,JUL,AUG,SEP,OCT,NO
V,DEC
Run As User: Could not be retrieved from the task sched
uler database
Delete Task If Not Rescheduled: Enabled
Stop Task If Runs X Hours and X Mins: 72:0
Repeat: Every: Disabled
Repeat: Until: Time: Disabled
Repeat: Until: Duration: Disabled
Repeat: Stop If Still Running: Disabled
Idle Time: Disabled
Power Management: Disabled
```

#### <a name="remarks"></a>Comentarios

-   Para ejecutar un **/ crear** comando con los permisos de un usuario diferente, use el **/u** parámetro. El **/u** parámetro sólo es válido para programar tareas en equipos remotos.
-   ¿Para ver más **schtasks / crear** ejemplos, escriba **schtasks /create /?** en un símbolo del sistema.
-   Para programar una tarea que se ejecuta con permisos de un usuario diferente, utilice el **/ru** parámetro. El **/ru** parámetro es válido para las tareas en equipos locales y remotos.
-   Para usar el **/u** parámetro, el equipo local debe estar en el mismo dominio que el equipo remoto o debe estar en un dominio que confía el dominio del equipo remoto. En caso contrario, no se creó la tarea, o el trabajo de tarea está vacío y no se ejecuta la tarea.
-   **SCHTASKS** siempre solicitará una contraseña, a menos que proporcione uno, incluso cuando se programa una tarea en el equipo local mediante la cuenta de usuario actual. Este comportamiento es normal para **schtasks**.
-   **SCHTASKS** no comprueba las ubicaciones de archivos de programa o las contraseñas de cuentas de usuario. Si no especifica la ubicación del archivo correcto o la contraseña correcta para la cuenta de usuario, se crea la tarea, pero no se ejecuta. Además, si la contraseña de una cuenta cambie o expire, y no cambia la contraseña guardada en la tarea, la tarea no se ejecuta.
-   La cuenta del sistema no tiene derechos de inicio de sesión interactivo. Los usuarios no ven y no pueden interactuar con programas se ejecutan con permisos del sistema.
-   Cada tarea ejecuta un solo programa. Sin embargo, puede crear un archivo por lotes que inicie múltiples tareas y, a continuación, programar una tarea que se ejecuta el archivo por lotes.
-   Puede probar una tarea tan pronto como cree. Use la **ejecutar** operación para probar la tarea y, a continuación, compruebe el archivo SchedLgU.txt (*SystemRoot*\SchedLgU.txt) para los errores.

## <a name="BKMK_change"></a>SCHTASKS cambiar

Cambia una o varias de las siguientes propiedades de una tarea.
-   El programa que se ejecuta la tarea ( **/tr**).
-   La cuenta de usuario que se ejecuta la tarea ( **/ru**).
-   La contraseña de la cuenta de usuario ( **/rp**).
-   Agrega la propiedad sólo interactiva a la tarea ( **/it**).

### <a name="syntax"></a>Sintaxis

```
schtasks /change /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/tr <TaskRun>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/{ENABLE | DISABLE}] [/it] [/z]
```

### <a name="parameters"></a>Parámetros

|          Término           |                                                                                                                                                                                                                                                                                                                                     Definición                                                                                                                                                                                                                                                                                                                                      |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /tn \<TaskName>     |                                                                                                                                                                                                                                                                                                               Identifica la tarea va a cambiar. Escriba el nombre de la tarea.                                                                                                                                                                                                                                                                                                               |
|     /s \<equipo >      |                                                                                                                                                                                                                                                                               Especifica el nombre o dirección IP de un equipo remoto (con o sin barras diagonales inversas). El valor predeterminado es el equipo local.                                                                                                                                                                                                                                                                               |
|  /u [\<Domain>\]<User>  |                                                                                                                                                                 Este comando se ejecuta con los permisos de la cuenta de usuario especificado. El valor predeterminado es el permiso del usuario actual del equipo local. La cuenta de usuario especificada debe ser miembro del grupo Administradores en el equipo remoto. El **/u** y **/p** parámetros solo son válidos para cambiar una tarea en un equipo remoto ( **/s**).                                                                                                                                                                  |
|     /p \<contraseña >      |                                                                                                                                                                                              Especifica la contraseña de la cuenta de usuario especificada en el **/u** parámetro. Si usas el **/u** parámetro, pero omita la **/p** parámetro o el argumento de contraseña, **schtasks** solicitará una contraseña.</br>El **/u** y **/p** parámetros son válidos únicamente cuando usa **/s**.                                                                                                                                                                                               |
| /ru {[\<Domain>\]<User> |                                                                                                                                                                                                                                                                                                                                       Sistema}                                                                                                                                                                                                                                                                                                                                       |
|     /RP \<contraseña >     |                                                                                                                                                                                                                                                 Especifica una nueva contraseña para la cuenta de usuario existente, o la cuenta de usuario especificada por el **/ru** parámetro. Este parámetro se omite con utilizado con la cuenta sistema local.                                                                                                                                                                                                                                                  |
|     /tr \<TaskRun>      |                                                                                                                                                                                  Cambia el programa que se ejecuta la tarea. Escriba la ruta de acceso y el nombre completo de un archivo ejecutable, un archivo de script o un archivo por lotes. Si se omite la ruta de acceso, **schtasks** supone que el archivo está en el \<systemroot > directorio \System32. El programa especificado reemplaza al programa original que ejecuta la tarea.                                                                                                                                                                                  |
|    /ST \<Starttime >     |                                                                                                                                                                                                                                                              Especifica la hora de inicio para la tarea, con el formato de 24 horas, hh: mm. Por ejemplo, un valor de 14:30 equivale a la hora de 12 horas de 2:30 PM.                                                                                                                                                                                                                                                               |
|     /RI \<intervalo >     |                                                                                                                                                                                                                                                                           Especifica el intervalo de repetición para la tarea programada, en minutos. Intervalo válido es 1-599940 (599940 minutos = 9999 horas).                                                                                                                                                                                                                                                                            |
|     /et \<EndTime>      |                                                                                                                                                                                                                                                               Especifica la hora de finalización de la tarea, con el formato de 24 horas, hh: mm. Por ejemplo, un valor de 14:30 equivale a la hora de 12 horas de 2:30 PM.                                                                                                                                                                                                                                                                |
|     /du \<duración >     |                                                                                                                                                                                                                                                                                                     Especifica que se cierre la tarea en el \<EndTime > o <Duration>, si se especifica.                                                                                                                                                                                                                                                                                                      |
|           /k            |                                                                                                                                                                   Detiene el programa que se ejecuta la tarea en el momento especificado por **/et** o **/du**. Sin **/k**, **schtasks** no se inicia el programa de nuevo una vez que llega la hora especificada por **/et** o **/du**, pero no se detiene el programa si aún se está ejecutando. Este parámetro es opcional y válido sólo con una programación por horas o minutos.                                                                                                                                                                   |
|    / SD \<StartDate >     |                                                                                                                                                                                                                                                                                              Especifica la primera fecha en que se debe ejecutar la tarea. El formato de fecha es MM/DD/AAAA.                                                                                                                                                                                                                                                                                               |
|     /Ed \<EndDate >      |                                                                                                                                                                                                                                                                                                 Especifica la última fecha en que se debe ejecutar la tarea. El formato es MM/DD/AAAA.                                                                                                                                                                                                                                                                                                  |
|         /ENABLE         |                                                                                                                                                                                                                                                                                                                       Especifica que se habilite la tarea programada.                                                                                                                                                                                                                                                                                                                       |
|        / DESHABILITAR         |                                                                                                                                                                                                                                                                                                                      Especifica que se deshabilite la tarea programada.                                                                                                                                                                                                                                                                                                                       |
|           /it           | Especifica que se ejecute la tarea programada solo cuando el usuario "Ejecutar como" (la cuenta de usuario que se ejecuta la tarea) ha iniciado sesión en el equipo.</br>Este parámetro no tiene ningún efecto en tareas que se ejecutan con permisos del sistema ni tareas que ya tienen establecida la propiedad sólo interactiva. No se puede utilizar un comando de cambio para quitar la propiedad sólo interactiva de una tarea.</br>De forma predeterminada, el usuario "Ejecutar como" es el usuario actual del equipo local cuando se programa la tarea o la cuenta especificada por el **/u** parámetro, si se usa alguno. Sin embargo, si el comando incluye el **/ru** parámetro, a continuación, el usuario "Ejecutar como" es la cuenta especificada por el **/ru** parámetro. |
|           /z            |                                                                                                                                                                                                                                                                                                          Especifica que se elimine la tarea cuando se complete su programación.                                                                                                                                                                                                                                                                                                          |
|           /?            |                                                                                                                                                                                                                                                                                                                        Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                                                                                                                                                                         |

### <a name="remarks"></a>Comentarios

-   El **/tn** y **/s** parámetros identifican la tarea. El **/tr**, **/ru**, y **/rp** los parámetros especifican las propiedades de la tarea que se puede cambiar.
-   El **/ru**, y **/rp** los parámetros especifican los permisos que se ejecuta la tarea. El **/u** y **/p** los parámetros especifican los permisos utilizados para cambiar la tarea.
-   Para cambiar las tareas en un equipo remoto, el usuario debe ser iniciado sesión en el equipo local con una cuenta que sea miembro del grupo Administradores en el equipo remoto.
-   Para ejecutar un **/cambiar** comando con los permisos de un usuario diferente ( **/u**, **/p**), el equipo local debe estar en el mismo dominio que el equipo remoto o debe estar en un dominio que confía el dominio del equipo remoto.
-   La cuenta del sistema no tiene derechos de inicio de sesión interactivo. Los usuarios no ven y no pueden interactuar con programas se ejecutan con permisos del sistema.
-   Para identificar las tareas con el **/it** propiedad, utilice una consulta detallada ( **/consulta /v**). En una presentación de la consulta detallada de una tarea con **/it**, **modo de inicio de sesión** campo tiene un valor de **sólo interactivo**.

### <a name="examples"></a>Ejemplos

### <a name="to-change-the-program-that-a-task-runs"></a>Para cambiar el programa que se ejecuta una tarea

El siguiente comando cambia el programa que se ejecuta la tarea de comprobación de Virus de VirusCheck.exe a VirusCheck2.exe. Este comando usa el **/tn** parámetro para identificar la tarea y el **/tr** parámetro para especificar el nuevo programa para la tarea. (No se puede cambiar el nombre de tarea).
```
schtasks /change /tn "Virus Check" /tr C:\VirusCheck2.exe
```
En respuesta, **SchTasks.exe** muestra el siguiente mensaje de éxito:
```
SUCCESS: The parameters of the scheduled task "Virus Check" have been changed.
```
Como resultado de este comando, la tarea de comprobación de Virus ejecuta ahora VirusCheck2.exe.

### <a name="to-change-the-password-for-a-remote-task"></a>Para cambiar la contraseña de una tarea remota

El siguiente comando cambia la contraseña de la cuenta de usuario para la tarea RemindMe en el equipo remoto, Svr01. El comando usa el **/tn** parámetro para identificar la tarea y el **/s** parámetro para especificar el equipo remoto. Usa el **/rp** parámetro para especificar la nueva contraseña, p@ssWord3.

Este procedimiento es necesario siempre que la contraseña de una cuenta de usuario caduca o se cambia. Si la contraseña guardada en una tarea ya no es válida, no ejecute la tarea.
```
schtasks /change /tn RemindMe /s Svr01 /rp p@ssWord3
```
En respuesta, **SchTasks.exe** muestra el siguiente mensaje de éxito:
```
SUCCESS: The parameters of the scheduled task "RemindMe" have been changed.
```
Como resultado de este comando, la tarea RemindMe se ejecuta ahora en su cuenta de usuario original, pero con una contraseña nueva.

### <a name="to-change-the-program-and-user-account-for-a-task"></a>Para cambiar la cuenta de usuario y el programa para una tarea

El siguiente comando cambia el programa que se ejecuta una tarea y los cambios de la cuenta de usuario con la que se ejecuta la tarea. Básicamente, utiliza una programación antigua para una nueva tarea. Este comando cambia la tarea ChkNews, que inicia Notepad.exe cada mañana a las 9:00 A.M., para iniciar Internet Explorer en su lugar.

El comando usa el **/tn** parámetro para identificar la tarea. Usa el **/tr** parámetro para cambiar el programa que se ejecuta la tarea y el **/ru** parámetro para cambiar la cuenta de usuario que se ejecuta la tarea.

El **/ru**, y **/rp** parámetro, que proporciona la contraseña de la cuenta de usuario, se omite. Debe proporcionar una contraseña para la cuenta, pero puede usar el **/ru**, y **/rp** parámetro y escriba la contraseña en texto sin cifrar o esperar **SchTasks.exe** que solicite una contraseña y, a continuación, escriba la contraseña en texto oculto.
```
schtasks /change /tn ChkNews /tr "c:\program files\Internet Explorer\iexplore.exe" /ru DomainX\Admin01
```
En respuesta, **SchTasks.exe** solicita la contraseña para la cuenta de usuario. Oculta el texto que escriba, por lo que la contraseña no es visible.
```
Please enter the password for DomainX\Admin01: 
```
Tenga en cuenta que el **/tn** parámetro identifica la tarea y que la **/tr** y **/ru** parámetros cambian las propiedades de la tarea. No se puede usar otro parámetro para identificar la tarea y no se puede cambiar el nombre de la tarea.

En respuesta, **SchTasks.exe** muestra el siguiente mensaje de éxito:
```
SUCCESS: The parameters of the scheduled task "ChkNews" have been changed.
```
Como resultado de este comando, la tarea ChkNews ahora ejecuta Internet Explorer con los permisos de una cuenta de administrador.

### <a name="to-change-a-program-to-the-system-account"></a>Para cambiar un programa a la cuenta del sistema

El siguiente comando cambia la tarea SecurityScript para que se ejecute con permisos de la cuenta del sistema. Usa el **/ru ""** parámetro para indicar la cuenta del sistema.
```
schtasks /change /tn SecurityScript /ru ""
```
En respuesta, **SchTasks.exe** muestra el siguiente mensaje de éxito:
```
INFO: The run as user name for the scheduled task "SecurityScript" will be changed to "NT AUTHORITY\SYSTEM".
SUCCESS: The parameters of the scheduled task "SecurityScript" have been changed.
```
Dado que las tareas que se ejecute con permisos de cuenta de sistema no requieren una contraseña, **SchTasks.exe** no solicita uno.

### <a name="to-run-a-program-only-when-i-am-logged-on"></a>Para ejecutar un programa solo cuando el usuario ha iniciado sesión

El siguiente comando agrega la propiedad sólo interactiva a MyApp, una tarea existente. Esta propiedad garantiza que la tarea se ejecuta solo cuando el usuario "Ejecutar como", es decir, la cuenta de usuario en el que se ejecuta la tarea, se ha iniciado sesión en el equipo.

El comando usa el **/tn** parámetro para identificar la tarea y el **/it** parámetro para agregar la propiedad sólo interactiva a la tarea. Dado que la tarea ya se ejecuta con los permisos de mi cuenta de usuario, no es necesario cambiar la **/ru** parámetro para la tarea.
```
schtasks /change /tn MyApp /it
```
En respuesta, **SchTasks.exe** muestra el siguiente mensaje de éxito.
```
SUCCESS: The parameters of the scheduled task "MyApp" have been changed.
```

## <a name="BKMK_run"></a>schtasks run

Se inicia una tarea programada inmediatamente. El **ejecutar** operación omite la programación, pero utiliza la ubicación del archivo de programa, la cuenta de usuario y la contraseña guardada en la tarea para ejecutar la tarea inmediatamente.

### <a name="syntax"></a>Sintaxis

```
schtasks /run /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Parámetros

|         Término          |                                                                                                                                                                 Definición                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /tn \<TaskName>    |                                                                                                                                                       Obligatorio. Identifica la tarea.                                                                                                                                                        |
|    /s \<equipo >     |                                                                                                           Especifica el nombre o dirección IP de un equipo remoto (con o sin barras diagonales inversas). El valor predeterminado es el equipo local.                                                                                                           |
| /u [\<Domain>\]<User> | Este comando se ejecuta con los permisos de la cuenta de usuario especificado. De forma predeterminada, el comando se ejecuta con los permisos del usuario actual del equipo local.</br>La cuenta de usuario especificada debe ser miembro del grupo Administradores en el equipo remoto. El **/u** y **/p** parámetros son válidos únicamente cuando usa **/s**. |
|    /p \<contraseña >     |                          Especifica la contraseña de la cuenta de usuario especificada en el **/u** parámetro. Si usas el **/u** parámetro, pero omita la **/p** parámetro o el argumento de contraseña, **schtasks** solicitará una contraseña.</br>El **/u** y **/p** parámetros son válidos únicamente cuando usa **/s**.                           |
|          /?           |                                                                                                                                                    Muestra la ayuda en el símbolo del sistema.                                                                                                                                                     |

### <a name="remarks"></a>Comentarios

-   Use esta operación para probar las tareas. Si no se ejecuta una tarea, compruebe el registro de transacciones del servicio Programador de tareas, \<Systemroot > \SchedLgU.txt errores.
-   Ejecución de una tarea no afecta a la programación de tareas y no cambia la próxima ejecución programada para la tarea.
-   Para ejecutar una tarea de forma remota, se debe programar la tarea en el equipo remoto. Cuando se ejecuta, la tarea se ejecuta solo en el equipo remoto. Para comprobar que se está ejecutando una tarea en un equipo remoto, use el Administrador de tareas o el registro de transacciones del programador de tareas, \<Systemroot > \SchedLgU.txt.

### <a name="examples"></a>Ejemplos

### <a name="to-run-a-task-on-the-local-computer"></a>Para ejecutar una tarea en el equipo local

El comando siguiente inicia la tarea "Security Script".
```
schtasks /run /tn "Security Script"
```
En respuesta, **SchTasks.exe** el script asociado con la tarea se inicia y muestra el mensaje siguiente:
```
SUCCESS: Attempted to run the scheduled task "Security Script".
```
Como indica el mensaje, **schtasks** intenta iniciar el programa, pero no puede comprobar que realmente inicia el programa.

### <a name="to-run-a-task-on-a-remote-computer"></a>Para ejecutar una tarea en un equipo remoto

El comando siguiente inicia la tarea de actualización en un equipo remoto, Svr01:
```
schtasks /run /tn Update /s Svr01
```
En este caso, **SchTasks.exe** muestra el mensaje de error siguiente:
```
ERROR: Unable to run the scheduled task "Update".
```
Para encontrar la causa del error, busque en el registro de transacciones de las tareas programadas, C:\Windows\SchedLgU.txt en Svr01. En este caso, la siguiente entrada aparece en el registro:
```
"Update.job" (update.exe) 3/26/2001 1:15:46 PM ** ERROR **
The attempt to log on to the account associated with the task failed, therefore, the task did not run.
The specific error is:
0x8007052e: Logon failure: unknown user name or bad password.
Verify that the task's Run-as name and password are valid and try again.
```
Aparentemente, el nombre de usuario o la contraseña en la tarea no es válido en el sistema. La siguiente **schtasks /change** comando actualiza el nombre de usuario y la contraseña para la tarea de actualización en Svr01:
```
schtasks /change /tn Update /s Svr01 /ru Administrator /rp PassW@rd3
```
Después de la **cambiar** comando finalice, el **ejecutar** se repite el comando. Esta vez, se inicia el programa Update.exe y **SchTasks.exe** muestra el mensaje siguiente:
```
SUCCESS: Attempted to run the scheduled task "Update".
```
Como indica el mensaje, **schtasks** intenta iniciar el programa, pero no puede comprobar que realmente inicia el programa.

## <a name="BKMK_end"></a>schtasks end

Detiene un programa iniciado por una tarea.

### <a name="syntax"></a>Sintaxis

```
schtasks /end /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Parámetros

|         Término          |                                                                                                                                                               Definición                                                                                                                                                                |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /tn \<TaskName>    |                                                                                                                                         Obligatorio. Identifica la tarea que se inició el programa.                                                                                                                                         |
|    /s \<equipo >     |                                                                                                                        Especifica el nombre o dirección IP de un equipo remoto. El valor predeterminado es el equipo local.                                                                                                                        |
| /u [\<Domain>\]<User> | Este comando se ejecuta con los permisos de la cuenta de usuario especificado. De forma predeterminada, el comando se ejecuta con los permisos del usuario actual del equipo local. La cuenta de usuario especificada debe ser miembro del grupo Administradores en el equipo remoto. El **/u** y **/p** parámetros son válidos únicamente cuando usa **/s**. |
|    /p \<contraseña >     |                        Especifica la contraseña de la cuenta de usuario especificada en el **/u** parámetro. Si usas el **/u** parámetro, pero omita la **/p** parámetro o el argumento de contraseña, **schtasks** solicitará una contraseña.</br>El **/u** y **/p** parámetros son válidos únicamente cuando usa **/s**.                         |
|          /?           |                                                                                                                                                             Muestra la Ayuda.                                                                                                                                                              |

### <a name="remarks"></a>Comentarios

**SchTasks.exe** finaliza solo las instancias de un programa iniciado por una tarea programada. Para detener otros procesos, use TaskKill. Para obtener más información, consulte [Taskkill](taskkill.md).

### <a name="examples"></a>Ejemplos

### <a name="to-end-a-task-on-a-local-computer"></a>Para finalizar una tarea en un equipo local

El siguiente comando detiene la instancia de Notepad.exe que inició la tarea de Mi Bloc de notas:
```
schtasks /end /tn "My Notepad"
```
En respuesta, **SchTasks.exe** detiene la instancia de Notepad.exe que inició la tarea, y muestra el siguiente mensaje de confirmación:
```
SUCCESS: The scheduled task "My Notepad" has been terminated successfully.
```

### <a name="to-end-a-task-on-a-remote-computer"></a>Para finalizar una tarea en un equipo remoto

El siguiente comando detiene la instancia de Internet Explorer que se inició la tarea InternetOn en el equipo remoto, Svr01:
```
schtasks /end /tn InternetOn /s Svr01
```
En respuesta, **SchTasks.exe** detiene la instancia de Internet Explorer que inició la tarea, y muestra el siguiente mensaje de confirmación:
```
SUCCESS: The scheduled task "InternetOn" has been terminated successfully.
```

## <a name="BKMK_delete"></a>schtasks delete

Elimina una tarea programada.

### <a name="syntax"></a>Sintaxis

```
schtasks /delete /tn {<TaskName> | *} [/f] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Parámetros

|         Término          |                                                                                                                                                                 Definición                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   /tn {\<TaskName>    |                                                                                                                                                                     \*}                                                                                                                                                                     |
|          /f           |                                                                                                                                  Suprime el mensaje de confirmación. La tarea se elimina sin previo aviso.                                                                                                                                  |
|    /s \<equipo >     |                                                                                                           Especifica el nombre o dirección IP de un equipo remoto (con o sin barras diagonales inversas). El valor predeterminado es el equipo local.                                                                                                           |
| /u [\<Domain>\]<User> | Este comando se ejecuta con los permisos de la cuenta de usuario especificado. De forma predeterminada, el comando se ejecuta con los permisos del usuario actual del equipo local.</br>La cuenta de usuario especificada debe ser miembro del grupo Administradores en el equipo remoto. El **/u** y **/p** parámetros son válidos únicamente cuando usa **/s**. |
|    /p \<contraseña >     |                          Especifica la contraseña de la cuenta de usuario especificada en el **/u** parámetro. Si usas el **/u** parámetro, pero omita la **/p** parámetro o el argumento de contraseña, **schtasks** solicitará una contraseña.</br>El **/u** y **/p** parámetros son válidos únicamente cuando usa **/s**.                           |
|          /?           |                                                                                                                                                    Muestra la ayuda en el símbolo del sistema.                                                                                                                                                     |

### <a name="remarks"></a>Comentarios

- El **eliminar** operación elimina la tarea de la programación. No elimine el programa que se ejecuta la tarea ni interrumpir un programa en ejecución.
- El **eliminar \\** * comando elimina todas las tareas programadas en el equipo, no solo las tareas programadas por el usuario actual.

### <a name="examples"></a>Ejemplos

### <a name="to-delete-a-task-from-the-schedule-of-a-remote-computer"></a>Para eliminar una tarea de la programación de un equipo remoto

El siguiente comando elimina la tarea "Iniciar correo electrónico" de la programación de un equipo remoto. Usa el **/s** parámetro para identificar el equipo remoto.
```
schtasks /delete /tn "Start Mail" /s Svr16
```
En respuesta, **SchTasks.exe** muestra el siguiente mensaje de confirmación. Para eliminar la tarea, presione s<strong>.</strong> Para cancelar el comando, escriba **n**:
```
WARNING: Are you sure you want to remove the task "Start Mail" (Y/N )? 
SUCCESS: The scheduled task "Start Mail" was successfully deleted.
```

### <a name="to-delete-all-tasks-scheduled-for-the-local-computer"></a>Para eliminar todas las tareas programadas para el equipo local

El siguiente comando elimina todas las tareas de la programación del equipo local, incluidas las tareas programadas por otros usuarios. Usa el **/tn \\** * parámetro para representar todas las tareas en el equipo y el **/f** parámetro para suprimir el mensaje de confirmación.
```
schtasks /delete /tn * /f
```
En respuesta, **SchTasks.exe** muestra los siguientes mensajes de éxito que indica que la única tarea programada, SecureScript, se elimina.

`SUCCESS: The scheduled task "SecureScript" was successfully deleted.`

## <a name="BKMK_query"></a>SCHTASKS consulta

Muestra las tareas programadas para ejecutarse en el equipo.

### <a name="syntax"></a>Sintaxis

```
schtasks [/query] [/fo {TABLE | LIST | CSV}] [/nh] [/v] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Parámetros

|         Término          |                                                                                                                                                                 Definición                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       [/query]        |                                                                                                                        El nombre de la operación es opcional. Escriba **schtasks** sin ningún parámetro realiza una consulta.                                                                                                                         |
|      /fo {TABLE       |                                                                                                                                                                    LISTA                                                                                                                                                                     |
|          /nh          |                                                                                                            Omite los encabezados de columna de la presentación de la tabla. Este parámetro es válido con el **tabla** y **CSV** formatos de salida.                                                                                                             |
|          /v           |                                                                                                         Propiedades avanzadas de las tareas se agrega a la presentación.</br>Las consultas que utilizan **/v** debe tener el formato **lista** o **CSV**.                                                                                                          |
|    /s \<equipo >     |                                                                                                           Especifica el nombre o dirección IP de un equipo remoto (con o sin barras diagonales inversas). El valor predeterminado es el equipo local.                                                                                                           |
| /u [\<Domain>\]<User> | Este comando se ejecuta con los permisos de la cuenta de usuario especificado. De forma predeterminada, el comando se ejecuta con los permisos del usuario actual del equipo local.</br>La cuenta de usuario especificada debe ser miembro del grupo Administradores en el equipo remoto. El **/u** y **/p** parámetros son válidos únicamente cuando usa **/s**. |
|    /p \<contraseña >     |                                        Especifica la contraseña de la cuenta de usuario especificada en el **/u** parámetro. Si usas **/u**, pero omita **/p** o el argumento de contraseña, **schtasks** solicitará una contraseña.</br>El **/u** y **/p** parámetros son válidos únicamente cuando usa **/s**.                                         |
|          /?           |                                                                                                                                                    Muestra la ayuda en el símbolo del sistema.                                                                                                                                                     |

### <a name="remarks"></a>Comentarios

**SchTasks.exe** finaliza solo las instancias de un programa iniciado por una tarea programada. Para detener otros procesos, use TaskKill. Para obtener más información, consulte [Taskkill](taskkill.md).

### <a name="examples"></a>Ejemplos

### <a name="to-display-the-scheduled-tasks-on-the-local-computer"></a>Para mostrar las tareas programadas en el equipo local

Los comandos siguientes muestran todas las tareas programadas para el equipo local. Estos comandos producen el mismo resultado y se pueden usar indistintamente.
```
schtasks
schtasks /query
```
En respuesta, **SchTasks.exe** muestra las tareas en el valor predeterminado, el formato de tabla simple, tal como se muestra en la tabla siguiente:
```
TaskName Next Run Time Status
========================= ======================== ==============
Microsoft Outlook At logon time
SecureScript 14:42:00 PM , 2/4/2001
```

### <a name="to-display-advanced-properties-of-scheduled-tasks"></a>Para mostrar propiedades avanzadas de las tareas programadas

El siguiente comando solicita una presentación detallada de las tareas en el equipo local. Usa el **/v** parámetro para solicitar una presentación detallada (detallada) y el **/fo lista** parámetro para formatear la visualización como una lista para facilitar la lectura. Puede usar este comando para comprobar que una tarea que ha creado tiene el patrón de periodicidad deseada.

**schtasks /query /fo LIST /v**

En respuesta, **SchTasks.exe** muestra una lista detallada de las propiedades para todas las tareas. La siguiente pantalla muestra la lista de tareas para una tarea programada para ejecutarse a las 4:00 A.M. en el último viernes de cada mes:
```
HostName: RESKIT01
TaskName: SecureScript
Next Run Time: 4:00:00 AM , 3/30/2001
Status: Not yet run
Logon mode: Interactive/Background
Last Run Time: Never
Last Result: 0
Creator: user01
Schedule: At 4:00 AM on the last Fri of every month, starting 3/24/2001
Task To Run: C:\WINDOWS\system32\notepad.exe
Start In: notepad.exe
Comment: N/A
Scheduled Task State: Enabled
Scheduled Type: Monthly
Modifier: Last FRIDAY
Start Time: 4:00:00 AM
Start Date: 3/24/2001
End Date: N/A
Days: FRIDAY
Months: JAN,FEB,MAR,APR,MAY,JUN,JUL,AUG,SEP,OCT,NOV,DEC
Run As User: RESKIT\user01
Delete Task If Not Rescheduled: Enabled
Stop Task If Runs X Hours and X Mins: 72:0
Repeat: Until Time: Disabled
Repeat: Duration: Disabled
Repeat: Stop If Still Running: Disabled
Idle: Start Time(For IDLE Scheduled Type): Disabled
Idle: Only Start If Idle for X Minutes: Disabled
Idle: If Not Idle Retry For X Minutes: Disabled
Idle: Stop Task If Idle State End: Disabled
Power Mgmt: No Start On Batteries: Disabled
Power Mgmt: Stop On Battery Mode: Disabled
```

### <a name="to-log-tasks-scheduled-for-a-remote-computer"></a>Para iniciar tareas programadas en un equipo remoto

El siguiente comando solicita una lista de tareas programadas en un equipo remoto y agrega las tareas a un archivo de registro separados por comas en el equipo local. Puede usar este formato de comando para recopilar y realizar un seguimiento de las tareas programadas para varios equipos.

El comando usa el **/s** parámetro para identificar el equipo remoto, Reskit16, el **/fo** parámetro para especificar el formato y la **/nh** parámetro para suprimir la columna títulos. El **>>** anexar la salida de redirecciones de símbolos en el registro de tareas, p0102.csv, en el equipo local, Svr01. Dado que el comando se ejecuta en el equipo remoto, la ruta de acceso del equipo local debe ser completo.
```
schtasks /query /s Reskit16 /fo csv /nh >> \\svr01\data\tasklogs\p0102.csv
```
En respuesta, **SchTasks.exe** agrega las tareas programadas en el equipo Reskit16 al archivo en el equipo local, Svr01 p0102.csv.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
