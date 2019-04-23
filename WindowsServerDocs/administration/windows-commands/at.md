---
title: at
description: 'Tema de los comandos de Windows para **en** : programa comandos y programas para ejecutarse en un equipo en una fecha y hora especificadas.'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ff18fd16-9437-4c53-8794-bfc67f5256b3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 26ffa962541666e0e19b1c408bd6ab0b904296dd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832526"
---
# <a name="at"></a>at

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Programa comandos y programas para ejecutarse en un equipo en una fecha y hora especificadas. Puede usar **en** sólo cuando se ejecuta el servicio de programación. Se utiliza sin parámetros, **en** enumera los comandos programados.
## <a name="syntax"></a>Sintaxis
```
at [\\computername] [[id] [/delete] | /delete [/yes]]
at [\\computername] <time> [/interactive] [/every:date[,...] | /next:date[,...]] <command>
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|----------------------|--------|
|\\\\\<computerName\>|Especifica un equipo remoto. Si se omite este parámetro, **en** programa los comandos y programas en el equipo local.|
|\<id\>|Especifica el número de identificación asignado a un comando programado.|
|/delete|Cancela un comando programado. Si se omite *ID*, se cancelan todos los comandos programados en el equipo.|
|/ yes|Respuestas sí a todas las consultas del sistema cuando se eliminan eventos programados.|
|\<time\>|Especifica el tiempo que desee ejecutar el comando. tiempo se expresa como horas: minutos en notación de 24 horas (es decir, 00:00 [medianoche] hasta las 23:59).|
|/ interactivo|Permite *comando* para interactuar con el escritorio del usuario que ha iniciado sesión en el momento *comando* se ejecuta.|
|/ cada:|Ejecuciones *comando* en cada día o días especificados de la semana o mes (por ejemplo, todos los jueves o el tercer día de cada mes).|
|\<date\>|Especifica la fecha cuando desee ejecutar el comando. Puede especificar uno o varios días de la semana (es decir, type **M**,**T**,**W**,**Th**,**F**,**S**,**Su**) o uno o más días del mes (es decir, type 1 a 31). Separe varias entradas de fecha con comas. Si se omite *fecha*, **en** utiliza el día del mes actual.|
|/next:|Ejecuciones *comando* en la siguiente repetición del día (por ejemplo, el siguiente jueves).|
|\<command\>|Especifica el comando de Windows, el programa (archivo .exe o .com) o programa por lotes (es decir, archivos .bat o. cmd) que desea ejecutar. Si el comando requiere una ruta de acceso como argumento, utilice la ruta de acceso absoluta (es decir, la ruta de acceso completa a partir la letra de unidad). Si el comando está en un equipo remoto, especifique la notación de convención de nomenclatura Universal (UNC) para el servidor y comparta nombre, en lugar de una letra de unidad remota.|
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="remarks"></a>Comentarios
-   **SCHTASKS** es otra herramienta de línea de comandos de programación que puede usar para crear y administrar las tareas programadas. Para obtener más información acerca de **schtasks**, vea Temas relacionados.
-   Uso de **en**  
    Para usar **en**, debe ser miembro del grupo Administradores local.
-   Cargando Cmd.exe  
    **en** no carga automáticamente Cmd.exe, el intérprete de comandos, antes de ejecutar comandos. Si no utiliza un archivo ejecutable (.exe), debe cargar explícitamente Cmd.exe al principio del comando como sigue: **cmd /c dir > c:\test.out**
-   Comandos de visualización programado  
    Cuando usas **en** sin opciones de línea de comandos, las tareas programadas aparecen en una tabla con un formato similares al siguiente:
    ```
    Status  ID   Day        time        Command Line
    OK      1    Each F     4:30 PM     net send group leads status due
    OK      2    Each M     12:00 AM    chkstor > check.file
    OK      3    Each F     11:59 PM    backup2.bat
    ```
-   Incluido el número de identificación (*ID*)  
    Al incluir el número de identificación (*ID*) con **en** en un símbolo del sistema, información de una sola entrada aparece en un formato similar al siguiente:  
    ```
    Task ID:      1
    Status:       OK
    Schedule:     Each  F
    time of Day:  4:30 PM
    Command:      net send group leads status due
    ```
    Después de programar un comando con **en**, especialmente un comando que tiene opciones de línea de comandos, compruebe que la sintaxis del comando es correcta, escriba **en** sin opciones de línea de comandos. Si la información de la columna de la línea de comandos no es correcta, elimine el comando y vuelva a escribirla. Si todavía no es correcta, vuelva a escribir el comando con menos opciones de línea de comandos.
-   Ver los resultados  
    Los comandos programan con **en** se ejecutan como procesos en segundo plano. No se muestra la salida en la pantalla del equipo. Para redirigir la salida a un archivo, utilice el símbolo de redirección (>). Si se redirige la salida a un archivo, deberá usar el símbolo de escape (^) antes del símbolo de redirección, si usas **en** en la línea de comandos o en un archivo por lotes. Por ejemplo, para redirigir la salida a resultado.txt, escriba:  
    `at 14:45 c:\test.bat ^>c:\output.txt`  
    El directorio actual para el comando en ejecución es la carpeta raíz.
-   Cambiar la hora del sistema  
    Si cambia la hora del sistema en un equipo después de programar un comando para ejecutar con **en**, sincronizar la **en** scheduler con la hora del sistema revisado escribiendo **en** sin Opciones de línea de comandos.
-   Almacenar comandos  
    Los comandos programados se almacenan en el registro. Como resultado, no pierda las tareas programadas si reinicia el servicio de programación.
-   Las unidades de conectarse a la red  
    No use una unidad redirigida para los trabajos programados que tienen acceso a la red. El servicio de programación no pueda tener acceso a la unidad redirigida o la unidad redirigida podría no estar presente si otro usuario ha iniciado sesión en el momento en que se ejecuta la tarea programada. En su lugar, use rutas de acceso UNC para los trabajos programados. Por ejemplo:  
    `at 1:00pm my_backup \\\server\share`  
    No use la sintaxis siguiente, donde **x:** es una conexión realizada por el usuario:  
    `at 1:00pm my_backup x:`  
    Si programa una **en** comando que usa una letra de unidad para conectarse a un directorio compartido, incluir un **en** comando para desconectar la unidad cuando haya terminado de utilizar la unidad. Si no se desconecta la unidad, la letra de unidad asignada no está disponible en el símbolo del sistema.
-   Tareas de detención tras 72 horas  
    De forma predeterminada, las tareas programadas con el **en** comando stop tras 72 horas. Puede modificar el registro para cambiar este valor predeterminado.
    1.  Inicie el editor del registro (regedit.exe).
    2.  Busque y haga clic en la siguiente clave del registro: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Schedule**
    3.  En el menú Edición, haga clic en Agregar valor y, a continuación, agregue el siguiente valor del registro: Nombre del valor: tipo de datos atTaskMaxHours: reg_DWOrd base: Datos del valor decimal: 0. Un valor de 0 en el campo de datos de valor no indica ningún límite, no se detiene. Los valores de 1 a 99 indica el número de horas.
**Precaución**
-   La edición incorrecta del Registro puede dañar gravemente el sistema. Antes de realizar cambios en el Registro, debe hacer una copia de seguridad de los datos de valor guardados en el equipo.
-   Programador de tareas y el **en** comando  
    Puede usar la carpeta tareas programadas para ver o modificar la configuración de una tarea que se creó mediante la **en** comando. Cuando programe una tarea mediante la **en** comando, la tarea se muestra en la carpeta de tareas programadas, con un nombre como el siguiente:**at3478**. Sin embargo, si modifica una tarea a través de la carpeta de tareas programadas, se actualiza a una tarea programada normal. La tarea ya no es visible para el **en** comando y en la cuenta de configuración ya no se aplica a él. Debe escribir explícitamente una cuenta de usuario y contraseña para la tarea.
## <a name="examples"></a>Ejemplos
Para mostrar una lista de los comandos programados en el servidor de Marketing, escriba:

`at \\marketing`

Para obtener más información sobre un comando con el número de identificación 3 en el servidor de trabajo, escriba:

`at \\corp 3`

Para programar un comando net share para ejecutarse en el servidor de trabajo a las 8:00 A.M. y redirigir la lista para el servidor de mantenimiento, en el directorio compartido de informes y el archivo trabajo.txt, escriba:

`at \\corp 08:00 cmd /c "net share reports=d:\marketing\reports >> \\maintenance\reports\corp.txt"`

Para la copia de seguridad de la unidad de disco duro del servidor de Marketing a una unidad de cinta a medianoche cada cinco días, crear un programa por lotes llamado archivo.cmd, que contiene los comandos de copia de seguridad, y, a continuación, programar el programa por lotes para ejecutar, escriba:

`at \\marketing 00:00 /every:5,10,15,20,25,30 archive`

Para cancelar todos los comandos programados en el servidor actual, desactive la **en** información de programación como sigue:

`at /delete`

Para ejecutar un comando que no es un archivo ejecutable (.exe), ir precedido de **cmd /c** cargar Cmd.exe como sigue:

`cmd /c dir > c:\test.out`
