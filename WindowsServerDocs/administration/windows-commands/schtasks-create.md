---
title: crear SchTasks
description: Artículo de referencia para el comando Schtasks Create, que
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/16/2020
ms.openlocfilehash: 4a7c3eb621f4c3d640fcb1f057b9d3cd00834e31
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91390819"
---
# <a name="schtasks-create"></a>crear SchTasks

Programa una tarea.

## <a name="syntax"></a>Sintaxis

```
schtasks /create /sc <scheduletype> /tn <taskname> /tr <taskrun> [/s <computer> [/u [<domain>\]<user> [/p <password>]]] [/ru {[<domain>\]<user> | system}] [/rp <password>] [/mo <modifier>] [/d <day>[,<day>...] | *] [/m <month>[,<month>...]] [/i <idletime>] [/st <starttime>] [/ri <interval>] [{/et <endtime> | /du <duration>} [/k]] [/sd <startdate>] [/ed <enddate>] [/it] [/z] [/f]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| once `<scheduletype>` | Especifica el tipo de programación. Los valores válidos son:<ul><li>**Minute** : especifica el número de minutos antes de que se ejecute la tarea.</li><li>**Cada hora** : especifica el número de horas antes de que se ejecute la tarea.</li><li>**Daily** : especifica el número de días antes de que se ejecute la tarea.</li><li>**Semanalmente** Especifica el número de semanas antes de que se ejecute la tarea.</li><li>**Mensual** : especifica el número de meses antes de que se ejecute la tarea.</li><li>**Una vez** : especifica que la tarea se ejecuta una vez en una fecha y hora especificadas.</li><li>**OnStart** : especifica que la tarea se ejecuta cada vez que se inicia el sistema. Puede especificar una fecha de inicio o ejecutar la tarea la próxima vez que se inicie el sistema.</li><li>**ONLOGON** : especifica que la tarea se ejecuta cada vez que un usuario (cualquier usuario) inicie sesión. Puede especificar una fecha o ejecutar la tarea la próxima vez que el usuario inicie sesión.</li><li>**OnIdle** : especifica que la tarea se ejecuta cuando el sistema está inactivo durante un período de tiempo especificado. Puede especificar una fecha o ejecutar la tarea la próxima vez que el sistema esté inactivo.</li></ul> |
| /TN `<taskname>` | Especifica un nombre para la tarea. Cada tarea del sistema debe tener un nombre único y debe ajustarse a las reglas de los nombres de archivo, sin superar los 238 caracteres. Utilice comillas para encerrar los nombres que incluyen espacios. |
| /TR `<Taskrun>` | Especifica el programa o comando que ejecuta la tarea. Escriba la ruta de acceso completa y el nombre de archivo de un archivo ejecutable, un archivo de script o un archivo por lotes. El nombre de la ruta de acceso no debe superar los 262 caracteres. Si no agrega la ruta de acceso, **SchTasks** supone que el archivo se encuentra en el `<systemroot>\System32` directorio. |
| modificado `<computer>` | Especifica el nombre o la dirección IP de un equipo remoto (con o sin barras diagonales inversas). La opción predeterminada es el equipo local. |
| /u `[<domain>]` | Ejecuta este comando con los permisos de la cuenta de usuario especificada. El valor predeterminado son los permisos del usuario actual del equipo local. Los parámetros **/u** y **/p** solo son válidos cuando se utiliza **/s**. Los permisos de la cuenta especificada se usan para programar la tarea y para ejecutar la tarea. Para ejecutar la tarea con los permisos de otro usuario, use el parámetro **/RU** . La cuenta de usuario debe ser miembro del grupo administradores en el equipo remoto. Además, el equipo local debe estar en el mismo dominio que el equipo remoto, o bien debe estar en un dominio que sea de confianza para el dominio del equipo remoto. |
| /p `<password>` | Especifica la contraseña de la cuenta de usuario especificada en el parámetro **/u** . Si usa el parámetro **/u** sin el parámetro **/p** o el argumento password, SchTasks le solicitará una contraseña. Los parámetros **/u** y **/p** solo son válidos cuando se utiliza **/s**. |
| /RU `{[<domain>\]<user> | system}` | Ejecuta la tarea con los permisos de la cuenta de usuario especificada. De forma predeterminada, la tarea se ejecuta con los permisos del usuario actual del equipo local o con el permiso del usuario especificado mediante el parámetro **/u** , si se incluye uno. El parámetro **/RU** es válido cuando se programan tareas en equipos locales o remotos. Entre las opciones válidas se incluyen:<ul><li>**Dominio** : especifica una cuenta de usuario alternativa.</li><li>**Sistema** : especifica la cuenta del sistema local, una cuenta con privilegios elevados usada por el sistema operativo y los servicios del sistema.</li></ul> |
| /RP `<password>` | Especifica la contraseña de la cuenta de usuario existente o la cuenta de usuario especificada por el parámetro **/RU** . Si no usa este parámetro al especificar una cuenta de usuario, SchTasks.exe le pedirá la contraseña la próxima vez que inicie sesión. No use el parámetro **/RP** para las tareas que se ejecutan con las credenciales de la cuenta del sistema (**/RU System**). La cuenta del sistema no tiene una contraseña y SchTasks.exe no solicita una. |
| /mes `<modifiers>` | Especifica la frecuencia con que se ejecuta la tarea dentro de su tipo de programación. Entre las opciones válidas se incluyen:<ul><li>**Minute** : especifica que la tarea se ejecuta cada <n> minutos. Puede usar cualquier valor entre 1-1439 minutos. De forma predeterminada, es 1 minuto.</li><li>Cada **hora** : especifica que la tarea se ejecuta cada <n> horas. Puede usar cualquier valor entre 1-23 horas. De forma predeterminada, es 1 hora.</li><li>**Daily** : especifica que la tarea se ejecuta cada <n> días. Puede usar cualquier valor entre 1-365 días. De forma predeterminada, es 1 día.</li><li>**Semanal** : especifica que la tarea se ejecuta cada <n> semana. Puede usar cualquier valor entre 1-52 semanas. De forma predeterminada, es de 1 semana.</li><li>**Mensual** : especifica que la tarea se ejecuta cada <n> meses. Puede usar cualquiera de los siguientes valores:<ul><li>Un número entre 1-12 meses</li><li>**LastDay** : para ejecutar la tarea el último día del mes</li><li>**Primero, segundo, tercero o cuarto junto con el `/d <day>` parámetro** : especifica la semana y el día determinados para ejecutar la tarea. Por ejemplo, el tercer miércoles del mes.</li></ul></li><li>**Una vez** : especifica que la tarea se ejecuta una vez.</li><li>**OnStart** : especifica que la tarea se ejecuta en el inicio.</li><li>**ONLOGON** : especifica que la tarea se ejecuta cuando el usuario especificado por el parámetro **/u** inicia sesión.</li><li>**OnIdle** : especifica que la tarea se ejecuta después de que el sistema esté inactivo durante el número de minutos especificado por el parámetro **/i**</li></ul> |
| /d DAY [, DAY...] | Especifica la frecuencia con que se ejecuta la tarea dentro de su tipo de programación. Entre las opciones válidas se incluyen:<ul><li>**Semanal** : especifica que la tarea se ejecuta semanalmente proporcionando un valor entre 1-52 semanas. Opcionalmente, también puede Agregar un determinado día de la semana agregando un valor de MON-SUN o un intervalo de [MON-SUN...]).</li><li>**Mensual** : especifica que la tarea se ejecuta semanalmente cada mes; para ello, se proporciona un valor de primero, segundo, tercero, cuarto, último. Opcionalmente, también puede Agregar un determinado día de la semana agregando un valor de Lun-Dom o proporcionando un número entre 1-12 meses. Si utiliza esta opción, también puede Agregar un determinado día del mes, proporcionando un número entre 1-31.<p>**Nota:** El valor de fecha 1-31 solo es válido sin el parámetro **/mo** , o si el parámetro **/mo** es mensualmente (1-12). El valor predeterminado es Day 1 (el primer día del mes).</li></ul> |
| /m mes [, mes...] | Especifica un mes o meses del año en los que se debe ejecutar la tarea programada. Entre las opciones válidas se incluyen JAN-DEC y `*` (cada mes). El parámetro **/m** solo es válido con una programación mensual. Se requiere cuando se usa el modificador LASTDAY. De lo contrario, es opcional y el valor predeterminado es `*` (cada mes). |
| /i <Idletime> | Especifica el número de minutos que el equipo está inactivo antes de que se inicie la tarea. Un valor válido es un número entero comprendido entre 1 y 999. Este parámetro solo es válido con una programación OnIdle y, a continuación, es necesario. |
| /St `<Starttime>` | Especifica la hora de inicio de la tarea, con el formato de hora de 24 horas, HH: mm. El valor predeterminado es la hora actual en el equipo local. El parámetro **/St** es válido con programaciones de minuto, hora, diaria, semanal, mensual y una vez. Es necesario para una programación una vez. |
| /RI `<interval>` | Especifica el intervalo de repetición de la tarea programada, en minutos. Esto no es aplicable a los tipos de programación: MINUTE, HOURly, OnStart, ONLOGON y OnIdle. El intervalo válido es 1-599940 (599940 minutos = 9999 horas). Si se especifican los parámetros **/et** o **/du** , el valor predeterminado es **10 minutos**. |
| /et `<Endtime>` | Especifica la hora del día a la que finaliza un programa de tareas por minuto o por hora en <HH: MM> formato de 24 horas. Después de la hora de finalización especificada, Schtasks no vuelve a iniciar la tarea hasta que se repite la hora de inicio. De forma predeterminada, las programaciones de tareas no tienen ninguna hora de finalización. Este parámetro es opcional y solo es válido con una programación por minuto o hora. |
| /du `<duration>` | Especifica un período de tiempo máximo para una programación de minuto o hora en <HHHH: MM> formato de 24 horas. Una vez transcurrido el tiempo especificado, Schtasks no vuelve a iniciar la tarea hasta que se repite la hora de inicio. De forma predeterminada, las programaciones de tareas no tienen una duración máxima. Este parámetro es opcional y solo es válido con una programación por minuto o hora. |
| /k | Detiene el programa que ejecuta la tarea en el momento especificado por **/et** o **/du**. Sin **/k**, Schtasks no vuelve a iniciar el programa después de alcanzar el tiempo especificado por **/et** o **/du** , ni detiene el programa si aún se está ejecutando. Este parámetro es opcional y solo es válido con una programación por minuto o hora. |
| /SD <Startdate> | Especifica la fecha en la que comienza la programación de tareas. El valor predeterminado es la fecha actual en el equipo local. El formato de **startDate** varía según la configuración regional seleccionada para el equipo local en **Opciones regionales y de idioma**. Solo un formato es válido para cada configuración regional. Los formatos de fecha válidos incluyen (Asegúrese de elegir el formato más similar al formato seleccionado en **fecha corta** en **Opciones regionales y de idioma** en el equipo local):<ul><li>`<MM>//` : Especifica que se deben usar formatos de primer mes, como inglés (Estados Unidos) y español (Panamá).</li><li>`<DD>//` : Especifica que se deben usar formatos de primer día, como búlgaro y holandés (Países Bajos).</li><li>`<YYYY>//` : Especifica que se debe usar para los formatos de año primero, como sueco y francés (Canadá).</li></ul> |
| /Ed `<Enddate>` | Especifica la fecha de finalización de la programación. Este parámetro es opcional. No es válido en una programación ONCE, OnStart, ONLOGON o OnIdle. De forma predeterminada, las programaciones no tienen fecha de finalización. El valor predeterminado es la fecha actual en el equipo local. El formato de **EndDate** varía según la configuración regional seleccionada para el equipo local en **Opciones regionales y de idioma**. Solo un formato es válido para cada configuración regional. Los formatos de fecha válidos incluyen (Asegúrese de elegir el formato más similar al formato seleccionado en **fecha corta** en **Opciones regionales y de idioma** en el equipo local):<ul><li>`<MM>//` : Especifica que se deben usar formatos de primer mes, como inglés (Estados Unidos) y español (Panamá).</li><li>`<DD>//` : Especifica que se deben usar formatos de primer día, como búlgaro y holandés (Países Bajos).</li><li>`<YYYY>//` : Especifica que se debe usar para los formatos de año primero, como sueco y francés (Canadá).</li></ul> |
| /It | Especifica que solo se ejecute la tarea programada cuando el usuario de ejecución (la cuenta de usuario en la que se ejecuta la tarea) ha iniciado sesión en el equipo. Este parámetro no tiene ningún efecto en las tareas que se ejecutan con permisos del sistema o tareas que ya tienen establecida la propiedad solo Interactive. No se puede usar un comando de cambio para quitar la propiedad solo interactiva de una tarea. De forma predeterminada, ejecutar como usuario es el usuario actual del equipo local cuando la tarea está programada o la cuenta especificada por el parámetro **/u** , si se usa una. Sin embargo, si el comando incluye el parámetro **/RU** , el usuario de ejecución es la cuenta especificada por el parámetro **/RU** . |
| /z | Especifica la eliminación de la tarea tras la finalización de su programación. |
| /f | Especifica que se cree la tarea y se suprimen las advertencias si la tarea especificada ya existe. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="to-schedule-a-task-to-run-every-n-minutes"></a>Para programar una tarea para que se ejecute cada `<n>` minutos

