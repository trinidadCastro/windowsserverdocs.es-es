---
title: at
description: 'Comandos de Windows: en, que programa comandos y **programas para que**se ejecuten en un equipo a una hora y fecha especificadas.'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ff18fd16-9437-4c53-8794-bfc67f5256b3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc71af64bdcb1b71afef1e88d42647c0e52612af
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851288"
---
# <a name="at"></a>at

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Programa comandos y programas para que se ejecuten en un equipo a una hora y fecha especificadas. Solo se puede usar **en** cuando se está ejecutando el servicio de programación. Se usa sin parámetros, **en** enumera los comandos programados.

## <a name="syntax"></a>Sintaxis

```
at [\computername] [[id] [/delete] | /delete [/yes]]
at [\computername] <time> [/interactive] [/every:date[,...] | /next:date[,...]] <command>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `\<computerName\>` | Especifica un equipo remoto. Si omite este parámetro, **en** programa los comandos y programas en el equipo local. |
| `<id>` | Especifica el número de identificación asignado a un comando programado. |
| `/delete` | Cancela un comando programado. Si omite *ID*, se cancelarán todos los comandos programados en el equipo. |
|  `/yes` | Responde afirmativamente a todas las consultas del sistema cuando se eliminan los eventos programados. |
| `<time>` | Especifica la hora a la que se desea ejecutar el comando. la hora se expresa en horas: minutos en notación de 24 horas (es decir, 00:00 [medianoche] a 23:59). |
| `interactive` | Permite que el *comando* interactúe con el escritorio del usuario que ha iniciado sesión en el momento en que se ejecuta el *comando* . |
| `every:` | Ejecuta el *comando* en cada día o días de la semana o mes especificados (por ejemplo, cada jueves o el tercer día de cada mes). |
|` <date>` | Especifica la fecha en la que desea ejecutar el comando. Puede especificar uno o varios días de la semana (es decir, el tipo **M**,**T**,**W**,**TH**,**F**,**S**,**su**) o uno o varios días del mes (es decir, el tipo 1 a 31). Separe varias entradas de fecha con comas. Si se omite la *fecha*, **en** utiliza el día actual del mes. |
| `next:` | Ejecuta el *comando* en la siguiente repetición del día (por ejemplo, el jueves siguiente). |
| `<command>`      | Especifica el comando de Windows, el programa (es decir, el archivo. exe o. com) o el programa de Batch (es decir, el archivo. bat o. cmd) que desea ejecutar. Cuando el comando requiere una ruta de acceso como argumento, utilice la ruta de acceso absoluta (es decir, toda la ruta de acceso que comienza con la letra de unidad). Si el comando se encuentra en un equipo remoto, especifique la notación de Convención de nomenclatura universal (UNC) para el servidor y el nombre del recurso compartido, en lugar de una letra de unidad remota. |
| `/?` | Muestra la Ayuda en el símbolo del sistema. |

## <a name="remarks"></a>Comentarios

- **SchTasks** es otra herramienta de programación de línea de comandos que puede usar para crear y administrar tareas programadas. Para obtener más información acerca de **SchTasks**, consulte [Schtasks](schtasks.md).

- Para usar **en**, debe ser miembro del grupo local Administradores.

- **at** no carga automáticamente cmd. exe, el intérprete de comandos, antes de ejecutar los comandos. Si no está ejecutando un archivo ejecutable (. exe), debe cargar explícitamente cmd. exe al principio del comando de la siguiente manera:

    ```
    cmd /c dir > c:\test.out
    ```

- Cuando se usa **at** sin opciones de línea de comandos, las tareas programadas aparecen en una tabla con un formato similar al siguiente:

    ```
    Status  ID   Day        time        Command Line
    OK      1    Each F     4:30 PM     net send group leads status due
    OK      2    Each M     12:00 AM    chkstor > check.file
    OK      3    Each F     11:59 PM    backup2.bat
    ```

- Cuando se incluye el número de identificación (*ID.* ) con **at** en un símbolo del sistema, la información de una sola entrada aparece en un formato similar al siguiente:  

    ```
    Task ID: 1
    Status: OK
    Schedule: Each  F
    Time of Day: 4:30 PM
    Command: net send group leads status due
  ```

    Después de programar un comando con **en**, especialmente un comando que tiene opciones de línea de comandos, compruebe que la sintaxis del comando es correcta escribiendo **en** sin opciones de línea de comandos. Si la información de la columna de línea de comandos es incorrecta, elimine el comando y vuelva a escribirlo. Si todavía es incorrecto, vuelva a escribir el comando con menos opciones de línea de comandos.

- Comandos programados con **en** ejecución como procesos en segundo plano. La salida no se muestra en la pantalla del equipo. Para redirigir la salida a un archivo, use el símbolo de redirección `(>)`. Si redirige los resultados a un archivo, debe utilizar el símbolo de escape `(^)` antes del símbolo de redireccionamiento, tanto si está utilizando **at** en la línea de comandos o en un archivo por lotes. Por ejemplo, para redirigir la salida a output. Text, escriba:

    `at 14:45 c:\test.bat ^>c:\output.txt`

    El directorio actual del comando que se está ejecutando es la carpeta SystemRoot.

- Si cambia la hora del sistema en un equipo después de programar un comando para que se ejecute con **at**, sincronice el **en** Scheduler con la hora del sistema revisada escribiendo **en** sin opciones de línea de comandos.

- Los comandos programados se almacenan en el registro. Como resultado, no se pierden las tareas programadas si se reinicia el servicio de programación.

- No use una unidad redirigida para los trabajos programados que tienen acceso a la red. Es posible que el servicio de programación no pueda obtener acceso a la unidad redirigida o que la unidad redirigida no esté presente si un usuario diferente ha iniciado sesión en el momento en que se ejecuta la tarea programada. En su lugar, use rutas de acceso UNC para los trabajos programados. Por ejemplo:  

    `at 1:00pm my_backup \\\server\share`  

    No use la sintaxis siguiente, donde **x:** es una conexión realizada por el usuario:  

    `at 1:00pm my_backup x:`  

    Si programa un comando **at** que usa una letra de unidad para conectarse a un directorio compartido, incluya un comando **at** para desconectar la unidad cuando haya terminado de usar la unidad. Si la unidad no está desconectada, la letra de unidad asignada no está disponible en el símbolo del sistema.

- De forma predeterminada, las tareas programadas con el comando **at** se detienen después de 72 horas. Puede modificar el registro para cambiar este valor predeterminado.

> [!Caution]
> Una modificación incorrecta del Registro puede provocar daños graves en el sistema. Antes de realizar cambios en el Registro, debe hacer una copia de seguridad de los datos de valor guardados en el equipo.

    1. Inicie el editor del registro (regedit. exe).

    2. Busque y haga clic en la clave siguiente en el registro: **HKEY_LOCAL_MACHINE \system\currentcontrolset\services\schedule**

    3. En el menú **edición** , haga clic en **Agregar valor**y, a continuación, agregue los siguientes valores del registro:

        - **Nombre del valor.** atTaskMaxHours

        - **Tipo de datos.** reg_DWOrd 

        - **Fijo.** Decimal

        - **Información del valor:** 0,1. Un valor de 0 en el campo de datos de valor indica que no hay límite y no se detiene. Los valores del 1 al 99 indican el número de horas.

- Puede usar la carpeta tareas programadas para ver o modificar la configuración de una tarea creada con el comando **at** . Cuando se programa una tarea mediante el comando **at** , la tarea se muestra en la carpeta tareas programadas, con un nombre como el siguiente:**at3478**. Sin embargo, si modifica una tarea at a través de la carpeta tareas programadas, se actualiza a una tarea programada normal. La tarea ya no está visible para el comando **at** y la configuración de la cuenta at ya no se aplica a ella. Debe especificar explícitamente una cuenta de usuario y una contraseña para la tarea.

## <a name="examples"></a>Ejemplos

Para mostrar una lista de los comandos programados en el servidor de marketing, escriba:

`at \\marketing`

Para obtener más información acerca de un comando con el número de identificación 3 en el servidor Corp, escriba:

`at \\corp 3`

Para programar un comando net Share para que se ejecute en el servidor Corp a las 8:00 A.M. y redirija la lista al servidor de mantenimiento, en el directorio compartido de informes y el archivo Corp. txt, escriba:

`at \\corp 08:00 cmd /c net share reports=d:\marketing\reports >> \\maintenance\reports\corp.txt`

Para realizar una copia de seguridad del disco duro del servidor de marketing en una unidad de cinta a medianoche cada cinco días, cree un programa de Batch llamado Archive. cmd, que contenga los comandos de copia de seguridad y, a continuación, programe el programa por lotes que se ejecutará, escriba:

`at \\marketing 00:00 /every:5,10,15,20,25,30 archive`

Para cancelar todos los comandos programados en el servidor actual, borre la información de **en** la programación de la siguiente manera:

`at /delete`

Para ejecutar un comando que no es un archivo ejecutable (es decir,. exe), preceda al comando con **cmd/c** para cargar cmd. exe de la siguiente manera:

`cmd /c dir > c:\test.out`

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)