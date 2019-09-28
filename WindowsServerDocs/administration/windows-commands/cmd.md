---
title: Cmd
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 032fbea2039faa09753ac0c2b51e4b62004d36ac
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379331"
---
# <a name="cmd"></a>Cmd

Inicia una nueva instancia del intérprete de comandos, cmd. exe. Si se usa sin parámetros, **cmd** muestra la versión y la información de copyright del sistema operativo.

## <a name="syntax"></a>Sintaxis

```
cmd [/c|/k] [/s] [/q] [/d] [/a|/u] [/t:{<B><F>|<F>}] [/e:{on|off}] [/f:{on|off}] [/v:{on|off}] [<String>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/c|Lleva a cabo el comando especificado por *cadena* y, a continuación, se detiene.|
|/k|Lleva a cabo el comando especificado por *cadena* y continúa.|
|/s|Modifica el tratamiento de la *cadena* después de **/c** o **/k**.|
|/q|Desactiva el eco.|
|/d|Deshabilita la ejecución de comandos de ejecución automática.|
|/a|Da formato al resultado del comando interno en una canalización o un archivo como American National Standards Institute (ANSI).|
|/u|Da formato al resultado del comando interno en una canalización o un archivo como Unicode.|
|/t: {\<B @ no__t-1 @ no__t-2F @ no__t-3 @ no__t-4 @ no__t-5F @ no__t-6}|Establece los colores de fondo (*B*) y de primer plano (*F*).|
|/e: activado|Habilita las extensiones de comandos.|
|/e: desactivado|Deshabilita las extensiones de comandos.|
|/f: activado|Habilita la finalización de nombres de archivos y directorios.|
|/f: desactivado|Deshabilita la finalización de nombres de archivos y directorios.|
|/v: activado|Habilita la expansión diferida de variables de entorno.|
|/v: desactivado|Deshabilita la expansión diferida de variables de entorno.|
|@no__t 0String >|Especifica el comando que se desea llevar a cabo.|
|/?|Muestra la ayuda en el símbolo del sistema.|

En la tabla siguiente se enumeran los dígitos hexadecimales válidos que puede usar como valores de \<B @ no__t-1 y \<F @ no__t-3

|Valor|Color|
|-----|-----|
|0|Negro|
|1|Azul|
|2|Verde|
|3|Aguamarina|
|4|Roja|
|5|Ponen|
|6|Amarillo|
|7|Blanco|
|8|Gris|
|9|Azul claro|
|a|Verde claro|
|b|Aguamarina claro|
|c|Rojo claro|
|d|Púrpura claro|
|E:.|Amarillo claro|
|f|Blanco brillante|

## <a name="remarks"></a>Comentarios

-   Usar varios comandos

    Para usar varios comandos para el > \<String, sepárelos mediante el separador de comandos **&&** y colóquelos entre comillas. Por ejemplo:

    ```
    "<Command>&&<Command>&&<Command>"
    ``` 
 
-   Procesar comillas

    Si especifica **/c** o **/k**, **cmd** procesa el resto de la *cadena* y se conservan las comillas solo si se cumplen todas las condiciones siguientes:  
    -   No use **/s**.
    -   Se usa exactamente un conjunto de Comillas.
    -   No se usa ningún carácter especial entre comillas (por ejemplo: & < > () @ ^ |).
    -   Utilice uno o varios caracteres de espacio en blanco dentro de las comillas.
    -   La *cadena* entre comillas es el nombre de un archivo ejecutable.

    Si no se cumplen las condiciones anteriores, la *cadena* se procesa examinando el primer carácter para comprobar si se trata de una comilla de apertura. Si el primer carácter es una comilla de apertura, se quita junto con la comilla de cierre. Se conserva cualquier texto que siga las comillas de cierre.
-   Ejecutar subclaves del registro

    Si no especifica **/d** en *String*, cmd. exe busca las siguientes subclaves del registro:

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\AutoRun\REG_SZ**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\AutoRun\REG_EXPAND_SZ**

    Si una o ambas subclaves del registro están presentes, se ejecutan antes que todas las demás variables.

> [!CAUTION]
> La edición incorrecta del Registro puede dañar gravemente el sistema. Antes de realizar cambios en el Registro, debe hacer una copia de seguridad de los datos de valor guardados en el equipo.

-   Habilitar y deshabilitar las extensiones de comando

    Las extensiones de comando están habilitadas de forma predeterminada en Windows XP. Puede deshabilitarlos para un proceso determinado mediante **/e: OFF**. Puede habilitar o deshabilitar las extensiones para todas las opciones de línea de comandos de **cmd** en un equipo o una sesión de usuario mediante el establecimiento de los siguientes valores **REG_DWORD** :

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\EnableExtensions\REG_DWORD**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\EnableExtensions\REG_DWORD**

    Establezca el valor **REG_DWORD** en **0 × 1** (habilitado) o en **0 × 0** (deshabilitado) en el registro mediante regedit. exe. La configuración especificada por el usuario tiene prioridad sobre la configuración del equipo y las opciones de línea de comandos tienen prioridad sobre la configuración del registro.

> [!CAUTION]
> La edición incorrecta del Registro puede dañar gravemente el sistema. Antes de realizar cambios en el Registro, debe hacer una copia de seguridad de los datos de valor guardados en el equipo.

    When you enable command extensions, the following commands are affected:  
    -  **assoc**
    -  **call**
    -  **chdir (cd)**
    -  **color**
    -  **del (erase)**
    -  **endlocal**
    -  **for**
    -  **ftype**
    -  **goto**
    -  **if**
    -  **mkdir (md)**
    -  **popd**
    -  **prompt**
    -  **pushd**
    -  **set**
    -  **setlocal**
    -  **shift**
    -  **start** (also includes changes to external command processes)

-   Habilitar la expansión diferida de variables de entorno

    Si habilita la expansión diferida de variables de entorno, puede usar el carácter de signo de exclamación para sustituir el valor de una variable de entorno en tiempo de ejecución.
-   Habilitar la finalización de nombres de archivos y directorios

    La finalización de los nombres de archivos y directorios no está habilitada de forma predeterminada. Puede habilitar o deshabilitar la finalización de un nombre de archivo para un proceso determinado del comando **cmd** con **/f:** {**on**|**OFF**}. Puede habilitar o deshabilitar la finalización de nombres de archivos y directorios para todos los procesos del comando **cmd** en un equipo o para una sesión de inicio de sesión de usuario mediante el establecimiento de los siguientes valores **REG_DWORD** :

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\CompletionChar\REG_DWORD**

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\PathCompletionChar\REG_DWORD**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\CompletionChar\REG_DWORD**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\PathCompletionChar\REG_DWORD**

    Para establecer el valor **REG_DWORD** , ejecute regedit. exe y use el valor hexadecimal de un carácter de control para una función determinada (por ejemplo, **0 × 9** es Tab y **0 × 08** es el retroceso). La configuración especificada por el usuario tiene prioridad sobre la configuración del equipo y las opciones de línea de comandos tienen prioridad sobre la configuración del registro.

> [!CAUTION]
> La edición incorrecta del Registro puede dañar gravemente el sistema. Antes de realizar cambios en el Registro, debe hacer una copia de seguridad de los datos de valor guardados en el equipo.

Si habilita la finalización de nombres de archivos y directorios mediante **/f: on**, use Ctrl + D para completar el nombre de directorio y Ctrl + f para completar el nombre de archivo. Para deshabilitar un carácter de finalización determinado en el registro, use el valor de espacio en blanco [**0 × 20**] porque no es un carácter de control válido.

Al presionar CTRL + D o CTRL + F, **cmd** procesa la finalización de los nombres de archivos y directorios. Estas funciones de combinación de teclas anexan un carácter comodín a una *cadena* (si no hay ninguna), generan una lista de rutas de acceso que coinciden y, a continuación, muestran la primera ruta de acceso coincidente. Si ninguna de las rutas de acceso coincide, la función de finalización de nombres de archivo y directorio emite un pitido y no cambia la pantalla. Para desplazarse por la lista de rutas de acceso coincidentes, presione CTRL + D o CTRL + F varias veces. Para desplazarse por la lista hacia atrás, presione la tecla Mayús y CTRL + D o CTRL + F simultáneamente. Para descartar la lista guardada de rutas de acceso coincidentes y generar una nueva lista, edite la *cadena* y presione Ctrl + D o Ctrl + F. Si cambia entre CTRL + D y CTRL + F, se descarta la lista guardada de rutas de acceso coincidentes y se genera una nueva lista. La única diferencia entre las combinaciones de teclas CTRL + D y CTRL + F es que CTRL + D solo coincide con los nombres de directorio y CTRL + F coincide con los nombres de archivo y de directorio. Si usa la finalización de nombres de archivo y directorio en cualquiera de los comandos de directorio integrados (es decir, **CD**, **MD**o **Rd**), se supone que se ha completado el directorio.

La finalización de nombres de archivos y directorios procesa correctamente los nombres de archivo que contienen espacios en blanco o caracteres especiales si se colocan comillas alrededor de la ruta de acceso coincidente.

Los siguientes caracteres especiales requieren comillas: & < > [] {} ^ =;! ' +, ' ~ [espacio en blanco].

Si la información proporcionada contiene espacios, utilice comillas alrededor del texto (por ejemplo, "nombre del equipo").

Si procesa la finalización de nombres de archivos y directorios desde una *cadena*, se descarta cualquier parte de la *ruta de acceso* situada a la derecha del cursor (en el punto de la *cadena* donde se procesó la finalización).

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