En una programación por minuto, el parámetro **/SC minute** es obligatorio. El parámetro **/mo** (modificador) es opcional y especifica el número de minutos entre cada ejecución de la tarea. El valor predeterminado de **/mo** es *1* (cada minuto). Los parámetros **/et** (hora de finalización) y **/du** (duración) son opcionales y se pueden usar con o sin el parámetro **/k** (Finalizar tarea).

### <a name="examples"></a>Ejemplos

- Para programar un script de seguridad, *sec. vbs*, para que se ejecute cada 20 minutos, escriba:

    ```
    schtasks /create /sc minute /mo 20 /tn Security Script /tr \\central\data\scripts\sec.vbs
    ```

    Dado que en este ejemplo no se incluye una fecha o una hora de inicio, la tarea se inicia 20 minutos después de que se complete el comando y se ejecuta cada 20 minutos después de que se ejecute el sistema. Tenga en cuenta que el archivo de origen del script de seguridad se encuentra en un equipo remoto, pero que la tarea está programada y se ejecuta en el equipo local.

- Para programar un script de seguridad, *sec. vbs*, para que se ejecute en el equipo local cada 100 minutos entre 5:00 P.M. y 7:59 A.M. cada día, escriba:

    ```
    schtasks /create /tn Security Script /tr sec.vbs /sc minute /mo 100 /st 17:00 /et 08:00 /k
    ```

    En este ejemplo se usa el parámetro **/SC** para especificar una programación de minutos y el parámetro **/mo** para especificar un intervalo de 100 minutos. Usa los parámetros **/St** y **/et** para especificar la hora de inicio y la hora de finalización de la programación de cada día. También usa el parámetro **/k** para detener el script si sigue ejecutándose a las 7:59 A.M. Sin **/k**, Schtasks no iniciaría el script después de las 7:59 a.m., pero si la instancia se inicia a las 6:20 a.m. todavía se estaba ejecutando, no lo detendría.

