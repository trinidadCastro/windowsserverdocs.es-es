---
title: Cmd
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6ec588db-31a9-4a73-a970-65a2c6f4abbe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 966ac7f70984dff6d26265e07a26a6eebcde9fb6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874396"
---
# <a name="cmd"></a>Cmd



Inicia una nueva instancia de intérprete de comandos Cmd.exe. Si se utiliza sin parámetros, **cmd** muestra la información de copyright y de versión del sistema operativo.

## <a name="syntax"></a>Sintaxis

```
cmd [/c|/k] [/s] [/q] [/d] [/a|/u] [/t:{<B><F>|<F>}] [/e:{on|off}] [/f:{on|off}] [/v:{on|off}] [<String>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/c|Lleva a cabo el comando especificado por *cadena* y, a continuación, se detiene.|
|/k|Lleva a cabo el comando especificado por *cadena* y continúa.|
|/s|Modifica el tratamiento de *cadena* después **/c** o **/k**.|
|/q|Desactiva el eco.|
|/d|Deshabilita la ejecución de comandos de ejecución automática.|
|/a|Formatos de salida del comando interno en una canalización o un archivo como American National Standards Institute (ANSI).|
|/u|Formatos de salida del comando interno en una canalización o un archivo como Unicode.|
|/t:{\<B\>\<F\>\|\<F\>}|Establece el fondo (*B*) y de primer plano (*F*) colores.|
|/e:on|Permite a las extensiones de comando.|
|/e:off|Deshabilita las extensiones de comandos.|
|/f:on|Permite la terminación de nombre de archivo y directorio.|
|/f:off|Deshabilita la terminación de nombre de archivo y directorio.|
|/v:on|Permite retrasa la expansión de variables de entorno.|
|/v:off|Deshabilita retrasa la expansión de variables de entorno.|
|\<String>|Especifica el comando que desee llevar a cabo.|
|/?|Muestra la ayuda en el símbolo del sistema.|

En la tabla siguiente se enumera los dígitos hexadecimales válidos que puede usar como valores para \<B\> y \<F\>

|Valor|Color|
|-----|-----|
|0|Negro|
|1|Azul|
|2|Verde|
|3|Aqua|
|4|Roja|
|5|Púrpura|
|6|Amarillo|
|7|Blanco|
|8|Gris|
|9|Azul claro|
|a|Verde claro|
|b|Aguamarina claro|
|c|Rojo claro|
|d|Púrpura claro|
|e|Amarillo claro|
|f|Blanco brillante|

## <a name="remarks"></a>Comentarios

-   Usar varios comandos

    Para usar varios comandos para \<cadena >, deberá separarlos por el separador de comandos **&&** y enciérrelas entre comillas. Por ejemplo:  
    ```
    "<Command>&&<Command>&&<Command>"
    ```  
-   Las comillas de procesamiento

    Si especifica **/c** o **/k**, **cmd** procesa el resto de *cadena,* y se conservan las comillas solo si todos los de las siguientes acciones se cumplen las condiciones:  
    -   No use **/s**.
    -   Usar un único conjunto de comillas.
    -   No utilice caracteres especiales dentro de las comillas (por ejemplo: & () < > @ ^ |).
    -   Utilice uno o varios caracteres de espacio en blanco entre las comillas.
    -   El *cadena* entre comillas es el nombre de un archivo ejecutable.

    Si no se cumplen las condiciones anteriores, *cadena* se procesa mediante el examen del primer carácter para comprobar si es una comilla de apertura. Si el primer carácter es una comilla de apertura, se elimina junto con la comilla de cierre. Se conserva cualquier texto que sigue a las comillas de cierre.
-   Ejecución de subclaves del registro

    Si no especifica **/d** en *cadena*, Cmd.exe busca las subclaves del registro siguientes:

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\AutoRun\REG_SZ**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\AutoRun\REG_EXPAND_SZ**

    Si una o ambas subclaves del registro están presentes, se ejecutan antes que todas las demás variables.

> [!CAUTION]
> La edición incorrecta del Registro puede dañar gravemente el sistema. Antes de realizar cambios en el Registro, debe hacer una copia de seguridad de los datos de valor guardados en el equipo.
-   Habilitar y deshabilitar las extensiones de comando

    Las extensiones de comando están habilitadas de forma predeterminada en Windows XP. Se puede deshabilitar para un proceso determinado mediante **/e: desactivar**. Puede habilitar o deshabilitar las extensiones para todos los **cmd** opciones de línea de comandos en una sesión de usuario o equipo estableciendo lo siguiente **REG_DWORD** valores:

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\EnableExtensions\REG_DWORD**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\EnableExtensions\REG_DWORD**

    Establecer el **REG_DWORD** valor como **0 × 1** (habilitado) o **0 × 0** (deshabilitado) en el registro mediante Regedit.exe. Configuración especificada por el usuario tiene prioridad sobre la configuración del equipo y las opciones de línea de comandos tienen prioridad sobre la configuración del registro.

>     [!CAUTION]
>     Incorrectly editing the registry may severely damage your system. Before making changes to the registry, you should back up any valued data on the computer.

    When you enable command extensions, the following commands are affected:  
    -   **assoc**
    -   **call**
    -   **chdir (cd)**
    -   **color**
    -   **del (erase)**
    -   **endlocal**
    -   **for**
    -   **ftype**
    -   **goto**
    -   **if**
    -   **mkdir (md)**
    -   **popd**
    -   **prompt**
    -   **pushd**
    -   **set**
    -   **setlocal**
    -   **shift**
    -   **start** (also includes changes to external command processes)
-   Habilitar expansión de variables de entorno retrasada

    Si habilita la expansión de variables de entorno retrasada, puede usar el carácter de signo de exclamación para sustituir el valor de una variable de entorno en tiempo de ejecución.
-   Habilitar la terminación de nombre de archivo y directorio

    Terminación de nombre de archivo y directorio no está habilitada de forma predeterminada. Puede habilitar o deshabilitar la terminación de nombre de archivo para un determinado proceso de la **cmd** comando **/f:**{**en**|**desactivar**}. Puede habilitar o deshabilitar la terminación de nombre de archivo y directorio para todos los procesos de la **cmd** comando en un equipo o para una sesión de inicio de sesión de usuario estableciendo lo siguiente **REG_DWORD** valores:

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\CompletionChar\REG_DWORD**

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\PathCompletionChar\REG_DWORD**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\CompletionChar\REG_DWORD**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\PathCompletionChar\REG_DWORD**

    Para establecer el **REG_DWORD** valor, ejecute Regedit.exe y utilice el valor hexadecimal de un carácter de control para una función determinada (por ejemplo, **0 × 9** pestaña y **0 × 08** es RETROCESO). Configuración especificada por el usuario tiene prioridad sobre la configuración del equipo y las opciones de línea de comandos tienen prioridad sobre la configuración del registro.

> [!CAUTION]
> La edición incorrecta del Registro puede dañar gravemente el sistema. Antes de realizar cambios en el Registro, debe hacer una copia de seguridad de los datos de valor guardados en el equipo.

Si habilita la terminación de nombre de archivo y directorio mediante el uso de **/f: en**, utilice CTRL+D para Autocompletar nombres de directorio y CTRL + F para Autocompletar nombres de archivo. Para deshabilitar un carácter de terminación determinado en el registro, use el valor de espacio en blanco [**0 × 20**] porque no es un carácter de control válido.

Al presionar CTRL + D o CTRL + F, **cmd** procesa la terminación de nombre de archivo y directorio. Estas funciones de la combinación de teclas anexan un carácter comodín para *cadena* (si uno no está presente), crear una lista de rutas de acceso que coincidan y, a continuación, muestre la primera ruta coincidente. Si ninguna de las rutas de acceso coincide, la función de finalización de nombre de archivo y directorio emite un sonido y no cambia la presentación. Para desplazarse por la lista de rutas coincidentes, presione CTRL+D o CTRL+F repetidamente. Para mover hacia atrás a través de la lista, presione la tecla MAYÚS y CTRL + D o CTRL + F simultáneamente. Para descartar la lista de rutas coincidentes guardada y generar una lista nueva, editar *cadena* y presione CTRL+D o CTRL + F. Si cambia entre CTRL+D y CTRL + F, la lista de guardados de rutas coincidentes se descarta y se genera una nueva lista. La única diferencia entre las combinaciones de teclas CTRL + D y CTRL + F es que CTRL+D sólo coincide con los nombres de directorio y CTRL + F coincide con los nombres de archivo y directorio. Si utiliza Autocompletar nombres de archivos y directorios en cualquiera de los comandos de directorio integrados (es decir, **CD**, **MD**, o **RD**), se asume la terminación del directorio.

Terminación de nombre de archivo y directorio procesa correctamente los nombres de archivo que contienen espacios en blanco o caracteres especiales si coloca las comillas alrededor de la ruta de acceso coincidente.

Los caracteres especiales siguientes requieren comillas: & < > [] {} ^ =;! ' + , ` ~ [white space].

Si la información que se proporciona contiene espacios, utilice comillas alrededor del texto (por ejemplo, "nombre del equipo").

Si se procesa la terminación de nombre de archivo y directorio desde *cadena*, cualquier parte de la *ruta de acceso* a la derecha del cursor se descarta (en el punto *cadena* donde el se procesó la terminación).

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
