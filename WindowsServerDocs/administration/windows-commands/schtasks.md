---
title: schtasks
description: Artículo de referencia de * * * *-
ms.topic: reference
ms.assetid: 2e713203-3dd8-491b-b9e1-9423618dc7e8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9710e0b8156ae6a09302c3ffdeaea3e48e7953d1
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637038"
---
# <a name="schtasks"></a>schtasks



Programa comandos y programas para que se ejecuten periódicamente o en un momento específico. Agrega y quita tareas de la programación, inicia y detiene las tareas a petición, y muestra y cambia las tareas programadas.

Para ver la sintaxis del comando, haga clic en uno de los siguientes comandos:
-   [crear SchTasks](#BKMK_create)
-   [cambio de SchTasks](#BKMK_change)
-   [ejecución de SchTasks](#BKMK_run)
-   [fin de SchTasks](#BKMK_end)
-   [eliminar SchTasks](#BKMK_delete)
-   [Schtasks (consulta)](#BKMK_query)

## <a name="remarks"></a>Observaciones

- **SchTasks.exe** realiza las mismas operaciones que **las tareas programadas** en el **Panel de control**. Puede usar estas herramientas juntas e indistintamente.
- **SchTasks** reemplaza **At.exe**, una herramienta incluida en versiones anteriores de Windows. Aunque **At.exe** todavía está incluido en la familia de Windows Server 2003, **Schtasks** es la herramienta de programación de tareas de línea de comandos recomendada.
- Los parámetros de un comando **SchTasks** pueden aparecer en cualquier orden. Al escribir **SchTasks** sin ningún parámetro, se realiza una consulta.
- Permisos para **SchTasks**
  -   Debe tener permiso para ejecutar el comando. Cualquier usuario puede programar una tarea en el equipo local y puede ver y cambiar las tareas programadas. Los miembros del grupo administradores pueden programar, ver y cambiar todas las tareas en el equipo local.
  -   Para programar, ver o cambiar una tarea en un equipo remoto, debe ser miembro del grupo administradores en el equipo remoto o debe usar el parámetro **/u** para proporcionar las credenciales de un administrador del equipo remoto.
  -   Puede usar el parámetro **/u** en una operación **/Create** o **/Change** solo cuando los equipos locales y remotos están en el mismo dominio o el equipo local se encuentra en un dominio en el que confía el dominio del equipo remoto. De lo contrario, el equipo remoto no puede autenticar la cuenta de usuario especificada y no puede comprobar que la cuenta es miembro del grupo administradores.
  -   La tarea debe tener permiso para ejecutarse. Los permisos necesarios varían con la tarea. De forma predeterminada, las tareas se ejecutan con los permisos del usuario actual del equipo local o con los permisos del usuario especificado mediante el parámetro **/u** , si se incluye alguno. Para ejecutar una tarea con permisos de una cuenta de usuario diferente o con permisos del sistema, use el parámetro **/RU** .
- Para comprobar que una tarea programada se ejecutó o averiguar por qué no se ejecutó una tarea programada, consulte el registro de transacciones del servicio de Programador de tareas, *SystemRoot*\SchedLgU.txt. Este registro registra las ejecuciones iniciadas por todas las herramientas que usan el servicio, incluidas **las tareas programadas** y **SchTasks.exe**.
- En raras ocasiones, los archivos de tarea se dañan. Las tareas dañadas no se ejecutan. Al intentar realizar una operación en tareas dañadas, **SchTasks.exe** muestra el siguiente mensaje de error:
  ```
  ERROR: The data is invalid.
  ```
  No se pueden recuperar tareas dañadas. Para restaurar las características de programación de tareas del sistema, use **SchTasks.exe** o **tareas programadas** para eliminar las tareas del sistema y volver a programarlas.

## <a name="schtasks-create"></a><a name=BKMK_create></a>crear SchTasks

Programa una tarea.

**SchTasks** usa diferentes combinaciones de parámetros para cada tipo de programación. Para ver la sintaxis combinada para crear tareas o para ver la sintaxis para crear una tarea con un tipo de programación determinado, haga clic en una de las siguientes opciones.
-   [Sintaxis combinada y descripciones de parámetros](#BKMK_syntax)
-   [Para programar una tarea que se ejecuta cada N minutos](#BKMK_minutes)
-   [Para programar una tarea que se ejecuta cada N horas](#BKMK_hours)
-   [Para programar una tarea que se ejecuta cada N días](#BKMK_days)
-   [Para programar una tarea que se ejecuta cada N semanas](#BKMK_weeks)
-   [Para programar una tarea que se ejecuta cada N meses](#BKMK_months)
-   [Para programar una tarea que se ejecuta en un día concreto de la semana](#BKMK_spec_day)
-   [Para programar una tarea que se ejecuta en una semana específica del mes](#BKMK_spec_week)
-   [Para programar una tarea que se ejecuta en una fecha específica cada mes](#BKMK_spec_date)
-   [Para programar una tarea que se ejecute el último día de un mes](#BKMK_last_day)
-   [Para programar una tarea que se ejecuta una vez](#BKMK_once)
-   [Para programar una tarea que se ejecuta cada vez que se inicia el sistema](#BKMK_startup)
-   [Para programar una tarea que se ejecuta cuando un usuario inicia sesión](#BKMK_logon)
-   [Para programar una tarea que se ejecuta cuando el sistema está inactivo](#BKMK_idle)
-   [Para programar una tarea que se ejecuta ahora](#BKMK_now)
-   [Para programar una tarea que se ejecuta con permisos diferentes](#BKMK_diff_perms)
-   [Para programar una tarea que se ejecuta con permisos del sistema](#BKMK_sys_perms)
-   [Para programar una tarea que ejecuta más de un programa](#BKMK_multi_progs)
-   [Para programar una tarea que se ejecuta en un equipo remoto](#BKMK_remote)

### <a name="combined-syntax-and-parameter-descriptions"></a><a name=BKMK_syntax></a>Sintaxis combinada y descripciones de parámetros

#### <a name="syntax"></a>Sintaxis

```
schtasks /create /sc <ScheduleType> /tn <TaskName> /tr <TaskRun> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/mo <Modifier>] [/d <Day>[,<Day>...] | *] [/m <Month>[,<Month>...]] [/i <IdleTime>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/it] [/z] [/f]
```

##### <a name="parameters"></a>Parámetros

##### <a name="sc-scheduletype"></a>once \<ScheduleType>

Especifica el tipo de programación. Los valores válidos son minuto, cada hora, diariamente, SEMANALmente, MENSUALmente, una vez, OnStart, ONLOGON, OnIdle.

|Tipo de programación|Descripción|
|-------------|-----------|
|MINUTO, CADA HORA, DIARIAMENTE, SEMANALMENTE, MENSUALMENTE|Especifica la unidad de tiempo para la programación.|
|UNA vez|La tarea se ejecuta una vez en una fecha y hora especificadas.|
|ONSTART|La tarea se ejecuta cada vez que se inicia el sistema. Puede especificar una fecha de inicio o ejecutar la tarea la próxima vez que se inicie el sistema.|
|AL iniciar|La tarea se ejecuta cada vez que un usuario (cualquier usuario) inicia sesión. Puede especificar una fecha o ejecutar la tarea la próxima vez que el usuario inicie sesión.|
|ONIDLE|La tarea se ejecuta siempre que el sistema está inactivo durante un período de tiempo especificado. Puede especificar una fecha o ejecutar la tarea la próxima vez que el sistema esté inactivo.|

##### <a name="tn-taskname"></a>/TN \<TaskName>

Especifica un nombre para la tarea. Cada tarea del sistema debe tener un nombre único. El nombre debe cumplir las reglas de los nombres de archivo y no debe superar los 238 caracteres. Utilice comillas para encerrar los nombres que incluyen espacios.

##### <a name="tr-taskrun"></a>/TR \<TaskRun>

Especifica el programa o comando que ejecuta la tarea. Escriba la ruta de acceso completa y el nombre de archivo de un archivo ejecutable, un archivo de script o un archivo por lotes. El nombre de la ruta de acceso no debe superar los 262 caracteres. Si omite la ruta de acceso, **SchTasks** supone que el archivo se encuentra en el directorio *systemroot*\System32

##### <a name="s-computer"></a>modificado \<Computer>

Programa una tarea en el equipo remoto especificado. Escriba el nombre o la dirección IP de un equipo remoto (con o sin barras diagonales inversas). La opción predeterminada es el equipo local. Los parámetros **/u** y **/p** solo son válidos cuando se utiliza **/s**.

##### <a name="u-domainuser"></a>5.50\<Domain>\]<User>

Ejecuta este comando con los permisos de la cuenta de usuario especificada. El valor predeterminado son los permisos del usuario actual del equipo local. Los parámetros **/u** y **/p** solo son válidos para programar una tarea en un equipo remoto (**/s**).

Los permisos de la cuenta especificada se usan para programar la tarea y para ejecutar la tarea. Para ejecutar la tarea con los permisos de otro usuario, use el parámetro **/RU** .

La cuenta de usuario debe ser miembro del grupo administradores en el equipo remoto. Además, el equipo local debe estar en el mismo dominio que el equipo remoto, o bien debe estar en un dominio que sea de confianza para el dominio del equipo remoto.

##### <a name="p-password"></a>/p \<Password>

Proporciona la contraseña de la cuenta de usuario especificada en el parámetro **/u** . Si usa el parámetro **/u** , pero omite el parámetro **/p** o el argumento de contraseña, **Schtasks** le pedirá una contraseña y ocultará el texto que escriba.

Los parámetros **/u** y **/p** solo son válidos para programar una tarea en un equipo remoto (**/s**).

##### <a name="ru-domainuser--system"></a>/ru {[ \<Domain> \] <User> | Integrado

Ejecuta la tarea con los permisos de la cuenta de usuario especificada. De forma predeterminada, la tarea se ejecuta con los permisos del usuario actual del equipo local o con el permiso del usuario especificado mediante el parámetro **/u** , si se incluye uno. El parámetro **/RU** es válido cuando se programan tareas en equipos locales o remotos.


|       Value        |                                                    Descripción                                                    |
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| [\<Domain>\]<User> |                                       Especifica una cuenta de usuario alternativa.                                        |
|    Sistema o     | Especifica la cuenta de sistema local, una cuenta con privilegios elevados usada por el sistema operativo y los servicios del sistema. |

##### <a name="rp-password"></a>/RP \<Password>

Proporciona la contraseña de la cuenta de usuario que se especifica en el parámetro **/RU** . Si omite este parámetro al especificar una cuenta de usuario, **SchTasks.exe** le pedirá la contraseña y ocultará el texto que escriba.

No use el parámetro **/RP** para las tareas que se ejecutan con las credenciales de la cuenta del sistema (**/RU System**). La cuenta del sistema no tiene una contraseña y **SchTasks.exe** no solicita una.

##### <a name="mo-modifier"></a>/mes \<Modifier>

Especifica la frecuencia con que se ejecuta la tarea dentro de su tipo de programación. Este parámetro es válido, pero opcional, para una programación de minuto, hora, diaria, semanal y mensual. El valor predeterminado es 1.

|Tipo de programación|Valores modificadores|Descripción|
|-------------|---------------|-----------|
|MINUTE|1 - 1439|La tarea se ejecuta cada \<N> minutos.|
|POR hora|1 - 23|La tarea se ejecuta cada \<N> horas.|
|DIARIAMENTE|1 - 365|La tarea se ejecuta cada \<N> días.|
|SEMANA|1 - 52|La tarea se ejecuta cada \<N> semana.|
|UNA vez|Sin modificadores.|La tarea se ejecuta una vez.|
|ONSTART|Sin modificadores.|La tarea se ejecuta en el inicio.|
|AL iniciar|Sin modificadores.|La tarea se ejecuta cuando el usuario especificado por el parámetro **/u** inicia sesión.|
|ONIDLE|Sin modificadores.|La tarea se ejecuta después de que el sistema esté inactivo durante el número de minutos especificado por el parámetro **/i** , que es necesario para su uso con OnIdle.|
|MENSUALMENTE|1 - 12|La tarea se ejecuta cada \<N> meses.|
|MENSUALMENTE|LASTDAY|La tarea se ejecuta el último día del mes.|
|MENSUALMENTE|PRIMERO, SEGUNDO, TERCERO, CUARTO, ÚLTIMO|Use con el parámetro **/d** \<Day> para ejecutar una tarea en una determinada semana y día. Por ejemplo, el tercer miércoles del mes.|

##### <a name="d-dayday--"></a>/d Day [, Day...] | *

Especifica un día o días de la semana o un día (o días) de un mes. Válido solo con una programación semanal o mensual.


| Tipo de programación |              Modificador              |     Valores de día (/d)      |                                                                                                 Descripción                                                                                                 |
|---------------|------------------------------------|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    SEMANA     |               1 - 52               | MON-SUN [, LUN-DOM...] |                                                                                                     \*                                                                                                      |
|    MENSUALMENTE    | PRIMERO, SEGUNDO, TERCERO, CUARTO, ÚLTIMO |        LUNES-SOL         |                                                                                   Se requiere para una programación de semana específica.                                                                                    |
|    MENSUALMENTE    |          Ninguno o {1-12}          |          1 - 31          | Opcional y válido solo con un parámetro de modificador (**/mo**) (una programación de fecha específica) o cuando **/mo** es 1-12 (una \<N> programación de cada mes). El valor predeterminado es Day 1 (el primer día del mes). |

##### <a name="m-monthmonth"></a>/m mes [, mes...]

Especifica un mes o meses del año en los que se debe ejecutar la tarea programada. Los valores válidos son JAN-DEC y * (cada mes). El parámetro **/m** solo es válido con una programación mensual. Se requiere cuando se usa el modificador LASTDAY. De lo contrario, es opcional y el valor predeterminado es * (cada mes).

##### <a name="i-idletime"></a>/i \<IdleTime>

Especifica el número de minutos que el equipo está inactivo antes de que se inicie la tarea. Un valor válido es un número entero comprendido entre 1 y 999. Este parámetro solo es válido con una programación OnIdle y, a continuación, es obligatorio.

##### <a name="st-starttime"></a>/St \<StartTime>

Especifica la hora del día a la que se inicia la tarea (cada vez que se inicia) en \<HH:MM> formato de 24 horas. El valor predeterminado es la hora actual en el equipo local. El parámetro **/St** es válido con programaciones de minuto, hora, diaria, semanal, mensual y una vez. Es necesario para una programación una vez.

##### <a name="ri-interval"></a>/RI \<Interval>

Especifica el intervalo de repetición en minutos. Esto no es aplicable a los tipos de programación: MINUTE, HOURly, OnStart, ONLOGON y OnIdle. El intervalo válido es de 1 a 599940 minutos (599940 minutos = 9999 horas). Si se especifica/ET o/DU, el intervalo de repetición tiene como valor predeterminado 10 minutos.

##### <a name="et-endtime"></a>/et \<EndTime>

Especifica la hora del día a la que finaliza un programa de tareas por minuto o por hora en \<HH:MM> formato de 24 horas. Después de la hora de finalización especificada, **SchTasks** no vuelve a iniciar la tarea hasta que se repite la hora de inicio. De forma predeterminada, las programaciones de tareas no tienen ninguna hora de finalización. Este parámetro es opcional y solo es válido con una programación por minuto o hora.

Para obtener un ejemplo, vea:
-   Para programar una tarea que se ejecute cada 100 minutos durante el horario no comercial en la sección **para programar una tarea que se ejecuta cada** \<N> **minutos** .

##### <a name="du-duration"></a>/du \<Duration>

Especifica un período de tiempo máximo para una programación de un minuto o una hora en \<HHHH:MM> formato de 24 horas. Una vez transcurrido el tiempo especificado, **SchTasks** no vuelve a iniciar la tarea hasta que se repite la hora de inicio. De forma predeterminada, las programaciones de tareas no tienen una duración máxima. Este parámetro es opcional y solo es válido con una programación por minuto o hora.

Para obtener un ejemplo, vea:
-   Para programar una tarea que se ejecute cada 3 horas durante 10 horas en la sección **para programar una tarea que se ejecuta cada** \<N> **horas** .

##### <a name="k"></a>/k

Detiene el programa que ejecuta la tarea en el momento especificado por **/et** o **/du**. Sin **/k**, **Schtasks** no vuelve a iniciar el programa después de alcanzar el tiempo especificado por **/et** o **/du**, pero no detiene el programa si todavía se está ejecutando. Este parámetro es opcional y solo es válido con una programación por minuto o hora.

Para obtener un ejemplo, vea:
-   Para programar una tarea que se ejecute cada 100 minutos durante el horario no comercial en la sección **para programar una tarea que se ejecuta cada** \<N> **minutos** .

##### <a name="sd-startdate"></a>/SD \<StartDate>

Especifica la fecha en la que comienza la programación de tareas. El valor predeterminado es la fecha actual en el equipo local. El parámetro **/SD** es válido y es opcional para todos los tipos de programación.

El formato de *startDate* varía según la configuración regional seleccionada para el equipo local en la **opción configuración regional y de idioma** del **Panel de control**. Solo un formato es válido para cada configuración regional.

En la tabla siguiente se enumeran los formatos de fecha válidos. Use el formato más similar al formato seleccionado en **fecha corta** en **configuración regional y de idioma** del **Panel de control** del equipo local.


|       Value       |                                        Descripción                                         |
|-------------------|--------------------------------------------------------------------------------------------|
| \<MM>/<DD>/<YYYY> | Se usa para los formatos de primer mes, como **Inglés (Estados Unidos)** y **Español (Panamá)**. |
| \<DD>/<MM>/<YYYY> |       Se usa para los formatos de primer día, como **búlgaro** y **holandés (Países Bajos)**.        |
| \<YYYY>/<MM>/<DD> |          Se usa para los formatos de año primero, como **sueco** y **francés (Canadá)**.          |

/Ed \<EndDate>

Especifica la fecha de finalización de la programación. Este parámetro es opcional. No es válido en una programación una vez, OnStart, ONLOGON o OnIdle. De forma predeterminada, las programaciones no tienen fecha de finalización.

El formato de *EndDate* varía según la configuración regional seleccionada para el equipo local en la **opción configuración regional y de idioma** del **Panel de control**. Solo un formato es válido para cada configuración regional.

En la tabla siguiente se enumeran los formatos de fecha válidos. Use el formato más similar al formato seleccionado en **fecha corta** en **configuración regional y de idioma** del **Panel de control** del equipo local.


|       Value       |                                        Descripción                                         |
|-------------------|--------------------------------------------------------------------------------------------|
| \<MM>/<DD>/<YYYY> | Se usa para los formatos de primer mes, como **Inglés (Estados Unidos)** y **Español (Panamá)**. |
| \<DD>/<MM>/<YYYY> |       Se usa para los formatos de primer día, como **búlgaro** y **holandés (Países Bajos)**.        |
| \<YYYY>/<MM>/<DD> |          Se usa para los formatos de año primero, como **sueco** y **francés (Canadá)**.          |

##### <a name="it"></a>/It

Especifica que se ejecute la tarea solo cuando el usuario de ejecución (la cuenta de usuario en la que se ejecuta la tarea) haya iniciado sesión en el equipo. Este parámetro no tiene ningún efecto en las tareas que se ejecutan con permisos del sistema.

De forma predeterminada, el usuario de ejecución es el usuario actual del equipo local cuando la tarea está programada o la cuenta especificada por el parámetro **/u** , si se usa una. Sin embargo, si el comando incluye el parámetro **/RU** , el usuario de ejecución es la cuenta especificada por el parámetro **/RU** .

Para obtener ejemplos, vea:
-   Para programar una tarea que se ejecuta cada 70 días si he iniciado sesión en la sección **para programar una tarea que se ejecuta cada** *N* **días** .
-   Para ejecutar una tarea solo cuando un usuario determinado ha iniciado sesión en la sección **para programar una tarea que se ejecuta con permisos diferentes** .

##### <a name="z"></a>/z

Especifica la eliminación de la tarea tras la finalización de su programación.

##### <a name="f"></a>/f

Especifica que se cree la tarea y se suprimen las advertencias si la tarea especificada ya existe.

##### <a name=""></a>/?

Muestra la ayuda en el símbolo del sistema.

### <a name="to-schedule-a-task-that-runs-every-n-minutes"></a><a name=BKMK_minutes></a>Para programar una tarea que se ejecuta cada N minutos

#### <a name="minute-schedule-syntax"></a>Sintaxis de programación de minutos

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc minute [/mo {1 - 1439}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Observaciones

En una programación por minuto, el parámetro **/SC minute** es obligatorio. El parámetro **/mo** (modificador) es opcional y especifica el número de minutos entre cada ejecución de la tarea. El valor predeterminado de **/mo** es 1 (cada minuto). Los parámetros **/et** (hora de finalización) y **/du** (duración) son opcionales y se pueden usar con o sin el parámetro **/k** (Finalizar tarea).

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-that-runs-every-20-minutes"></a>Para programar una tarea que se ejecuta cada 20 minutos

El siguiente comando programa un script de seguridad, sec. vbs, para que se ejecute cada 20 minutos. El comando usa el parámetro **/SC** para especificar una programación de minutos y el parámetro **/mo** para especificar un intervalo de 20 minutos.

Dado que el comando no incluye una fecha o una hora de inicio, la tarea se inicia 20 minutos después de que se complete el comando y se ejecuta cada 20 minutos después de que se ejecute el sistema. Tenga en cuenta que el archivo de origen del script de seguridad se encuentra en un equipo remoto, pero que la tarea está programada y se ejecuta en el equipo local.
```
schtasks /create /sc minute /mo 20 /tn Security Script /tr \\central\data\scripts\sec.vbs
```

#### <a name="to-schedule-a-task-that-runs-every-100-minutes-during-non-business-hours"></a>Para programar una tarea que se ejecuta cada 100 minutos durante el horario no comercial

El siguiente comando programa un script de seguridad, sec. vbs, para que se ejecute en el equipo local cada 100 minutos entre 5:00 P.M. y 7:59 A.M. cada día. El comando usa el parámetro **/SC** para especificar una programación de minutos y el parámetro **/mo** para especificar un intervalo de 100 minutos. Usa los parámetros **/St** y **/et** para especificar la hora de inicio y la hora de finalización de la programación de cada día. También usa el parámetro **/k** para detener el script si sigue ejecutándose a las 7:59 A.M. Sin **/k**, **Schtasks** no iniciaría el script después de las 7:59 a.m., pero si la instancia se inicia a las 6:20 a.m. todavía se estaba ejecutando, no lo detendría.
```
schtasks /create /tn Security Script /tr sec.vbs /sc minute /mo 100 /st 17:00 /et 08:00 /k
```

### <a name="to-schedule-a-task-that-runs-every-n-hours"></a><a name=BKMK_hours></a>Para programar una tarea que se ejecuta cada N horas

#### <a name="hourly-schedule-syntax"></a>Sintaxis de programación por hora

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc hourly [/mo {1 - 23}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Observaciones

En una programación por hora, se requiere el parámetro **/SC HOURLY** . El parámetro **/mo** (modificador) es opcional y especifica el número de horas entre cada ejecución de la tarea. El valor predeterminado de **/mo** es 1 (cada hora). El parámetro **/k** (Finalizar tarea) es opcional y se puede usar con **/et** (finalizar en el momento especificado) o **/du** (finalizar después del intervalo especificado).

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-that-runs-every-five-hours"></a>Para programar una tarea que se ejecuta cada cinco horas

El siguiente comando programa el programa MyApp para que se ejecute cada cinco horas a partir del primer día del 2002 de marzo. Usa el parámetro **/mo** para especificar el intervalo y el parámetro **/SD** para especificar la fecha de inicio. Dado que el comando no especifica una hora de inicio, la hora actual se utiliza como hora de inicio.

Dado que el equipo local está configurado para usar la opción **Inglés (Zimbabue)** en **las opciones de configuración regional y de idioma** del **Panel de control**, el formato de la fecha de inicio es MM/DD/AAAA (03/01/2002).
```
schtasks /create /sc hourly /mo 5 /sd 03/01/2002 /tn My App /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-hour-at-five-minutes-past-the-hour"></a>Para programar una tarea que se ejecute cada hora a los cinco minutos posteriores a la hora

El siguiente comando programa el programa MyApp para ejecutarse cada hora a partir de cinco minutos después de la medianoche. Dado que se omite el parámetro **/mo** , el comando usa el valor predeterminado para la programación por hora, que es cada (1) hora. Si este comando se ejecuta después de las 12:05 A.M., el programa no se ejecuta hasta el día siguiente.
```
schtasks /create /sc hourly /st 00:05 /tn My App /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-3-hours-for-10-hours"></a>Para programar una tarea que se ejecuta cada 3 horas durante 10 horas

El siguiente comando programa el programa MyApp para que se ejecute cada 3 horas durante 10 horas.

El comando usa el parámetro **/SC** para especificar una programación por hora y el parámetro **/mo** para especificar el intervalo de 3 horas. Usa el parámetro **/St** para iniciar la programación a medianoche y el parámetro **/du** para finalizar las repeticiones después de 10 horas. Dado que el programa se ejecuta durante unos minutos, el parámetro **/k** , que detiene el programa si aún se está ejecutando cuando expira la duración, no es necesario.
```
schtasks /create /tn My App /tr myapp.exe /sc hourly /mo 3 /st 00:00 /du 0010:00
```
En este ejemplo, la tarea se ejecuta a las 12:00 A.M., 3:00 A.M., 6:00 A.M. y 9:00 A.M. Dado que la duración es de 10 horas, la tarea no se ejecuta de nuevo a las 12:00 P.M. En su lugar, se inicia de nuevo a las 12:00 A.M. el día siguiente.

### <a name="to-schedule-a-task-that-runs-every-n-days"></a><a name=BKMK_days></a>Para programar una tarea que se ejecuta cada N días

#### <a name="daily-schedule-syntax"></a>Sintaxis de programación diaria

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc daily [/mo {1 - 365}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Observaciones

En una programación diaria, se requiere el parámetro **/SC Daily** . El parámetro **/mo** (modificador) es opcional y especifica el número de días entre cada ejecución de la tarea. El valor predeterminado de **/mo** es 1 (cada día).

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-that-runs-every-day"></a>Para programar una tarea que se ejecuta todos los días

Para programar el programa MyApp para que se ejecute una vez al día, cada día, a las 8:00 A.M. hasta el 31 de diciembre de 2002. Dado que omite el parámetro **/mo** , el intervalo predeterminado de 1 se usa para ejecutar el comando cada día.

En este ejemplo, dado que el sistema del equipo local se establece en la opción **Inglés (Reino Unido)** de **las opciones de configuración regional y de idioma** del **Panel de control**, el formato de la fecha de finalización es DD/MM/AAAA (31/12/2002)
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc daily /st 08:00 /ed 31/12/2002
```

#### <a name="to-schedule-a-task-that-runs-every-12-days"></a>Para programar una tarea que se ejecuta cada 12 días

Para programar el programa MyApp para que se ejecute cada doce días a las 1:00 P.M. (13:00) a partir del 31 de diciembre de 2002. El comando usa el parámetro **/mo** para especificar un intervalo de dos (2) días y los parámetros **/SD** y **/St** para especificar la fecha y la hora.

En este ejemplo, dado que el sistema está establecido en la opción **Inglés (Zimbabue)** en **configuración regional y de idioma** en el **Panel de control**, el formato de la fecha de finalización es MM/DD/AAAA (12/31/2002)
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc daily /mo 12 /sd 12/31/2002 /st 13:00
```

#### <a name="to-schedule-a-task-that-runs-every-70-days-if-i-am-logged-on"></a>Para programar una tarea que se ejecuta cada 70 días si he iniciado sesión

El siguiente comando programa un script de seguridad, sec. vbs, para que se ejecute cada 70 días. El comando usa el parámetro **/mo** para especificar un intervalo de 70 días. También usa el parámetro **/it** para especificar que la tarea se ejecuta solo cuando el usuario en cuya cuenta se ejecuta la tarea inicia sesión en el equipo. Dado que la tarea se ejecutará con los permisos de mi cuenta de usuario, la tarea se ejecutará solo cuando haya iniciado sesión.
```
schtasks /create /tn Security Script /tr sec.vbs /sc daily /mo 70 /it
```

> [!NOTE]
> Para identificar las tareas con la propiedad solo Interactive (**/it**), use una consulta detallada **(/Query/v**). En una presentación de consulta detallada de una tarea con **/it**, el campo **modo de inicio de sesión** tiene un valor de **solo interactivo**.

### <a name="to-schedule-a-task-that-runs-every-n-weeks"></a><a name=BKMK_weeks></a>Para programar una tarea que se ejecuta cada N semanas

#### <a name="weekly-schedule-syntax"></a>Sintaxis de programación semanal

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/mo {1 - 52}] [/d {<MON - SUN>[,MON - SUN...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Observaciones

En una programación semanal, se requiere el parámetro **/SC Weekly** . El parámetro **/mo** (modificador) es opcional y especifica el número de semanas entre cada ejecución de la tarea. El valor predeterminado de **/mo** es 1 (cada semana).

Las programaciones semanales también tienen un parámetro opcional **/d** para programar la ejecución de la tarea en los días de la semana especificados o en todos los días (*). El valor predeterminado es LUN (lunes). La opción todos los días (*) equivale a programar una tarea diaria.

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-that-runs-every-six-weeks"></a>Para programar una tarea que se ejecuta cada seis semanas

El siguiente comando programa el programa MyApp para que se ejecute en un equipo remoto cada seis semanas. El comando usa el parámetro **/mo** para especificar el intervalo. Dado que el comando omite el parámetro **/d** , la tarea se ejecuta los lunes.

Este comando también usa el parámetro **/s** para especificar el equipo remoto y el parámetro **/u** para ejecutar el comando con los permisos de la cuenta de administrador del usuario. Dado que se omite el parámetro **/p** , **SchTasks.exe** solicita al usuario la contraseña de la cuenta de administrador.

Además, dado que el comando se ejecuta de forma remota, todas las rutas de acceso en el comando, incluida la ruta de acceso a MyApp.exe, hacen referencia a las rutas de acceso en el equipo remoto.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /mo 6 /s Server16 /u Admin01
```

#### <a name="to-schedule-a-task-that-runs-every-other-week-on-friday"></a>Para programar una tarea que se ejecuta cada dos semanas el viernes

El siguiente comando programa una tarea para que se ejecute cada dos viernes. Usa el parámetro **/mo** para especificar el intervalo de dos semanas y el parámetro **/d** para especificar el día de la semana. Para programar una tarea que se ejecute todos los viernes, omita el parámetro **/mo** o establézcalo en 1.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /mo 2 /d FRI
```

### <a name="to-schedule-a-task-that-runs-every-n-months"></a><a name=BKMK_months></a>Para programar una tarea que se ejecuta cada N meses

#### <a name="syntax"></a>Sintaxis

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly [/mo {1 - 12}] [/d {1 - 31}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Observaciones

En este tipo de programación, se requiere el parámetro **/SC Monthly** . El parámetro **/mo** (modificador), que especifica el número de meses entre cada ejecución de la tarea, es opcional y el valor predeterminado es 1 (cada mes). Este tipo de programación también tiene un parámetro opcional **/d** para programar la ejecución de la tarea en una fecha determinada del mes. El valor predeterminado es 1 (el primer día del mes).

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-that-runs-on-the-first-day-of-every-month"></a>Para programar una tarea que se ejecuta el primer día de cada mes

El siguiente comando programa el programa MyApp para que se ejecute el primer día de cada mes. Dado que un valor de 1 es el valor predeterminado para el parámetro **/mo** (modificador) y el parámetro **/d** (día), estos parámetros se omiten en el comando.
```
schtasks /create /tn My App /tr myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-that-runs-every-three-months"></a>Para programar una tarea que se ejecuta cada tres meses

El siguiente comando programa el programa MyApp para que se ejecute cada tres meses. Usa el parámetro **/mo** para especificar el intervalo.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo 3
```

#### <a name="to-schedule-a-task-that-runs-at-midnight-on-the-21st-day-of-every-other-month"></a>Para programar una tarea que se ejecuta a medianoche el día 21 de cada mes

El siguiente comando programa el programa MyApp para que se ejecute todos los meses del día 21 del mes a medianoche. El comando especifica que esta tarea se debe ejecutar durante un año, del 2 de julio de 2002 al 30 de junio de 2003.

El comando usa el parámetro **/mo** para especificar el intervalo mensual (cada dos meses), el parámetro **/d** para especificar la fecha y el **/St** para especificar la hora. También se usan los parámetros **/SD** y **/Ed** para especificar la fecha de inicio y la fecha de finalización, respectivamente. Dado que el equipo local se establece en la opción **Inglés (Sudáfrica)** en **configuración regional y de idioma** en el **Panel de control**, las fechas se especifican en el formato local, AAAA/MM/DD.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo 2 /d 21 /st 00:00 /sd 2002/07/01 /ed 2003/06/30
```

### <a name="to-schedule-a-task-that-runs-on-a-specific-day-of-the-week"></a><a name=BKMK_spec_day></a>Para programar una tarea que se ejecuta en un día concreto de la semana

#### <a name="weekly-schedule-syntax"></a>Sintaxis de programación semanal

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/d {<MON - SUN>[,MON - SUN...] | *}] [/mo {1 - 52}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Observaciones

La programación del día de la semana es una variación de la programación semanal. En una programación semanal, se requiere el parámetro **/SC Weekly** . El parámetro **/mo** (modificador) es opcional y especifica el número de semanas entre cada ejecución de la tarea. El valor predeterminado de **/mo** es 1 (cada semana). El parámetro **/d** , que es opcional, programa la tarea para que se ejecute en los días de la semana especificados o en todos los días ( \* ). El valor predeterminado es LUN (lunes). La opción todos los días **( \* /d **) es equivalente a programar una tarea diaria.

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-that-runs-every-wednesday"></a>Para programar una tarea que se ejecuta todos los miércoles

El siguiente comando programa el programa MyApp para que se ejecute cada semana el miércoles. El comando usa el parámetro **/d** para especificar el día de la semana. Dado que el comando omite el parámetro **/mo** , la tarea se ejecuta cada semana.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /d WED
```

#### <a name="to-schedule-a-task-that-runs-every-eight-weeks-on-monday-and-friday"></a>Para programar una tarea que se ejecuta cada ocho semanas el lunes y el viernes

El siguiente comando programa una tarea para que se ejecute el lunes y el viernes de cada octavo semana. Usa el parámetro **/d** para especificar los días y el parámetro **/mo** para especificar el intervalo de ocho semanas.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /mo 8 /d MON,FRI
```

### <a name="to-schedule-a-task-that-runs-on-a-specific-week-of-the-month"></a><a name=BKMK_spec_week></a>Para programar una tarea que se ejecuta en una semana específica del mes

#### <a name="specific-week-syntax"></a>Sintaxis de semana específica

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo {FIRST | SECOND | THIRD | FOURTH | LAST} /d MON - SUN [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Observaciones

En este tipo de programación, es necesario el parámetro **/SC Monthly** , el parámetro **/mo** (modificador) y el parámetro **/d** (Day). El parámetro **/mo** (modificador) especifica la semana en la que se ejecuta la tarea. El parámetro **/d** especifica el día de la semana. (Solo puede especificar un día de la semana para este tipo de programación). Esta programación también tiene un parámetro **/m** (mes) opcional que le permite programar la tarea para meses concretos o cada mes ( \* ). El valor predeterminado del parámetro **/m** es cada mes ( \* ).

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-for-the-second-sunday-of-every-month"></a>Para programar una tarea para el segundo domingo de cada mes

El siguiente comando programa el programa MyApp para que se ejecute el segundo domingo de cada mes. Usa el parámetro **/mo** para especificar la segunda semana del mes y el parámetro **/d** para especificar el día.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo SECOND /d SUN
```

#### <a name="to-schedule-a-task-for-the-first-monday-in-march-and-september"></a>Para programar una tarea el primer lunes en marzo y septiembre

El siguiente comando programa el programa MyApp para que se ejecute el primer lunes de marzo y septiembre. Usa el parámetro **/mo** para especificar la primera semana del mes y el parámetro **/d** para especificar el día. Usa el parámetro **/m** para especificar el mes, separando los argumentos de mes con una coma.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo FIRST /d MON /m MAR,SEP
```

### <a name="to-schedule-a-task-that-runs-on-a-specific-date-each-month"></a><a name=BKMK_spec_date></a>Para programar una tarea que se ejecuta en una fecha específica cada mes

#### <a name="specific-date-syntax"></a>Sintaxis de fecha específica

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /d {1 - 31} [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Observaciones

En el tipo de programación de fecha específica, se requiere el parámetro **/SC Monthly** y el parámetro **/d** (Day). El parámetro **/d** especifica una fecha del mes (1-31), no un día de la semana. Solo puede especificar un día en la programación. El parámetro **/mo** (modificador) no es válido con este tipo de programación.

El parámetro **/m** (mes) es opcional para este tipo de programación y el valor predeterminado es todos los meses (<em>). **Schtasks</em> * no permite programar una tarea para una fecha que no se produce en un mes especificado por el parámetro **/m** . Sin embargo, si omite el parámetro **/m** y programa una tarea para una fecha que no aparece en cada mes, como el día 31, la tarea no se ejecuta en los meses más cortos. Para programar una tarea para el último día del mes, use el tipo de programación del último día.

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-for-the-first-day-of-every-month"></a>Para programar una tarea el primer día de cada mes

El siguiente comando programa el programa MyApp para que se ejecute el primer día de cada mes. Dado que el modificador predeterminado es None (sin modificador), el día predeterminado es Day 1 y el mes predeterminado es cada mes, el comando no necesita ningún parámetro adicional.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-for-the-15th-days-of-may-and-june"></a>Para programar una tarea durante los quince días de mayo y junio

El siguiente comando programa el programa MyApp para que se ejecute el 15 de mayo y el 15 de junio a las 3:00 P.M. (15:00). Usa el parámetro **/m** para especificar la fecha y el parámetro **/m** para especificar los meses. También usa el parámetro **/St** para especificar la hora de inicio.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /d 15 /m MAY,JUN /st 15:00
```

### <a name="to-schedule-a-task-that-runs-on-the-last-day-of-a-month"></a><a name=BKMK_last_day></a>Para programar una tarea que se ejecute el último día de un mes

#### <a name="last-day-syntax"></a>Sintaxis del último día

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo LASTDAY /m {JAN - DEC[,JAN - DEC...] | *} [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Observaciones

En el tipo de programación del último día, es necesario el parámetro **/SC Monthly** , el parámetro **/mo lastDay** (modificador) y el parámetro **/m** (month). El parámetro **/d** (Day) no es válido.

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-for-the-last-day-of-every-month"></a>Para programar una tarea para el último día de cada mes

El siguiente comando programa el programa MyApp para que se ejecute el último día de cada mes. Usa el parámetro **/mo** para especificar el último día y el parámetro **/m** con el carácter comodín (*) para indicar que el programa se ejecuta cada mes.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo lastday /m *
```

#### <a name="to-schedule-a-task-at-600-pm-on-the-last-days-of-february-and-march"></a>Para programar una tarea a las 6:00 P.M. los últimos días de febrero y marzo

El siguiente comando programa el programa MyApp para que se ejecute el último día de febrero y el último día de marzo a las 6:00 P.M. Usa el parámetro **/mo** para especificar el último día, el parámetro **/m** para especificar los meses y el parámetro **/St** para especificar la hora de inicio.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo lastday /m FEB,MAR /st 18:00
```

### <a name="to-schedule-a-task-that-runs-once"></a><a name=BKMK_once></a>Para programar una tarea que se ejecuta una vez

#### <a name="syntax"></a>Sintaxis

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once /st <HH:MM> [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Observaciones

En el tipo de programación Run-once, se requiere el parámetro **/SC once** . El parámetro **/St** , que especifica el tiempo que se ejecuta la tarea, es obligatorio. El parámetro **/SD** , que especifica la fecha en la que se ejecuta la tarea, es opcional. Los parámetros **/mo** (modificador) y **/Ed** (fecha de finalización) no son válidos para este tipo de programación.

**SchTasks** no permite programar una tarea para que se ejecute una vez si la fecha y la hora especificadas están en el pasado, en función de la hora del equipo local. Para programar una tarea que se ejecute una vez en un equipo remoto en una zona horaria diferente, debe programarla antes de que se produzca esa fecha y hora en el equipo local.

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-that-runs-one-time"></a>Para programar una tarea que se ejecuta una vez

El siguiente comando programa el programa MyApp para que se ejecute a medianoche el 1 de enero de 2003. Usa el parámetro **/SC** para especificar el tipo de programación y el **/SD** y **St** para especificar la fecha y la hora.

Dado que el equipo local usa la opción **Inglés (Estados Unidos)** en **las opciones de configuración regional y de idioma** del **Panel de control**, el formato de la fecha de inicio es mm/dd/aaaa.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc once /sd 01/01/2003 /st 00:00
```

### <a name="to-schedule-a-task-that-runs-every-time-the-system-starts"></a><a name=BKMK_startup></a>Para programar una tarea que se ejecuta cada vez que se inicia el sistema

#### <a name="syntax"></a>Sintaxis

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onstart [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Observaciones

En el tipo de programación on-Start, se requiere el parámetro **/SC OnStart** . El parámetro **/SD** (fecha de inicio) es opcional y el valor predeterminado es la fecha actual.

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-that-runs-when-the-system-starts"></a>Para programar una tarea que se ejecuta cuando se inicia el sistema

El siguiente comando programa el programa MyApp para que se ejecute cada vez que se inicie el sistema, a partir del 15 de marzo de 2001:

Dado que el equipo local usa la opción **Inglés (Estados Unidos)** en **las opciones de configuración regional y de idioma** del **Panel de control**, el formato de la fecha de inicio es mm/dd/aaaa.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc onstart /sd 03/15/2001
```

### <a name="to-schedule-a-task-that-runs-when-a-user-logs-on"></a><a name=BKMK_logon></a>Para programar una tarea que se ejecuta cuando un usuario inicia sesión

#### <a name="syntax"></a>Sintaxis

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onlogon [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Observaciones

El tipo de programación de inicio de sesión programa una tarea que se ejecuta cada vez que un usuario inicia sesión en el equipo. En el tipo de programación al iniciar sesión, se requiere el parámetro **/SC ONLOGON** . El parámetro **/SD** (fecha de inicio) es opcional y el valor predeterminado es la fecha actual.

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-that-runs-when-a-user-logs-on-to-a-remote-computer"></a>Para programar una tarea que se ejecuta cuando un usuario inicia sesión en un equipo remoto

El siguiente comando programa un archivo por lotes para que se ejecute cada vez que un usuario (cualquier usuario) inicie sesión en el equipo remoto. Usa el parámetro **/s** para especificar el equipo remoto. Dado que el comando es remoto, todas las rutas de acceso del comando, incluida la ruta de acceso al archivo por lotes, hacen referencia a una ruta de acceso en el equipo remoto.
```
schtasks /create /tn Start Web Site /tr c:\myiis\webstart.bat /sc onlogon /s Server23
```

### <a name="to-schedule-a-task-that-runs-when-the-system-is-idle"></a><a name=BKMK_idle></a>Para programar una tarea que se ejecuta cuando el sistema está inactivo

#### <a name="syntax"></a>Sintaxis

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onidle /i {1 - 999} [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Observaciones

El tipo en programación inactiva programa una tarea que se ejecuta siempre que no haya ninguna actividad de usuario durante el tiempo especificado por el parámetro **/i** . En el tipo de programación on IDLE, se requieren el parámetro **/SC** OnIdle y el parámetro **/i** . **/SD** (fecha de inicio) es opcional y el valor predeterminado es la fecha actual.

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-that-runs-whenever-the-computer-is-idle"></a>Para programar una tarea que se ejecuta cuando el equipo está inactivo

El siguiente comando programa el programa MyApp para que se ejecute siempre que el equipo esté inactivo. Usa el parámetro **/i** necesario para especificar que el equipo debe permanecer inactivo durante diez minutos antes de que se inicie la tarea.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc onidle /i 10
```

### <a name="to-schedule-a-task-that-runs-now"></a><a name=BKMK_now></a>Para programar una tarea que se ejecuta ahora

**SchTasks** no tiene una opción ejecutar ahora, pero puede simular esa opción creando una tarea que se ejecute una vez y se inicie en unos minutos.

#### <a name="syntax"></a>Sintaxis

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once [/st <HH:MM>] /sd <MM/DD/YYYY> [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="examples"></a>Ejemplos

#### <a name="to-schedule-a-task-that-runs-a-few-minutes-from-now"></a>Para programar una tarea que se ejecuta unos minutos desde ahora.

El siguiente comando programa una tarea para que se ejecute una vez, el 13 de noviembre de 2002 a las 2:18 P.M. (hora local).

Dado que el equipo local usa la opción **Inglés (Estados Unidos)** en **las opciones de configuración regional y de idioma** del **Panel de control**, el formato de la fecha de inicio es mm/dd/aaaa.
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc once /st 14:18 /sd 11/13/2002
```

### <a name="to-schedule-a-task-that-runs-with-different-permissions"></a><a name=BKMK_diff_perms></a>Para programar una tarea que se ejecuta con permisos diferentes

Puede programar tareas de todos los tipos para que se ejecuten con permisos de una cuenta alternativa tanto en el equipo local como en el equipo remoto. Además de los parámetros necesarios para el tipo de programación determinado, el parámetro **/RU** es obligatorio y el parámetro **/RP** es opcional.

#### <a name="examples"></a>Ejemplos

#### <a name="to-run-a-task-with-administrator-permissions-on-the-local-computer"></a>Para ejecutar una tarea con permisos de administrador en el equipo local

El siguiente comando programa el programa MyApp para que se ejecute en el equipo local. Utiliza el **/RU** para especificar que la tarea debe ejecutarse con los permisos de la cuenta de administrador del usuario (Admin06). En este ejemplo, la tarea está programada para ejecutarse todos los martes, pero puede usar cualquier tipo de programación para la ejecución de una tarea con permisos alternativos.
```
schtasks /create /tn My App /tr myapp.exe /sc weekly /d TUE /ru Admin06
```
En respuesta, **SchTasks.exe** solicita la contraseña de ejecución para la cuenta de Admin06 y, a continuación, muestra un mensaje de operación correcta.
```
Please enter the run as password for Admin06: ********
SUCCESS: The scheduled task My App has successfully been created.
```

#### <a name="to-run-a-task-with-alternate-permissions-on-a-remote-computer"></a>Para ejecutar una tarea con permisos alternativos en un equipo remoto

El siguiente comando programa el programa MyApp para que se ejecute en el equipo de marketing cada cuatro días.

El comando usa el parámetro **/SC** para especificar una programación diaria y un parámetro **/mo** para especificar un intervalo de cuatro días.

El comando usa el parámetro **/s** para proporcionar el nombre del equipo remoto y el parámetro **/u** para especificar una cuenta con permiso para programar una tarea en el equipo remoto (Admin01 en el equipo de marketing). También usa el parámetro **/RU** para especificar que la tarea debe ejecutarse con los permisos de la cuenta de usuario que no es administrador (User01 en el dominio reskit). Sin el parámetro **/RU** , la tarea se ejecutaría con los permisos de la cuenta especificada por **/u**.
```
schtasks /create /tn My App /tr myapp.exe /sc daily /mo 4 /s Marketing /u Marketing\Admin01 /ru Reskits\User01
```
**SchTasks** primero solicita la contraseña del usuario con el nombre del parámetro **/u** (para ejecutar el comando) y, a continuación, solicita la contraseña del usuario nombrado por el parámetro **/RU** (para ejecutar la tarea). Después de autenticar las contraseñas, **SchTasks** muestra un mensaje que indica que la tarea está programada.
```
Type the password for Marketing\Admin01:********

Please enter the run as password for Reskits\User01: ********

SUCCESS: The scheduled task My App has successfully been created.
```

#### <a name="to-run-a-task-only-when-a-particular-user-is-logged-on"></a>Para ejecutar una tarea solo cuando un usuario determinado ha iniciado sesión

El siguiente comando programa el programa de AdminCheck.exe para que se ejecute en el equipo público todos los viernes a las 4:00 A.M., pero solo si el administrador del equipo ha iniciado sesión.

El comando usa el parámetro **/SC** para especificar una programación semanal, el parámetro **/d** para especificar el día y el parámetro **/St** para especificar la hora de inicio.

El comando usa el parámetro **/s** para proporcionar el nombre del equipo remoto y el parámetro **/u** para especificar una cuenta con permiso para programar una tarea en el equipo remoto. También usa el parámetro **/RU** para configurar la tarea para que se ejecute con los permisos del administrador del equipo público (Public\Admin01) y el parámetro **/it** para indicar que la tarea se ejecuta solo cuando la cuenta Public\Admin01 ha iniciado sesión.
```
schtasks /create /tn Check Admin /tr AdminCheck.exe /sc weekly /d FRI /st 04:00 /s Public /u Domain3\Admin06 /ru Public\Admin01 /it
```
**Nota**
-   Para identificar las tareas con la propiedad solo Interactive (**/it**), use una consulta detallada **(/Query/v**). En una presentación de consulta detallada de una tarea con **/it**, el campo **modo de inicio de sesión** tiene un valor de **solo interactivo**.

### <a name="to-schedule-a-task-that-runs-with-system-permissions"></a><a name=BKMK_sys_perms></a>Para programar una tarea que se ejecuta con permisos del sistema

Las tareas de todos los tipos se pueden ejecutar con los permisos de la cuenta del sistema en el equipo local y en el equipo remoto. Además de los parámetros necesarios para el tipo de programación determinado, el parámetro **/RU System** (o * */RU * *) es obligatorio y el parámetro **/RP** no es válido.

**Importante**
-   La cuenta del sistema no tiene derechos de inicio de sesión interactivo. Los usuarios no pueden ver ni interactuar con programas o tareas que se ejecuten con permisos del sistema.
-   El parámetro **/RU** determina los permisos en los que se ejecuta la tarea, no los permisos que se usan para programar la tarea. Solo los administradores pueden programar tareas, independientemente del valor del parámetro **/RU** .

**Nota**

Para identificar las tareas que se ejecutan con permisos del sistema, utilice una consulta detallada (**/query** **/v**). En una visualización de consulta detallada de una tarea de ejecución del sistema, el campo de **usuario ejecutar como** tiene un valor de **NT AUTHORITY\SYSTEM** y el campo **modo de inicio de sesión** solo tiene un valor de **background**.

#### <a name="examples"></a>Ejemplos

#### <a name="to-run-a-task-with-system-permissions"></a>Para ejecutar una tarea con permisos del sistema

El siguiente comando programa el programa MyApp para que se ejecute en el equipo local con los permisos de la cuenta del sistema. En este ejemplo, la tarea está programada para ejecutarse el decimoquinto día de cada mes, pero puede usar cualquier tipo de programación para la ejecución de una tarea con permisos del sistema.

El comando usa el parámetro **del sistema/RU** para especificar el contexto de seguridad del sistema. Dado que las tareas del sistema no usan una contraseña, se omite el parámetro **/RP** .
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /d 15 /ru System
```
En respuesta, **SchTasks.exe** muestra un mensaje informativo y un mensaje de operación correcta. No solicita una contraseña.
```
INFO: The task will be created under user name (NT AUTHORITY\SYSTEM).
SUCCESS: The Scheduled task My App has successfully been created.
```

#### <a name="to-run-a-task-with-system-permissions-on-a-remote-computer"></a>Para ejecutar una tarea con permisos del sistema en un equipo remoto

El siguiente comando programa el programa MyApp para que se ejecute en el equipo Finance01 cada mañana a las 4:00 A.M. con permisos del sistema.

El comando usa el parámetro **/TN** para asignar un nombre a la tarea y el parámetro **/TR** para especificar la copia remota del programa MyApp. Usa el parámetro **/SC** para especificar una programación diaria, pero omite el parámetro **/mo** porque 1 (todos los días) es el valor predeterminado. Usa el parámetro **/St** para especificar la hora de inicio, que es también la hora a la que se ejecutará la tarea cada día.

El comando usa el parámetro **/s** para proporcionar el nombre del equipo remoto y el parámetro **/u** para especificar una cuenta con permiso para programar una tarea en el equipo remoto. También usa el parámetro **/RU** para especificar que la tarea debe ejecutarse en la cuenta del sistema. Sin el parámetro **/RU** , la tarea se ejecutaría con los permisos de la cuenta especificada por **/u**.
```
schtasks /create /tn My App /tr myapp.exe /sc daily /st 04:00 /s Finance01 /u Admin01 /ru System
```
**SchTasks** solicita la contraseña del usuario nombrado por el parámetro **/u** y, después de autenticar la contraseña, muestra un mensaje que indica que la tarea se ha creado y que se ejecutará con los permisos de la cuenta del sistema.
```
Type the password for Admin01:**********

INFO: The Schedule Task My App will be created under user name (NT AUTHORITY\
SYSTEM).
SUCCESS: The scheduled task My App has successfully been created.
```

### <a name="to-schedule-a-task-that-runs-more-than-one-program"></a><a name=BKMK_multi_progs></a>Para programar una tarea que ejecuta más de un programa

Cada tarea ejecuta un solo programa. Sin embargo, puede crear un archivo por lotes que ejecute varios programas y, a continuación, programar una tarea para que ejecute el archivo por lotes. En el siguiente procedimiento se muestra este método:
1. Cree un archivo por lotes que inicie los programas que desea ejecutar.

   En este ejemplo, se crea un archivo por lotes que inicia Visor de eventos (Eventvwr.exe) y el monitor de sistema (Perfmon.exe).
   - Abra un editor de texto, como el Bloc de notas.
   - Escriba el nombre y la ruta de acceso completa al archivo ejecutable para cada programa. En este caso, el archivo incluye las siguientes instrucciones.
     ```
     C:\Windows\System32\Eventvwr.exe
     C:\Windows\System32\Perfmon.exe
     ```
   - Guarde el archivo como MyApps.bat.
2. Use **Schtasks.exe** para crear una tarea que ejecute MyApps.bat.

   El siguiente comando crea la tarea de supervisión, que se ejecuta cada vez que alguien inicia sesión. Usa el parámetro **/TN** para asignar un nombre a la tarea y el parámetro **/tr** para ejecutar MyApps.bat. Usa el parámetro **/SC** para indicar el tipo de programación ONLOGON y el parámetro **/RU** para ejecutar la tarea con los permisos de la cuenta de administrador del usuario.
   ```
   schtasks /create /tn Monitor /tr C:\MyApps.bat /sc onlogon /ru Reskit\Administrator
   ```
   Como resultado de este comando, cada vez que un usuario inicia sesión en el equipo, la tarea se inicia tanto Visor de eventos como el monitor de sistema.

### <a name="to-schedule-a-task-that-runs-on-a-remote-computer"></a><a name=BKMK_remote></a>Para programar una tarea que se ejecuta en un equipo remoto

Para programar la ejecución de una tarea en un equipo remoto, debe agregar la tarea a la programación del equipo remoto. Las tareas de todos los tipos se pueden programar en un equipo remoto, pero se deben cumplir las condiciones siguientes.
-   Debe tener permiso para programar la tarea. Como tal, debe haber iniciado sesión en el equipo local con una cuenta que sea miembro del grupo administradores en el equipo remoto, o bien debe usar el parámetro **/u** para proporcionar las credenciales de un administrador del equipo remoto.
-   Puede usar el parámetro **/u** solo cuando los equipos locales y remotos están en el mismo dominio o el equipo local se encuentra en un dominio en el que confía el dominio del equipo remoto. De lo contrario, el equipo remoto no puede autenticar la cuenta de usuario especificada y no puede comprobar que la cuenta es miembro del grupo administradores.
-   La tarea debe tener permisos suficientes para ejecutarse en el equipo remoto. Los permisos necesarios varían con la tarea. De forma predeterminada, la tarea se ejecuta con el permiso del usuario actual del equipo local o, si se usa el parámetro **/u** , la tarea se ejecuta con el permiso de la cuenta especificada por el parámetro **/u** . Sin embargo, puede usar el parámetro **/RU** para ejecutar la tarea con permisos de una cuenta de usuario diferente o con permisos del sistema.

#### <a name="examples"></a>Ejemplos

#### <a name="an-administrator-schedules-a-task-on-a-remote-computer"></a>Un administrador programa una tarea en un equipo remoto

El siguiente comando programa el programa MyApp para que se ejecute en el equipo remoto SRV01 cada diez días a partir de inmediatamente. El comando usa el parámetro **/s** para proporcionar el nombre del equipo remoto. Dado que el usuario actual local es un administrador del equipo remoto, el parámetro **/u** , que proporciona permisos alternativos para programar la tarea, no es necesario.

Tenga en cuenta que al programar tareas en un equipo remoto, todos los parámetros hacen referencia al equipo remoto. Por lo tanto, el archivo ejecutable especificado por el parámetro **/TR** hace referencia a la copia de MyApp.exe en el equipo remoto.
```
schtasks /create /s SRV01 /tn My App /tr c:\program files\corpapps\myapp.exe /sc daily /mo 10
```
En respuesta, **SchTasks** muestra un mensaje de confirmación que indica que la tarea está programada.

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-1"></a>Un usuario programa un comando en un equipo remoto (caso 1)

El siguiente comando programa el programa MyApp para que se ejecute en el equipo remoto SRV06 cada tres horas. Dado que se requieren permisos de administrador para programar una tarea, el comando usa los parámetros **/u** y **/p** para proporcionar las credenciales de la cuenta de administrador del usuario (Admin01 en el dominio reskit). De forma predeterminada, estos permisos también se usan para ejecutar la tarea. Sin embargo, dado que la tarea no necesita permisos de administrador para ejecutarse, el comando incluye los parámetros **/u** y **/RP** para invalidar el valor predeterminado y ejecutar la tarea con el permiso de la cuenta de usuario que no es administrador en el equipo remoto.
```
schtasks /create /s SRV06 /tn My App /tr c:\program files\corpapps\myapp.exe /sc hourly /mo 3 /u reskits\admin01 /p R43253@4$ /ru SRV06\user03 /rp MyFav!!Pswd
```
En respuesta, **SchTasks** muestra un mensaje de confirmación que indica que la tarea está programada.

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-2"></a>Un usuario programa un comando en un equipo remoto (caso 2)

El siguiente comando programa el programa MyApp para que se ejecute en el equipo remoto SRV02 el último día de cada mes. Dado que el usuario actual local (user03) no es un administrador del equipo remoto, el comando usa el parámetro **/u** para proporcionar las credenciales de la cuenta de administrador del usuario (Admin01 en el dominio reskit). Los permisos de la cuenta de administrador se usarán para programar la tarea y para ejecutar la tarea.
```
schtasks /create /s SRV02 /tn My App /tr c:\program files\corpapps\myapp.exe /sc monthly /mo LASTDAY /m * /u reskits\admin01
```
Dado que el comando no incluía el parámetro **/p** (contraseña), **Schtasks** solicita la contraseña. A continuación, muestra un mensaje de confirmación y, en este caso, una advertencia.
```
Type the password for reskits\admin01:********

SUCCESS: The scheduled task My App has successfully been created.

WARNING: The Scheduled task My App has been created, but may not run because
the account information could not be set.
```
Esta advertencia indica que el dominio remoto no pudo autenticar la cuenta especificada por el parámetro **/u** . En este caso, el dominio remoto no pudo autenticar la cuenta de usuario porque el equipo local no es miembro de un dominio en el que confía el dominio del equipo remoto. Cuando esto ocurre, el trabajo de tarea aparece en la lista de tareas programadas, pero la tarea está realmente vacía y no se ejecutará.

La siguiente presentación de una consulta detallada expone el problema con la tarea. En la pantalla, tenga en cuenta que el valor de la **siguiente hora de ejecución** **nunca** es y que no se pudo recuperar el valor de **Ejecutar como usuario** **de la base de datos del programador de tareas**.

Si este equipo era miembro del mismo dominio o de un dominio de confianza, la tarea se habría programado correctamente y se habría ejecutado según lo especificado.
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

#### <a name="remarks"></a>Observaciones

-   Para ejecutar un comando **/Create** con los permisos de otro usuario, use el parámetro **/u** . El parámetro **/u** solo es válido para programar tareas en equipos remotos.
-   Para ver más ejemplos de **Schtasks/Create** , escriba **Schtasks/Create/?** en un símbolo del sistema.
-   Para programar una tarea que se ejecuta con permisos de otro usuario, use el parámetro **/RU** . El parámetro **/RU** es válido para las tareas en equipos locales y remotos.
-   Para usar el parámetro **/u** , el equipo local debe estar en el mismo dominio que el equipo remoto o debe estar en un dominio en el que confíe el dominio del equipo remoto. De lo contrario, la tarea no se crea o el trabajo de tarea está vacío y la tarea no se ejecuta.
-   **SchTasks** siempre solicita una contraseña a menos que proporcione una, incluso cuando programa una tarea en el equipo local con la cuenta de usuario actual. Este es el comportamiento normal de **SchTasks**.
-   **SchTasks** no comprueba las ubicaciones de los archivos de programa o las contraseñas de cuentas de usuario. Si no especifica la ubicación correcta del archivo o la contraseña correcta para la cuenta de usuario, se creará la tarea, pero no se ejecutará. Además, si la contraseña de una cuenta cambia o expira, y no cambia la contraseña guardada en la tarea, la tarea no se ejecuta.
-   La cuenta del sistema no tiene derechos de inicio de sesión interactivo. Los usuarios no ven ni pueden interactuar con los programas que se ejecutan con permisos del sistema.
-   Cada tarea ejecuta un solo programa. Sin embargo, puede crear un archivo por lotes que inicie varias tareas y, a continuación, programar una tarea que ejecute el archivo por lotes.
-   Puede probar una tarea en cuanto la cree. Use la operación **Ejecutar** para probar la tarea y, a continuación, compruebe si hay errores en el archivo de SchedLgU.txt (*raízDelSistema*\SchedLgU.txt).

## <a name="schtasks-change"></a><a name=BKMK_change></a>cambio de SchTasks

Cambia una o varias de las siguientes propiedades de una tarea.
-   Programa que ejecuta la tarea (**/TR**).
-   La cuenta de usuario en la que se ejecuta la tarea (**/RU**).
-   La contraseña de la cuenta de usuario (**/RP**).
-   Agrega la propiedad solo Interactive a la tarea (**/it**).

### <a name="syntax"></a>Sintaxis

```
schtasks /change /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/tr <TaskRun>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/{ENABLE | DISABLE}] [/it] [/z]
```

#### <a name="parameters"></a>Parámetros

|          Término           |                                                                                                                                                                                                                                                                                                                                     Definición                                                                                                                                                                                                                                                                                                                                      |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /TN \<TaskName>     |                                                                                                                                                                                                                                                                                                               Identifica la tarea que se va a cambiar. Escriba el nombre de la tarea.                                                                                                                                                                                                                                                                                                               |
|     modificado \<Computer>      |                                                                                                                                                                                                                                                                               Especifica el nombre o la dirección IP de un equipo remoto (con o sin barras diagonales inversas). La opción predeterminada es el equipo local.                                                                                                                                                                                                                                                                               |
|  5.50\<Domain>\]<User>  |                                                                                                                                                                 Ejecuta este comando con los permisos de la cuenta de usuario especificada. El valor predeterminado son los permisos del usuario actual del equipo local. La cuenta de usuario especificada debe ser miembro del grupo administradores en el equipo remoto. Los parámetros **/u** y **/p** solo son válidos para cambiar una tarea en un equipo remoto (**/s**).                                                                                                                                                                  |
|     /p \<Password>      |                                                                                                                                                                                              Especifica la contraseña de la cuenta de usuario especificada en el parámetro **/u** . Si usa el parámetro **/u** , pero omite el parámetro **/p** o el argumento de contraseña, **Schtasks** solicita una contraseña.</br>Los parámetros **/u** y **/p** solo son válidos cuando se utiliza **/s**.                                                                                                                                                                                               |
| /ru {[\<Domain>\]<User> |                                                                                                                                                                                                                                                                                                                                       Integrado                                                                                                                                                                                                                                                                                                                                       |
|     /RP \<Password>     |                                                                                                                                                                                                                                                 Especifica una nueva contraseña para la cuenta de usuario existente o la cuenta de usuario especificada por el parámetro **/RU** . Este parámetro se omite cuando se usa con la cuenta de sistema local.                                                                                                                                                                                                                                                  |
|     /TR \<TaskRun>      |                                                                                                                                                                                  Cambia el programa que ejecuta la tarea. Escriba la ruta de acceso completa y el nombre de archivo de un archivo ejecutable, un archivo de script o un archivo por lotes. Si omite la ruta de acceso, **SchTasks** supone que el archivo se encuentra en el \<systemroot> directorio \System32 El programa especificado reemplaza el programa original ejecutado por la tarea.                                                                                                                                                                                  |
|    /St \<Starttime>     |                                                                                                                                                                                                                                                              Especifica la hora de inicio de la tarea, con el formato de hora de 24 horas, HH: mm. Por ejemplo, un valor de 14:30 es equivalente a la hora de 12 horas de 2:30 PM.                                                                                                                                                                                                                                                               |
|     /RI \<Interval>     |                                                                                                                                                                                                                                                                           Especifica el intervalo de repetición de la tarea programada, en minutos. El intervalo válido es 1-599940 (599940 minutos = 9999 horas).                                                                                                                                                                                                                                                                            |
|     /et \<EndTime>      |                                                                                                                                                                                                                                                               Especifica la hora de finalización de la tarea, con el formato de hora de 24 horas, HH: mm. Por ejemplo, un valor de 14:30 es equivalente a la hora de 12 horas de 2:30 PM.                                                                                                                                                                                                                                                                |
|     /du \<Duration>     |                                                                                                                                                                                                                                                                                                     Especifica que se cierre la tarea en \<EndTime> o <Duration> , si se especifica.                                                                                                                                                                                                                                                                                                      |
|           /k            |                                                                                                                                                                   Detiene el programa que ejecuta la tarea en el momento especificado por **/et** o **/du**. Sin **/k**, **Schtasks** no vuelve a iniciar el programa después de alcanzar el tiempo especificado por **/et** o **/du**, pero no detiene el programa si todavía se está ejecutando. Este parámetro es opcional y solo es válido con una programación por minuto o hora.                                                                                                                                                                   |
|    /SD \<StartDate>     |                                                                                                                                                                                                                                                                                              Especifica la primera fecha en la que se debe ejecutar la tarea. El formato de fecha es MM/DD/AAAA.                                                                                                                                                                                                                                                                                               |
|     /Ed \<EndDate>      |                                                                                                                                                                                                                                                                                                 Especifica la última fecha en la que se debe ejecutar la tarea. El formato es MM/DD/AAAA.                                                                                                                                                                                                                                                                                                  |
|         /ENABLE         |                                                                                                                                                                                                                                                                                                                       Especifica que se va a habilitar la tarea programada.                                                                                                                                                                                                                                                                                                                       |
|        /DISABLE         |                                                                                                                                                                                                                                                                                                                      Especifica que se deshabilite la tarea programada.                                                                                                                                                                                                                                                                                                                       |
|           /It           | Especifica que solo se ejecute la tarea programada cuando el usuario de ejecución (la cuenta de usuario en la que se ejecuta la tarea) ha iniciado sesión en el equipo.</br>Este parámetro no tiene ningún efecto en las tareas que se ejecutan con permisos del sistema o tareas que ya tienen establecida la propiedad solo Interactive. No se puede usar un comando de cambio para quitar la propiedad solo interactiva de una tarea.</br>De forma predeterminada, el usuario de ejecución es el usuario actual del equipo local cuando la tarea está programada o la cuenta especificada por el parámetro **/u** , si se usa una. Sin embargo, si el comando incluye el parámetro **/RU** , el usuario de ejecución es la cuenta especificada por el parámetro **/RU** . |
|           /z            |                                                                                                                                                                                                                                                                                                          Especifica la eliminación de la tarea tras la finalización de su programación.                                                                                                                                                                                                                                                                                                          |
|           /?            |                                                                                                                                                                                                                                                                                                                        Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                                                                                                                                                                         |

### <a name="remarks"></a>Observaciones

-   Los parámetros **/TN** y **/s** identifican la tarea. Los parámetros **/TR**, **/RU**y **/RP** especifican las propiedades de la tarea que puede cambiar.
-   Los parámetros **/RU**y **/RP** especifican los permisos en los que se ejecuta la tarea. Los parámetros **/u** y **/p** especifican los permisos que se usan para cambiar la tarea.
-   Para cambiar las tareas de un equipo remoto, el usuario debe haber iniciado sesión en el equipo local con una cuenta que sea miembro del grupo administradores en el equipo remoto.
-   Para ejecutar un comando **/Change** con los permisos de un usuario diferente (**/u**, **/p**), el equipo local debe estar en el mismo dominio que el equipo remoto o debe estar en un dominio en el que confíe el dominio del equipo remoto.
-   La cuenta del sistema no tiene derechos de inicio de sesión interactivo. Los usuarios no ven ni pueden interactuar con los programas que se ejecutan con permisos del sistema.
-   Para identificar las tareas con la propiedad **/it** , use una consulta detallada (**/query/v**). En una presentación de consulta detallada de una tarea con **/it**, el campo **modo de inicio de sesión** tiene un valor de **solo interactivo**.

### <a name="examples"></a>Ejemplos

### <a name="to-change-the-program-that-a-task-runs"></a>Para cambiar el programa que ejecuta una tarea

El siguiente comando cambia el programa que se ejecuta en la tarea de comprobación de virus desde VirusCheck.exe a VirusCheck2.exe. Este comando usa el parámetro **/TN** para identificar la tarea y el parámetro **/TR** para especificar el nuevo programa para la tarea. (No se puede cambiar el nombre de la tarea).
```
schtasks /change /tn Virus Check /tr C:\VirusCheck2.exe
```
En respuesta, **SchTasks.exe** muestra el siguiente mensaje de confirmación:
```
SUCCESS: The parameters of the scheduled task Virus Check have been changed.
```
Como resultado de este comando, la tarea de comprobación de virus se ejecuta ahora VirusCheck2.exe.

### <a name="to-change-the-password-for-a-remote-task"></a>Para cambiar la contraseña de una tarea remota

El siguiente comando cambia la contraseña de la cuenta de usuario de la tarea RemindMe en el equipo remoto, Svr01. El comando usa el parámetro **/TN** para identificar la tarea y el parámetro **/s** para especificar el equipo remoto. Usa el parámetro **/RP** para especificar la nueva contraseña, p@ssWord3 .

Este procedimiento es necesario siempre que expire o cambie la contraseña de una cuenta de usuario. Si la contraseña guardada en una tarea ya no es válida, la tarea no se ejecuta.
```
schtasks /change /tn RemindMe /s Svr01 /rp p@ssWord3
```
En respuesta, **SchTasks.exe** muestra el siguiente mensaje de confirmación:
```
SUCCESS: The parameters of the scheduled task RemindMe have been changed.
```
Como resultado de este comando, la tarea RemindMe ahora se ejecuta con su cuenta de usuario original, pero con una nueva contraseña.

### <a name="to-change-the-program-and-user-account-for-a-task"></a>Para cambiar el programa y la cuenta de usuario de una tarea

El siguiente comando cambia el programa que ejecuta una tarea y cambia la cuenta de usuario en la que se ejecuta la tarea. Esencialmente, usa una programación antigua para una nueva tarea. Este comando cambia la tarea ChkNews, que comienza Notepad.exe cada mañana a las 9:00 A.M., para iniciar Internet Explorer en su lugar.

El comando usa el parámetro **/TN** para identificar la tarea. Usa el parámetro **/TR** para cambiar el programa que ejecuta la tarea y el parámetro **/RU** para cambiar la cuenta de usuario en la que se ejecuta la tarea.

Se omiten los parámetros **/RU**y **/RP** , que proporcionan la contraseña de la cuenta de usuario. Debe proporcionar una contraseña para la cuenta, pero puede usar el parámetro **/RU**y **/RP** y escribir la contraseña en texto no cifrado, o bien esperar a que **SchTasks.exe** le pida una contraseña y, a continuación, escribir la contraseña en texto oculto.
```
schtasks /change /tn ChkNews /tr c:\program files\Internet Explorer\iexplore.exe /ru DomainX\Admin01
```
En respuesta, **SchTasks.exe** solicita la contraseña de la cuenta de usuario. Oculta el texto que escribe, por lo que la contraseña no es visible.
```
Please enter the password for DomainX\Admin01:
```
Tenga en cuenta que el parámetro **/TN** identifica la tarea y que los parámetros **/TR** y **/RU** cambian las propiedades de la tarea. No puede usar otro parámetro para identificar la tarea y no puede cambiar el nombre de la tarea.

En respuesta, **SchTasks.exe** muestra el siguiente mensaje de confirmación:
```
SUCCESS: The parameters of the scheduled task ChkNews have been changed.
```
Como resultado de este comando, la tarea ChkNews ahora ejecuta Internet Explorer con los permisos de una cuenta de administrador.

### <a name="to-change-a-program-to-the-system-account"></a>Para cambiar un programa a la cuenta del sistema

El siguiente comando cambia la tarea SecurityScript para que se ejecute con los permisos de la cuenta del sistema. Usa el parámetro * */ru * * para indicar la cuenta del sistema.
```
schtasks /change /tn SecurityScript /ru
```
En respuesta, **SchTasks.exe** muestra el siguiente mensaje de confirmación:
```
INFO: The run as user name for the scheduled task SecurityScript will be changed to NT AUTHORITY\SYSTEM.
SUCCESS: The parameters of the scheduled task SecurityScript have been changed.
```
Dado que las tareas que se ejecutan con permisos de cuenta del sistema no requieren una contraseña, **SchTasks.exe** no solicita una.

### <a name="to-run-a-program-only-when-i-am-logged-on"></a>Para ejecutar un programa solo cuando he iniciado sesión

El comando siguiente agrega la propiedad solo Interactive a MyApp, una tarea existente. Esta propiedad garantiza que la tarea se ejecute solo cuando el usuario de ejecución, es decir, la cuenta de usuario en la que se ejecuta la tarea, inicie sesión en el equipo.

El comando usa el parámetro **/TN** para identificar la tarea y el parámetro **/it** para agregar la propiedad solo interactiva a la tarea. Dado que la tarea ya se ejecuta con los permisos de mi cuenta de usuario, no es necesario cambiar el parámetro **/RU** de la tarea.
```
schtasks /change /tn MyApp /it
```
En respuesta, **SchTasks.exe** muestra el siguiente mensaje de operación correcta.
```
SUCCESS: The parameters of the scheduled task MyApp have been changed.
```

## <a name="schtasks-run"></a><a name=BKMK_run></a>ejecución de SchTasks

Inicia inmediatamente una tarea programada. La operación de **ejecución** omite la programación, pero utiliza la ubicación del archivo de programa, la cuenta de usuario y la contraseña guardadas en la tarea para ejecutar la tarea inmediatamente.

### <a name="syntax"></a>Sintaxis

```
schtasks /run /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="parameters"></a>Parámetros

|         Término          |                                                                                                                                                                 Definición                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /TN \<TaskName>    |                                                                                                                                                       Necesario. Identifica la tarea.                                                                                                                                                        |
|    modificado \<Computer>     |                                                                                                           Especifica el nombre o la dirección IP de un equipo remoto (con o sin barras diagonales inversas). La opción predeterminada es el equipo local.                                                                                                           |
| 5.50\<Domain>\]<User> | Ejecuta este comando con los permisos de la cuenta de usuario especificada. De forma predeterminada, el comando se ejecuta con los permisos del usuario actual del equipo local.</br>La cuenta de usuario especificada debe ser miembro del grupo administradores en el equipo remoto. Los parámetros **/u** y **/p** solo son válidos cuando se utiliza **/s**. |
|    /p \<Password>     |                          Especifica la contraseña de la cuenta de usuario especificada en el parámetro **/u** . Si usa el parámetro **/u** , pero omite el parámetro **/p** o el argumento de contraseña, **Schtasks** solicita una contraseña.</br>Los parámetros **/u** y **/p** solo son válidos cuando se utiliza **/s**.                           |
|          /?           |                                                                                                                                                    Muestra la ayuda en el símbolo del sistema.                                                                                                                                                     |

### <a name="remarks"></a>Observaciones

-   Use esta operación para probar sus tareas. Si una tarea no se ejecuta, compruebe si hay errores en el registro de transacciones del servicio de Programador de tareas, \<Systemroot>\SchedLgU.txt.
-   La ejecución de una tarea no afecta a la programación de tareas y no cambia la siguiente hora de ejecución programada para la tarea.
-   Para ejecutar una tarea de forma remota, se debe programar la tarea en el equipo remoto. Al ejecutarlo, la tarea solo se ejecuta en el equipo remoto. Para comprobar que una tarea se ejecuta en un equipo remoto, use el administrador de tareas o el registro de transacciones de Programador de tareas, \<Systemroot>\SchedLgU.txt.

### <a name="examples"></a>Ejemplos

### <a name="to-run-a-task-on-the-local-computer"></a>Para ejecutar una tarea en el equipo local

El comando siguiente inicia la tarea script de seguridad.
```
schtasks /run /tn Security Script
```
En respuesta, **SchTasks.exe** inicia el script asociado a la tarea y muestra el siguiente mensaje:
```
SUCCESS: Attempted to run the scheduled task Security Script.
```
Como implica el mensaje, **SchTasks** intenta iniciar el programa, pero no puede que el programa se inicie realmente.

### <a name="to-run-a-task-on-a-remote-computer"></a>Para ejecutar una tarea en un equipo remoto

El comando siguiente inicia la tarea de actualización en un equipo remoto, Svr01:
```
schtasks /run /tn Update /s Svr01
```
En este caso, **SchTasks.exe** muestra el siguiente mensaje de error:
```
ERROR: Unable to run the scheduled task Update.
```
Para encontrar la causa del error, busque en el registro de transacciones de tareas programadas, C:\Windows\SchedLgU.txt en Svr01. En este caso, la siguiente entrada aparece en el registro:
```
Update.job (update.exe) 3/26/2001 1:15:46 PM ** ERROR **
The attempt to log on to the account associated with the task failed, therefore, the task did not run.
The specific error is:
0x8007052e: Logon failure: unknown user name or bad password.
Verify that the task's Run-as name and password are valid and try again.
```
Aparentemente, el nombre de usuario o la contraseña de la tarea no son válidos en el sistema. El siguiente comando **Schtasks/Change** actualiza el nombre de usuario y la contraseña de la tarea de actualización en Svr01:
```
schtasks /change /tn Update /s Svr01 /ru Administrator /rp PassW@rd3
```
Una vez completado el comando de **cambio** , el comando **Ejecutar** se repite. Esta vez, el programa de Update.exe se inicia y **SchTasks.exe** muestra el siguiente mensaje:
```
SUCCESS: Attempted to run the scheduled task Update.
```
Como implica el mensaje, **SchTasks** intenta iniciar el programa, pero no puede que el programa se inicie realmente.

## <a name="schtasks-end"></a><a name=BKMK_end></a>fin de SchTasks

Detiene un programa iniciado por una tarea.

### <a name="syntax"></a>Sintaxis

```
schtasks /end /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="parameters"></a>Parámetros

|         Término          |                                                                                                                                                               Definición                                                                                                                                                                |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /TN \<TaskName>    |                                                                                                                                         Necesario. Identifica la tarea que inició el programa.                                                                                                                                         |
|    modificado \<Computer>     |                                                                                                                        Especifica el nombre o la dirección IP de un equipo remoto. La opción predeterminada es el equipo local.                                                                                                                        |
| 5.50\<Domain>\]<User> | Ejecuta este comando con los permisos de la cuenta de usuario especificada. De forma predeterminada, el comando se ejecuta con los permisos del usuario actual del equipo local. La cuenta de usuario especificada debe ser miembro del grupo administradores en el equipo remoto. Los parámetros **/u** y **/p** solo son válidos cuando se utiliza **/s**. |
|    /p \<Password>     |                        Especifica la contraseña de la cuenta de usuario especificada en el parámetro **/u** . Si usa el parámetro **/u** , pero omite el parámetro **/p** o el argumento de contraseña, **Schtasks** solicita una contraseña.</br>Los parámetros **/u** y **/p** solo son válidos cuando se utiliza **/s**.                         |
|          /?           |                                                                                                                                                             Muestra información de ayuda.                                                                                                                                                              |

### <a name="remarks"></a>Observaciones

**SchTasks.exe** finaliza únicamente las instancias de un programa iniciadas por una tarea programada. Para detener otros procesos, use TaskKill. Para obtener más información, consulte [Taskkill](taskkill.md).

### <a name="examples"></a>Ejemplos

### <a name="to-end-a-task-on-a-local-computer"></a>Para finalizar una tarea en un equipo local

El comando siguiente detiene la instancia de Notepad.exe iniciada por la tarea My Notepad:
```
schtasks /end /tn My Notepad
```
En respuesta, **SchTasks.exe** detiene la instancia de Notepad.exe que inició la tarea y muestra el siguiente mensaje de confirmación:
```
SUCCESS: The scheduled task My Notepad has been terminated successfully.
```

### <a name="to-end-a-task-on-a-remote-computer"></a>Para finalizar una tarea en un equipo remoto

El comando siguiente detiene la instancia de Internet Explorer iniciada por la tarea de Internet en el equipo remoto, Svr01:
```
schtasks /end /tn InternetOn /s Svr01
```
En respuesta, **SchTasks.exe** detiene la instancia de Internet Explorer que inició la tarea y muestra el siguiente mensaje de confirmación:
```
SUCCESS: The scheduled task InternetOn has been terminated successfully.
```

## <a name="schtasks-delete"></a><a name=BKMK_delete></a>eliminar SchTasks

Elimina una tarea programada.

### <a name="syntax"></a>Sintaxis

```
schtasks /delete /tn {<TaskName> | *} [/f] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="parameters"></a>Parámetros

|         Término          |                                                                                                                                                                 Definición                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   TN\<TaskName>    |                                                                                                                                                                     \*}                                                                                                                                                                     |
|          /f           |                                                                                                                                  Suprime el mensaje de confirmación. La tarea se elimina sin previo aviso.                                                                                                                                  |
|    modificado \<Computer>     |                                                                                                           Especifica el nombre o la dirección IP de un equipo remoto (con o sin barras diagonales inversas). La opción predeterminada es el equipo local.                                                                                                           |
| 5.50\<Domain>\]<User> | Ejecuta este comando con los permisos de la cuenta de usuario especificada. De forma predeterminada, el comando se ejecuta con los permisos del usuario actual del equipo local.</br>La cuenta de usuario especificada debe ser miembro del grupo administradores en el equipo remoto. Los parámetros **/u** y **/p** solo son válidos cuando se utiliza **/s**. |
|    /p \<Password>     |                          Especifica la contraseña de la cuenta de usuario especificada en el parámetro **/u** . Si usa el parámetro **/u** , pero omite el parámetro **/p** o el argumento de contraseña, **Schtasks** solicita una contraseña.</br>Los parámetros **/u** y **/p** solo son válidos cuando se utiliza **/s**.                           |
|          /?           |                                                                                                                                                    Muestra la ayuda en el símbolo del sistema.                                                                                                                                                     |

### <a name="remarks"></a>Observaciones

- La operación de **eliminación** elimina la tarea de la programación. No elimina el programa que ejecuta la tarea ni interrumpe un programa en ejecución.
- El **comando \\ Delete *** elimina todas las tareas programadas para el equipo, no solo las tareas programadas por el usuario actual.

### <a name="examples"></a>Ejemplos

### <a name="to-delete-a-task-from-the-schedule-of-a-remote-computer"></a>Para eliminar una tarea de la programación de un equipo remoto

El siguiente comando elimina la tarea iniciar correo de la programación de un equipo remoto. Usa el parámetro **/s** para identificar el equipo remoto.
```
schtasks /delete /tn Start Mail /s Svr16
```
En respuesta, **SchTasks.exe** muestra el siguiente mensaje de confirmación. Para eliminar la tarea, presione Y<strong>.</strong> Para cancelar el comando, escriba **n**:
```
WARNING: Are you sure you want to remove the task Start Mail (Y/N )?
SUCCESS: The scheduled task Start Mail was successfully deleted.
```

### <a name="to-delete-all-tasks-scheduled-for-the-local-computer"></a>Para eliminar todas las tareas programadas para el equipo local

El comando siguiente elimina todas las tareas de la programación del equipo local, incluidas las tareas programadas por otros usuarios. Usa el parámetro **/TN \\ *** para representar todas las tareas del equipo y el parámetro **/f** para suprimir el mensaje de confirmación.
```
schtasks /delete /tn * /f
```
En respuesta, **SchTasks.exe** muestra los siguientes mensajes de éxito que indican que la única tarea programada, SecureScript, se elimina.

`SUCCESS: The scheduled task SecureScript was successfully deleted.`

## <a name="schtasks-query"></a><a name=BKMK_query></a>Schtasks (consulta)

Muestra las tareas programadas para ejecutarse en el equipo.

### <a name="syntax"></a>Sintaxis

```
schtasks [/query] [/fo {TABLE | LIST | CSV}] [/nh] [/v] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="parameters"></a>Parámetros

|         Término          |                                                                                                                                                                 Definición                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /Query        |                                                                                                                        El nombre de la operación es opcional. Al escribir **SchTasks** sin ningún parámetro, se realiza una consulta.                                                                                                                         |
|      /FO \<format>    |  Especifica el formato de salida. Los valores válidos son TABLE, LIST y CSV.                                                                                                                                 |
|          /NH          |                                                                                                            Omite los encabezados de columna de la visualización de la tabla. Este parámetro es válido con los formatos de salida de **tabla** y **CSV** .                                                                                                             |
|          /v           |                                                                                                         Agrega propiedades avanzadas de las tareas a la pantalla.</br>Las consultas que usan **/v** deben tener el formato **List** o **CSV**.                                                                                                          |
|    modificado \<Computer>     |                                                                                                           Especifica el nombre o la dirección IP de un equipo remoto (con o sin barras diagonales inversas). La opción predeterminada es el equipo local.                                                                                                           |
| 5.50\<Domain>\]<User> | Ejecuta este comando con los permisos de la cuenta de usuario especificada. De forma predeterminada, el comando se ejecuta con los permisos del usuario actual del equipo local.</br>La cuenta de usuario especificada debe ser miembro del grupo administradores en el equipo remoto. Los parámetros **/u** y **/p** solo son válidos cuando se utiliza **/s**. |
|    /p \<Password>     |                                        Especifica la contraseña de la cuenta de usuario especificada en el parámetro **/u** . Si usa **/u**, pero omite **/p** o el argumento de contraseña, **Schtasks** solicita una contraseña.</br>Los parámetros **/u** y **/p** solo son válidos cuando se utiliza **/s**.                                         |
|          /?           |                                                                                                                                                    Muestra la ayuda en el símbolo del sistema.                                                                                                                                                     |

### <a name="remarks"></a>Observaciones

**SchTasks.exe** finaliza únicamente las instancias de un programa iniciadas por una tarea programada. Para detener otros procesos, use TaskKill. Para obtener más información, consulte [Taskkill](taskkill.md).

### <a name="examples"></a>Ejemplos

### <a name="to-display-the-scheduled-tasks-on-the-local-computer"></a>Para mostrar las tareas programadas en el equipo local

Los siguientes comandos muestran todas las tareas programadas para el equipo local. Estos comandos generan el mismo resultado y se pueden usar indistintamente.
```
schtasks
schtasks /query
```
En respuesta, **SchTasks.exe** muestra las tareas en el formato de tabla simple predeterminado, tal como se muestra en la tabla siguiente:
```
TaskName Next Run Time Status
========================= ======================== ==============
Microsoft Outlook At logon time
SecureScript 14:42:00 PM , 2/4/2001
```

### <a name="to-display-advanced-properties-of-scheduled-tasks"></a>Para mostrar las propiedades avanzadas de las tareas programadas

El comando siguiente solicita una presentación detallada de las tareas en el equipo local. Usa el parámetro **/v** para solicitar una presentación detallada (detallada) y el parámetro **/FO List** para dar formato a la presentación como una lista para facilitar la lectura. Puede usar este comando para comprobar que una tarea que ha creado tiene el patrón de periodicidad previsto.

**Schtasks/Query/FO LIST/v**

En respuesta, en **SchTasks.exe** se muestra una lista de propiedades detallada para todas las tareas. La siguiente pantalla muestra la lista de tareas de una tarea programada para ejecutarse a las 4:00 A.M. el último viernes de cada mes:
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

### <a name="to-log-tasks-scheduled-for-a-remote-computer"></a>Para registrar tareas programadas para un equipo remoto

El comando siguiente solicita una lista de tareas programadas para un equipo remoto y agrega las tareas a un archivo de registro separado por comas en el equipo local. Puede usar este formato de comando para recopilar y realizar un seguimiento de las tareas programadas para varios equipos.

El comando usa el parámetro **/s** para identificar el equipo remoto, Reskit16, el parámetro **/FO** para especificar el formato y el parámetro **/NH** para suprimir los encabezados de columna. El **>>** Símbolo Append redirige la salida al registro de tareas p0102.csv, en el equipo local, Svr01. Dado que el comando se ejecuta en el equipo remoto, la ruta de acceso del equipo local debe ser completa.
```
schtasks /query /s Reskit16 /fo csv /nh >> \\svr01\data\tasklogs\p0102.csv
```
En respuesta, **SchTasks.exe** agrega las tareas programadas para el equipo Reskit16 al archivo de p0102.csv en el equipo local, Svr01.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