## <a name="to-schedule-a-task-to-run-every-n-hours"></a>Para programar una tarea para que se ejecute cada `<n>` horas

En una programación por hora, se requiere el parámetro **/SC HOURLY** . El parámetro **/mo** (modificador) es opcional y especifica el número de horas entre cada ejecución de la tarea. El valor predeterminado de **/mo** es *1* (cada hora). El parámetro **/k** (Finalizar tarea) es opcional y se puede usar con **/et** (finalizar en el momento especificado) o **/du** (finalizar después del intervalo especificado).

### <a name="examples"></a>Ejemplos

- Para programar el programa MyApp para que se ejecute cada cinco horas, a partir del primer día del 2002 de marzo, escriba:

    ```
    schtasks /create /sc hourly /mo 5 /sd 03/01/2002 /tn My App /tr c:\apps\myapp.exe
    ```

    En este ejemplo, el equipo local utiliza la opción **Inglés (Zimbabue)** en **las opciones de configuración regional y de idioma**, por lo que el formato de la fecha de inicio es MM/DD/AAAA (03/01/2002).

- Para programar el programa MyApp para que se ejecute cada hora, empezando en cinco minutos después de la medianoche, escriba:

    ```
    schtasks /create /sc hourly /st 00:05 /tn My App /tr c:\apps\myapp.exe
    ```

- Para programar que el programa MyApp se ejecute cada 3 horas, durante 10 horas en total, escriba:

    ```
    schtasks /create /tn My App /tr myapp.exe /sc hourly /mo 3 /st 00:00 /du 0010:00
    ```

    En este ejemplo, la tarea se ejecuta a las 12:00 A.M., 3:00 A.M., 6:00 A.M. y 9:00 A.M. Dado que la duración es de 10 horas, la tarea no se ejecuta de nuevo a las 12:00 P.M. En su lugar, se inicia de nuevo a las 12:00 A.M. el día siguiente. Además, dado que el programa se ejecuta durante unos minutos, el parámetro **/k** , que detiene el programa si todavía se está ejecutando cuando expira la duración, no es necesario.

## <a name="to-schedule-a-task-to-run-every-n-days"></a>Para programar una tarea para que se ejecute cada `<n>` días

En una programación diaria, se requiere el parámetro **/SC Daily** . El parámetro **/mo** (modificador) es opcional y especifica el número de días entre cada ejecución de la tarea. El valor predeterminado de **/mo** es *1* (cada día).

### <a name="examples"></a>Ejemplos

- Para programar el programa MyApp para que se ejecute una vez al día, cada día, a las 8:00 A.M. hasta el 31 de diciembre de 2021, escriba:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc daily /st 08:00 /ed 31/12/2021
    ```

     En este ejemplo, el sistema del equipo local se establece en la opción **Inglés (Reino Unido)** de **las opciones de configuración regional y de idioma**, por lo que el formato de la fecha de finalización es DD/MM/AAAA (31/12/2021). Además, como en este ejemplo no se incluye el parámetro **/mo** , el intervalo predeterminado de *1* se usa para ejecutar el comando cada día.

- Para programar el programa MyApp para que se ejecute cada doce días a las 1:00 P.M. (13:00) a partir del 31 de diciembre de 2021, escriba:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc daily /mo 12 /sd 12/31/2002 /st 13:00
    ```

    En este ejemplo, el sistema se establece en la opción **Inglés (Zimbabue)** en **las opciones de configuración regional y de idioma**, por lo que el formato de la fecha de finalización es MM/DD/AAAA (12/31/2021).

- Para programar un script de seguridad, *sec. vbs*, para que se ejecute cada 70 días, escriba:

    ```
    schtasks /create /tn Security Script /tr sec.vbs /sc daily /mo 70 /it
    ```

    En este ejemplo, el parámetro **/it** se usa para especificar que la tarea se ejecuta solo cuando el usuario de la cuenta que ejecuta la tarea inicia sesión en el equipo. Dado que la tarea se ejecuta con los permisos de una cuenta de usuario específica, esta tarea solo se ejecuta cuando el usuario ha iniciado sesión.

    > [!NOTE]
    > Para identificar las tareas con la propiedad solo Interactive (**/it**), use una consulta detallada (**/query/v**). En una presentación de consulta detallada de una tarea con/it, el campo **modo de inicio de sesión** tiene un valor de solo interactivo.

## <a name="to-schedule-a-task-to-run-every-n-weeks"></a>Para programar una tarea para que se ejecute cada `<n>` semana

En una programación semanal, se requiere el parámetro **/SC Weekly** . El parámetro **/mo** (modificador) es opcional y especifica el número de semanas entre cada ejecución de la tarea. El valor predeterminado de **/mo** es *1* (cada semana).

Las programaciones semanales también tienen un parámetro opcional **/d** para programar la ejecución de la tarea en los días de la semana especificados o en todos los días (). El valor predeterminado es *LUN (lunes)*. La opción todos los días () equivale a programar una tarea diaria.

### <a name="examples"></a>Ejemplos

- Para programar el programa MyApp para que se ejecute en un equipo remoto cada seis semanas, escriba:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /mo 6 /s Server16 /u Admin01
    ```

    Dado que este ejemplo deja el parámetro **/d** , la tarea se ejecuta los lunes. En este ejemplo también se usa el parámetro **/s** para especificar el equipo remoto y el parámetro **/u** para ejecutar el comando con los permisos de la cuenta de administrador del usuario. Además, dado que el parámetro **/p** se deja, SchTasks.exe solicita al usuario la contraseña de la cuenta de administrador y, dado que el comando se ejecuta de forma remota, todas las rutas de acceso en el comando, incluida la ruta de acceso a MyApp.exe, hacen referencia a las rutas del equipo remoto.

- Para programar una tarea para que se ejecute cada dos viernes, escriba:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /mo 2 /d FRI
    ```

    En este ejemplo se usa el parámetro **/mo** para especificar el intervalo de dos semanas y el parámetro **/d** para especificar el día de la semana. Para programar una tarea que se ejecute todos los viernes, deje el parámetro **/mo** o establézcalo en *1*.

## <a name="to-schedule-a-task-to-run-every-n-months"></a>Para programar una tarea para que se ejecute cada `<n>` meses

En este tipo de programación, se requiere el parámetro **/SC Monthly** . El parámetro **/mo** (modificador), que especifica el número de meses entre cada ejecución de la tarea, es opcional y el valor predeterminado es *1* (cada mes). Este tipo de programación también tiene un parámetro opcional **/d** para programar la ejecución de la tarea en una fecha determinada del mes. El valor predeterminado es *1* (el primer día del mes).

### <a name="examples"></a>Ejemplos

- Para programar que el programa MyApp se ejecute el primer día de cada mes, escriba:

    ```
    schtasks /create /tn My App /tr myapp.exe /sc monthly
    ```

    El valor predeterminado para los parámetros **/mo** (modificador) y **/d** (día) es *1*, por lo que no es necesario usar ninguno de estos parámetros en este ejemplo.

- Para programar el programa MyApp para que se ejecute cada tres meses, escriba:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo 3
    ```

    En este ejemplo se usa el parámetro **/mo** para especificar un intervalo de 3 meses.

- Para programar el programa MyApp de forma que se ejecute todos los meses del día 21 del mes a medianoche durante un año, del 2 de julio de 2002 al 30 de junio de 2003, escriba:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo 2 /d 21 /st 00:00 /sd 2002/07/01 /ed 2003/06/30
    ```

    En este ejemplo se usa el parámetro **/mo** para especificar el intervalo mensual (cada dos meses), el parámetro **/d** para especificar la fecha, el parámetro **/St** para especificar la hora y los parámetros **/SD** y **/Ed** para especificar la fecha de inicio y la fecha de finalización, respectivamente. También en este ejemplo, el equipo local se establece en la opción **Inglés (Sudáfrica)** de **las opciones de configuración regional y de idioma**, por lo que las fechas se especifican en el formato local, AAAA/MM/DD.

## <a name="to-schedule-a-task-to-run-on-a-specific-day-of-the-week"></a>Para programar la ejecución de una tarea en un día concreto de la semana

La programación del día de la semana es una variación de la programación semanal. En una programación semanal, se requiere el parámetro **/SC Weekly** . El parámetro **/mo** (modificador) es opcional y especifica el número de semanas entre cada ejecución de la tarea. El valor predeterminado de **/mo** es *1* (cada semana). El parámetro **/d** , que es opcional, programa la tarea para que se ejecute en los días de la semana especificados o en todos los días (**&#42;**). El valor predeterminado es *LUN (lunes)*. La opción todos los días `(/d *)` es equivalente a programar una tarea diaria.

### <a name="examples"></a>Ejemplos

- Para programar el programa MyApp para que se ejecute cada semana el miércoles, escriba:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /d WED
    ```

    En este ejemplo se usa el parámetro **/d** para especificar el día de la semana. Dado que el comando deja el parámetro **/mo** , la tarea se ejecuta cada semana.

- Para programar una tarea para que se ejecute el lunes y el viernes de cada octava semana, escriba:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /mo 8 /d MON,FRI
    ```

    En este ejemplo se usa el parámetro **/d** para especificar los días y el parámetro **/mo** para especificar el intervalo de ocho semanas.

## <a name="to-schedule-a-task-to-run-on-a-specific-week-of-the-month"></a>Para programar una tarea para que se ejecute en una semana específica del mes

En este tipo de programación, es necesario el parámetro **/SC Monthly** , el parámetro **/mo** (modificador) y el parámetro **/d** (Day). El parámetro **/mo** (modificador) especifica la semana en la que se ejecuta la tarea. El parámetro **/d** especifica el día de la semana. Solo puede especificar un día de la semana para este tipo de programación. Esta programación también tiene un parámetro **/m** (mes) opcional que le permite programar la tarea para meses concretos o cada mes (**&#42;**). El valor predeterminado del parámetro **/m** es cada mes (**&#42;**).

### <a name="examples"></a>Ejemplos

- Para programar que el programa MyApp se ejecute el segundo domingo de cada mes, escriba:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo SECOND /d SUN
    ```

    En este ejemplo se usa el parámetro **/mo** para especificar la segunda semana del mes y el parámetro **/d** para especificar el día.

- Para programar que el programa MyApp se ejecute el primer lunes de marzo y septiembre, escriba:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo FIRST /d MON /m MAR,SEP
    ```

    En este ejemplo se usa el parámetro **/mo** para especificar la primera semana del mes y el parámetro **/d** para especificar el día. Usa el parámetro **/m** para especificar el mes, separando los argumentos de mes con una coma.

## <a name="to-schedule-a-task-to-run-on-a-specific-day-each-month"></a>Para programar la ejecución de una tarea en un día concreto cada mes

En este tipo de programación, es necesario el parámetro **/SC Monthly** y el parámetro **/d** (Day). El parámetro **/d** especifica una fecha del mes (1-31), no un día de la semana, y solo puede especificar un día en la programación. El parámetro **/m** (mes) es opcional y su valor predeterminado es cada mes *()*, mientras que el parámetro **/mo** (modificador) no es válido con este tipo de programación.

Schtasks.exe no le permitirá programar una tarea para una fecha que no esté en un mes especificado por el parámetro **/m** . Por ejemplo, al intentar programar el día 31 de febrero. Sin embargo, si no usa el parámetro **/m** y programa una tarea para una fecha que no aparece en cada mes, la tarea no se ejecutará en los meses más cortos. Para programar una tarea para el último día del mes, use el tipo de programación del último día.

### <a name="examples"></a>Ejemplos

- Para programar que el programa MyApp se ejecute el primer día de cada mes, escriba:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly
    ```

    Dado que el modificador predeterminado es *None* (sin modificador), este comando usa el día predeterminado de *1*y el mes predeterminado de *cada mes*, sin necesidad de ningún parámetro adicional.

- Para programar el programa MyApp para que se ejecute el 15 de mayo y el 15 de junio a las 3:00 P.M. (15:00), escriba:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /d 15 /m MAY,JUN /st 15:00
    ```

    En este ejemplo se usa el parámetro **/d** para especificar la fecha y el parámetro **/m** para especificar los meses. También usa el parámetro **/St** para especificar la hora de inicio.

## <a name="to-schedule-a-task-to-run-on-the-last-day-of-a-month"></a>Para programar una tarea para que se ejecute el último día de un mes

En el tipo de programación del último día, es necesario el parámetro **/SC Monthly** , el parámetro **/mo lastDay** (modificador) y el parámetro **/m** (month). El parámetro **/d** (Day) no es válido.

### <a name="examples"></a>Ejemplos

- Para programar que el programa MyApp se ejecute el último día de cada mes, escriba:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo lastday /m *
    ```

    En este ejemplo se usa el parámetro **/mo** para especificar el último día y el parámetro **/m** con el carácter comodín (**&#42;**) para indicar que el programa se ejecuta cada mes.

- Para programar que el programa MyApp se ejecute el último día de febrero y el último día de marzo a las 6:00 P.M., escriba:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo lastday /m FEB,MAR /st 18:00
    ```

    En este ejemplo se usa el parámetro **/mo** para especificar el último día, el parámetro **/m** para especificar los meses y el parámetro **/St** para especificar la hora de inicio.

## <a name="to-schedule-to-run-once"></a>Para programar para que se ejecute una vez

En el tipo de programación Run-once, se requiere el parámetro **/SC once** . El parámetro **/St** , que especifica el tiempo que se ejecuta la tarea, es obligatorio. El parámetro **/SD** , que especifica la fecha en la que se ejecuta la tarea, es opcional, mientras que los parámetros **/mo** (modificador) y **/Ed** (fecha de finalización) no son válidos.

Schtasks no permite programar una tarea para que se ejecute una vez si la fecha y la hora especificadas están en el pasado, en función de la hora del equipo local. Para programar una tarea que se ejecute una vez en un equipo remoto en una zona horaria diferente, debe programarla antes de que se produzca esa fecha y hora en el equipo local.

### <a name="example"></a>Ejemplo

- Para programar el programa MyApp para que se ejecute a medianoche el 1 de enero de 2003, escriba:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc once /sd 01/01/2003 /st 00:00
    ```

    En este ejemplo se usa el parámetro **/SC** para especificar el tipo de programación y los parámetros **/SD** y **/St** para especificar la fecha y la hora. Además, en este ejemplo, el equipo local usa la opción **Inglés (Estados Unidos)** en **las opciones de configuración regional y de idioma**, el formato de la fecha de inicio es mm/dd/aaaa.

## <a name="to-schedule-a-task-to-run-every-time-the-system-starts"></a>Para programar una tarea para que se ejecute cada vez que se inicie el sistema

En el tipo de programación on-Start, se requiere el parámetro **/SC OnStart** . El parámetro **/SD** (fecha de inicio) es opcional y el valor predeterminado es la fecha actual.

### <a name="example"></a>Ejemplo

- Para programar el programa MyApp para que se ejecute cada vez que se inicie el sistema, a partir del 15 de marzo de 2001, escriba:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc onstart /sd 03/15/2001
    ```

    En este ejemplo, el equipo local usa la opción **Inglés (Estados Unidos)** en **las opciones de configuración regional y de idioma**, el formato de la fecha de inicio es mm/dd/aaaa.

## <a name="to-schedule-a-task-to-run-when-a-user-logs-on"></a>Para programar una tarea para que se ejecute cuando un usuario inicia sesión

El tipo de programación de inicio de sesión programa una tarea que se ejecuta cada vez que un usuario inicia sesión en el equipo. En el tipo de programación al iniciar sesión, se requiere el parámetro **/SC ONLOGON** . El parámetro **/SD** (fecha de inicio) es opcional y el valor predeterminado es la fecha actual.

### <a name="example"></a>Ejemplo

- Para programar una tarea que se ejecuta cuando un usuario inicia sesión en un equipo remoto, escriba:

    ```
    schtasks /create /tn Start Web Site /tr c:\myiis\webstart.bat /sc onlogon /s Server23
    ```

    En este ejemplo se programa un archivo por lotes para que se ejecute cada vez que un usuario (cualquier usuario) inicie sesión en el equipo remoto. Usa el parámetro **/s** para especificar el equipo remoto. Dado que el comando es remoto, todas las rutas de acceso del comando, incluida la ruta de acceso al archivo por lotes, hacen referencia a una ruta de acceso en el equipo remoto.

## <a name="to-schedule-a-task-to-run-when-the-system-is-idle"></a>Para programar la ejecución de una tarea cuando el sistema está inactivo

El tipo en programación inactiva programa una tarea que se ejecuta siempre que no haya ninguna actividad de usuario durante el tiempo especificado por el parámetro **/i** . En el tipo de programación on IDLE, se requieren el parámetro **/SC** OnIdle y el parámetro **/i** . **/SD** (fecha de inicio) es opcional y el valor predeterminado es la fecha actual.

### <a name="example"></a>Ejemplo

- Para programar la ejecución del programa MyApp siempre que el equipo esté inactivo, escriba:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc onidle /i 10
    ```

    En este ejemplo se usa el parámetro **/i** necesario para especificar que el equipo debe permanecer inactivo durante diez minutos antes de que se inicie la tarea.

## <a name="to-schedule-a-task-to-run-now"></a>Para programar la ejecución de una tarea ahora

Schtasks no tiene una opción ejecutar ahora, pero puede simular esa opción creando una tarea que se ejecute una vez y se inicie en unos minutos.

### <a name="example"></a>Ejemplo

- Para programar una tarea para que se ejecute una vez, el 13 de noviembre de 2020 a las 2:18 P.M. hora local, escriba:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc once /st 14:18 /sd 11/13/2002
    ```

    En este ejemplo, el equipo local utiliza la opción **Inglés (Estados Unidos)** en **las opciones de configuración regional y de idioma**, por lo que el formato de la fecha de inicio es mm/dd/aaaa.

## <a name="to-schedule-a-task-that-runs-with-different-permissions"></a>Para programar una tarea que se ejecuta con permisos diferentes

Puede programar tareas de todos los tipos para que se ejecuten con permisos de una cuenta alternativa tanto en el equipo local como en el equipo remoto. Además de los parámetros necesarios para el tipo de programación determinado, el parámetro **/RU** es obligatorio y el parámetro **/RP** es opcional.

### <a name="examples"></a>Ejemplos

- Para ejecutar el programa MyApp en el equipo local, escriba:

    ```
    schtasks /create /tn My App /tr myapp.exe /sc weekly /d TUE /ru Admin06
    ```

    En este ejemplo se usa el parámetro **/RU** para especificar que la tarea debe ejecutarse con los permisos de la cuenta de administrador del usuario (*Admin06*). También en este ejemplo, la tarea está programada para ejecutarse todos los martes, pero puede usar cualquier tipo de programación para la ejecución de una tarea con permisos alternativos.

    En respuesta, SchTasks.exe solicita la contraseña de ejecución para la cuenta de *Admin06* y, a continuación, muestra un mensaje de operación correcta:

    ```
    Please enter the run as password for Admin06: ********
    SUCCESS: The scheduled task My App has successfully been created.
    ```

- Para ejecutar el programa MyApp en el equipo de *marketing* cada cuatro días, escriba:

    ```
    schtasks /create /tn My App /tr myapp.exe /sc daily /mo 4 /s Marketing /u Marketing\Admin01 /ru Reskits\User01
    ```

    En este ejemplo se usa el parámetro **/SC** para especificar una programación diaria y el parámetro **/mo** para especificar un intervalo de cuatro días. Además, en este ejemplo se usa el parámetro **/s** para proporcionar el nombre del equipo remoto y el parámetro **/u** para especificar una cuenta con permiso para programar una tarea en el equipo remoto (*Admin01 en el equipo de marketing*). Por último, en este ejemplo se usa el parámetro **/RU** para especificar que la tarea debe ejecutarse con los permisos de la cuenta que no es de administrador del usuario (*User01* en el dominio *reskit* ). Sin el parámetro **/RU** , la tarea se ejecutaría con los permisos de la cuenta especificada por **/u**.

    Al ejecutar este ejemplo, SchTasks solicita primero la contraseña del usuario nombrado por el parámetro **/u** (para ejecutar el comando) y, a continuación, solicita la contraseña del usuario nombrado por el parámetro **/RU** (para ejecutar la tarea). Después de autenticar las contraseñas, SchTasks muestra un mensaje que indica que la tarea está programada:

    ```
    Type the password for Marketing\Admin01:********
    Please enter the run as password for Reskits\User01: ********
    SUCCESS: The scheduled task My App has successfully been created.
    ```

- Para ejecutar la programación del programa de *AdminCheck.exe* para que se ejecute en el equipo *público* todos los viernes a las 4:00 A.M., pero solo si el administrador del equipo ha iniciado sesión, escriba:

    ```
    schtasks /create /tn Check Admin /tr AdminCheck.exe /sc weekly /d FRI /st 04:00 /s Public /u Domain3\Admin06 /ru Public\Admin01 /it
    ```

    En este ejemplo se usa el parámetro **/SC** para especificar una programación semanal, el parámetro **/d** para especificar el día y el parámetro **/St** para especificar la hora de inicio. También usa el parámetro **/s** para proporcionar el nombre del equipo remoto, el parámetro **/u** para especificar una cuenta con permiso para programar una tarea en el equipo remoto, el parámetro **/RU** para configurar la tarea para que se ejecute con los permisos del administrador del equipo público (*Public\Admin01*), y el parámetro **/it** para indicar que la tarea se ejecuta solo cuando la cuenta *Public\Admin01* ha iniciado sesión.

    > [!NOTE]
    > Para identificar las tareas con la propiedad solo Interactive (**/it**), use una consulta detallada ( `/query /v` ). En una presentación de consulta detallada de una tarea con **/it**, el campo **modo de inicio de sesión** tiene un valor de **solo interactivo**.

## <a name="to-schedule-a-task-that-runs-with-system-permissions"></a>Para programar una tarea que se ejecuta con permisos del sistema

Las tareas de todos los tipos se pueden ejecutar con los permisos de la cuenta **del sistema** en el equipo local y en el equipo remoto. Además de los parámetros necesarios para el tipo de programación determinado, se requiere el parámetro **/RU System** (o **/RU**), mientras que el parámetro **/RP** no es válido.

> [!IMPORTANT]
> La cuenta **del sistema** no tiene derechos de inicio de sesión interactivo. Los usuarios no pueden ver ni interactuar con programas o tareas que se ejecuten con permisos del sistema. El parámetro **/RU** determina los permisos en los que se ejecuta la tarea, no los permisos que se usan para programar la tarea. Solo los administradores pueden programar tareas, independientemente del valor del parámetro **/RU** .
>
> Para identificar las tareas que se ejecutan con permisos del sistema, utilice una consulta detallada ( `/query /v` ). En una visualización de consulta detallada de una tarea de ejecución del sistema, el campo de **usuario ejecutar como** tiene un valor de **NT AUTHORITY\SYSTEM** y el campo **modo de inicio de sesión** solo tiene un valor de **background**.

### <a name="examples"></a>Ejemplos

- Para programar que el programa MyApp se ejecute en el equipo local con los permisos de la cuenta **del sistema** , escriba:

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /d 15 /ru System
    ```

    En este ejemplo, la tarea está programada para ejecutarse el decimoquinto día de cada mes, pero puede usar cualquier tipo de programación para la ejecución de una tarea con permisos del sistema. Además, en este ejemplo se usa el parámetro **del sistema/RU** para especificar el contexto de seguridad del sistema. Dado que las tareas del sistema no usan una contraseña, se deja el parámetro **/RP** .

    En respuesta, SchTasks.exe muestra un mensaje informativo y un mensaje de operación correcta, sin pedir una contraseña:

    ```
    INFO: The task will be created under user name (NT AUTHORITY\SYSTEM).
    SUCCESS: The Scheduled task My App has successfully been created.
    ```

- Para programar el programa MyApp para que se ejecute en el equipo *Finance01* cada mañana a las 4:00 A.M., con los permisos del sistema, escriba:

    ```
    schtasks /create /tn My App /tr myapp.exe /sc daily /st 04:00 /s Finance01 /u Admin01 /ru System
    ```

    En este ejemplo se usa el parámetro **/TN** para asignar un nombre a la tarea y el parámetro **/TR** para especificar la copia remota del programa MyApp, el parámetro **/SC** para especificar una programación diaria, pero se deja el parámetro **/mo** porque *1* (todos los días) es el valor predeterminado. En este ejemplo también se usa el parámetro **/St** para especificar la hora de inicio, que también es el momento en que la tarea se ejecutará cada día, el parámetro **/s** para proporcionar el nombre del equipo remoto, el parámetro **/u** para especificar una cuenta con permiso para programar una tarea en el equipo remoto y el parámetro **/RU** para especificar que la tarea debe ejecutarse en la cuenta del sistema. Sin el parámetro **/RU** , la tarea se ejecutaría con los permisos de la cuenta especificada por el parámetro **/u** .

    Schtasks.exe solicita la contraseña del usuario nombrado por el parámetro **/u** y, después de autenticar la contraseña, muestra un mensaje que indica que la tarea se ha creado y que se ejecutará con los permisos de la cuenta **del sistema** :

    ```
    Type the password for Admin01:**********

    INFO: The Schedule Task My App will be created under user name (NT AUTHORITY\
    SYSTEM).
    SUCCESS: The scheduled task My App has successfully been created.
    ```

## <a name="to-schedule-a-task-that-runs-more-than-one-program"></a>Para programar una tarea que ejecuta más de un programa

Cada tarea ejecuta un solo programa. Sin embargo, puede crear un archivo por lotes que ejecute varios programas y, a continuación, programar una tarea para que ejecute el archivo por lotes.

1. Con un editor de texto, como el Bloc de notas, cree un archivo por lotes que incluya el nombre y la ruta de acceso completa al archivo. exe necesario para iniciar los programas Visor de eventos (Eventvwr.exe) y monitor de sistema (Perfmon.exe).

    ```
    C:\Windows\System32\Eventvwr.exe
    C:\Windows\System32\Perfmon.exe
    ```

2. Guarde el archivo como *MyApps.bat*, abra schtasks.exe y, a continuación, cree una tarea para ejecutar *MyApps.bat* escribiendo:

    ```
    schtasks /create /tn Monitor /tr C:\MyApps.bat /sc onlogon /ru Reskit\Administrator
    ```

    Este comando crea la tarea de supervisión, que se ejecuta cada vez que alguien inicia sesión. Usa el parámetro **/TN** para asignar un nombre a la tarea, el parámetro **/tr** para ejecutar MyApps.bat, el parámetro **/SC** para indicar el tipo de programación ONLOGON y el parámetro **/RU** para ejecutar la tarea con los permisos de la cuenta de administrador del usuario.

    Como resultado de este comando, cada vez que un usuario inicia sesión en el equipo, la tarea se inicia tanto Visor de eventos como el monitor de sistema.

## <a name="to-schedule-a-task-that-runs-on-a-remote-computer"></a>Para programar una tarea que se ejecuta en un equipo remoto

Para programar la ejecución de una tarea en un equipo remoto, debe agregar la tarea a la programación del equipo remoto. Las tareas de todos los tipos se pueden programar en un equipo remoto, pero se deben cumplir las condiciones siguientes:

- Debe tener permiso para programar la tarea. Como tal, debe haber iniciado sesión en el equipo local con una cuenta que sea miembro del grupo administradores en el equipo remoto, o bien debe usar el parámetro **/u** para proporcionar las credenciales de un administrador del equipo remoto.

- Puede usar el parámetro **/u** solo cuando los equipos locales y remotos están en el mismo dominio o el equipo local se encuentra en un dominio en el que confía el dominio del equipo remoto. De lo contrario, el equipo remoto no puede autenticar la cuenta de usuario especificada y no puede comprobar que la cuenta es miembro del grupo administradores.

- La tarea debe tener permisos suficientes para ejecutarse en el equipo remoto. Los permisos necesarios varían con la tarea. De forma predeterminada, la tarea se ejecuta con el permiso del usuario actual del equipo local o, si se usa el parámetro **/u** , la tarea se ejecuta con el permiso de la cuenta especificada por el parámetro **/u** . Sin embargo, puede usar el parámetro **/RU** para ejecutar la tarea con permisos de una cuenta de usuario diferente o con permisos del sistema.

### <a name="examples"></a>Ejemplos

- Para programar el programa MyApp (como administrador) para que se ejecute en el equipo remoto *SRV01* cada diez días a partir de inmediatamente, escriba:

    ```
    schtasks /create /s SRV01 /tn My App /tr c:\program files\corpapps\myapp.exe /sc daily /mo 10
    ```

    En este ejemplo se usa el parámetro **/s** para proporcionar el nombre del equipo remoto. Dado que el usuario actual local es un administrador del equipo remoto, el parámetro **/u** , que proporciona permisos alternativos para programar la tarea, no es necesario.

    > [!NOTE]
    > Al programar tareas en un equipo remoto, todos los parámetros hacen referencia al equipo remoto. Por lo tanto, el archivo especificado por el parámetro **/TR** hace referencia a la copia de MyApp.exe en el equipo remoto.

- Para programar el programa MyApp (como usuario) para que se ejecute en el equipo remoto *SRV06* cada tres horas, escriba:

    ```
    schtasks /create /s SRV06 /tn My App /tr c:\program files\corpapps\myapp.exe /sc hourly /mo 3 /u reskits\admin01 /p R43253@4$ /ru SRV06\user03 /rp MyFav!!Pswd
    ```

    Dado que se requieren permisos de administrador para programar una tarea, el comando usa los parámetros **/u** y **/p** para proporcionar las credenciales de la cuenta de administrador del usuario (*Admin01* en el dominio *reskit* ). De forma predeterminada, estos permisos también se usan para ejecutar la tarea. Sin embargo, dado que la tarea no necesita permisos de administrador para ejecutarse, el comando incluye los parámetros **/u** y **/RP** para invalidar el valor predeterminado y ejecutar la tarea con el permiso de la cuenta de usuario que no es administrador en el equipo remoto.

- Programar el programa MyApp (como usuario) para que se ejecute en el equipo remoto *SRV02* el último día de cada mes.

    ```
    schtasks /create /s SRV02 /tn My App /tr c:\program files\corpapps\myapp.exe /sc monthly /mo LASTDAY /m * /u reskits\admin01
    ```

    Dado que el usuario actual local (*user03*) no es un administrador del equipo remoto, el comando usa el parámetro **/u** para proporcionar las credenciales de la cuenta de administrador del usuario (*Admin01* en el dominio *reskit* ). Los permisos de la cuenta de administrador se usarán para programar la tarea y para ejecutar la tarea.

    Dado que el comando no incluía el parámetro **/p** (contraseña), SchTasks solicita la contraseña. A continuación, muestra un mensaje de confirmación y, en este caso, una advertencia:

    ```
    Type the password for reskits\admin01:********

    SUCCESS: The scheduled task My App has successfully been created.
    WARNING: The scheduled task My App has been created, but may not run because the account information could not be set.
    ```

    Esta advertencia indica que el dominio remoto no pudo autenticar la cuenta especificada por el parámetro **/u** . En este caso, el dominio remoto no pudo autenticar la cuenta de usuario porque el equipo local no es miembro de un dominio en el que confía el dominio del equipo remoto. Cuando esto ocurre, el trabajo de tarea aparece en la lista de tareas programadas, pero la tarea está realmente vacía y no se ejecuta.

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

- Para ejecutar el comando **/Create** con los permisos de otro usuario, use el parámetro **/u** . El parámetro **/u** solo es válido para programar tareas en equipos remotos.

- Para ver más `schtasks /create` ejemplos, escriba `schtasks /create /?` en un símbolo del sistema.

- Para programar una tarea que se ejecuta con permisos de otro usuario, use el parámetro **/RU** . El parámetro **/RU** es válido para las tareas en equipos locales y remotos.

- Para usar el parámetro **/u** , el equipo local debe estar en el mismo dominio que el equipo remoto o debe encontrarse en un dominio en el que confíe el dominio del equipo remoto. De lo contrario, la tarea no se crea o el trabajo de tarea está vacío y la tarea no se ejecuta.

- Schtasks siempre solicita una contraseña a menos que proporcione una, incluso cuando programa una tarea en el equipo local con la cuenta de usuario actual. Este es el comportamiento normal de SchTasks.

- Schtasks no comprueba las ubicaciones de los archivos de programa o las contraseñas de cuentas de usuario. Si no especifica la ubicación correcta del archivo o la contraseña correcta para la cuenta de usuario, se creará la tarea, pero no se ejecutará. Además, si la contraseña de una cuenta cambia o expira, y no cambia la contraseña guardada en la tarea, la tarea no se ejecutará.

- La cuenta **del sistema** no tiene derechos de inicio de sesión interactivo. Los usuarios no ven ni pueden interactuar con los programas que se ejecutan con permisos del sistema.

- Cada tarea ejecuta un solo programa. Sin embargo, puede crear un archivo por lotes que inicie varias tareas y, a continuación, programar una tarea que ejecute el archivo por lotes.

- Puede probar una tarea en cuanto la cree. Use la operación ejecutar para probar la tarea y, a continuación, compruebe si hay errores en el archivo de SchedLgU.txt (SystemRoot\SchedLgU.txt).

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando de cambio de SchTasks](schtasks-change.md)

- [comando de eliminación de SchTasks](schtasks-delete.md)

- [comando de fin de SchTasks](schtasks-end.md)

- [comando de consulta SchTasks](schtasks-query.md)

- [comando ejecutar SchTasks](schtasks-run.md)