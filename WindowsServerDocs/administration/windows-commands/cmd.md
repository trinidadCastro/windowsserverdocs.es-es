---
title: cmd
description: Artículo de referencia para el comando cmd, que inicia una nueva instancia del intérprete de comandos, Cmd.exe.
ms.topic: reference
ms.assetid: 6ec588db-31a9-4a73-a970-65a2c6f4abbe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b782a93d4c61f43bbe45497871fe66f29ef972a4
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030983"
---
# <a name="cmd"></a>cmd

Inicia una nueva instancia del intérprete de comandos Cmd.exe. Si se usa sin parámetros, **cmd** muestra la versión y la información de copyright del sistema operativo.

## <a name="syntax"></a>Sintaxis

```
cmd [/c|/k] [/s] [/q] [/d] [/a|/u] [/t:{<b><f> | <f>}] [/e:{on | off}] [/f:{on | off}] [/v:{on | off}] [<string>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /C | Lleva a cabo el comando especificado por *cadena* y, a continuación, se detiene. |
| /k | Lleva a cabo el comando especificado por *cadena* y continúa. |
| /s | Modifica el tratamiento de la *cadena* después de **/c** o **/k**. |
| /q | Desactiva el eco. |
| /d | Deshabilita la ejecución de comandos de ejecución automática. |
| /a | Da formato al resultado del comando interno en una canalización o un archivo como American National Standards Institute (ANSI). |
| /U | Da formato al resultado del comando interno en una canalización o un archivo como Unicode. |
| /t: {`<b><f>` | `<f>`} | Establece los colores de fondo (*b*) y de primer plano (*f*). |
| /e: activado | Habilita las extensiones de comandos. |
| /e: desactivado | Deshabilita las extensiones de comandos. |
| /f: activado | Habilita la finalización de nombres de archivos y directorios. |
| /f: desactivado | Deshabilita la finalización de nombres de archivos y directorios. |
| /v: activado | Habilita la expansión diferida de variables de entorno. |
| /v: desactivado | Deshabilita la expansión diferida de variables de entorno. |
| `<string>` | Especifica el comando que se desea llevar a cabo. |
| /? | Muestra la ayuda en el símbolo del sistema. |

En la tabla siguiente se enumeran los dígitos hexadecimales válidos que puede usar como valores para `<b>` y `<f>` :

| Valor | Color |
| ----- | ----- |
| 0 | Negro |
| 1 | Azul |
| 2 | Verde |
| 3 | Aqua |
| 4 | Red |
| 5 | Púrpura |
| 6 | Amarillo |
| 7 | Blanco |
| 8 | Gris |
| 9 | Azul claro |
| a | Verde claro |
| b | Aguamarina claro |
| c | Rojo claro |
| d | Púrpura claro |
| e | Amarillo claro |
| f | Blanco brillante |

## <a name="remarks"></a>Observaciones

- Para usar varios comandos para `<string>` , sepárelos mediante el separador **&&** de comandos y inclúyalo entre comillas. Por ejemplo:

    ```
    "<command1>&&<command2>&&<command3>"
    ```

- Si especifica **/c** o **/k**, los procesos **cmd** , el resto de la *cadena*y las comillas se conservan solo si se cumplen todas las condiciones siguientes:

    - No se usa también **/s**.

    - Se usa exactamente un conjunto de Comillas.

    - No se usa ningún carácter especial entre comillas (por ejemplo:  & < >  () @ ^ |).

    - Utilice uno o varios caracteres de espacio en blanco dentro de las comillas.

    - La *cadena* entre comillas es el nombre de un archivo ejecutable.

    Si no se cumplen las condiciones anteriores, la *cadena* se procesa examinando el primer carácter para comprobar si se trata de una comilla de apertura. Si el primer carácter es una comilla de apertura, se quita junto con la comilla de cierre. Se conserva cualquier texto que siga las comillas de cierre.

- Si no especifica **/d** en *cadena*, Cmd.exe busca las siguientes subclaves del registro:

    - **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\AutoRun\ REG_SZ**

    - **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\AutoRun\ REG_EXPAND_SZ**

    Si una o ambas subclaves del registro están presentes, se ejecutan antes que todas las demás variables.

    > [!CAUTION]
    > La edición incorrecta del Registro puede dañar gravemente el sistema. Antes de realizar cambios en el Registro, debe hacer una copia de seguridad de los datos de valor guardados en el equipo.

- Puede deshabilitar las extensiones de comando para un proceso determinado mediante **/e: OFF**. Puede habilitar o deshabilitar las extensiones para todas las opciones de línea de comandos de **cmd** en un equipo o una sesión de usuario mediante el establecimiento de los siguientes valores de **REG_DWORD** :

    - **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\EnableExtensions\ REG_DWORD**

    - **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\EnableExtensions\ REG_DWORD**

    Establezca el valor de **REG_DWORD** en **0 × 1** (habilitado) o en **0 × 0** (deshabilitado) en el registro mediante Regedit.exe. La configuración especificada por el usuario tiene prioridad sobre la configuración del equipo y las opciones de línea de comandos tienen prioridad sobre la configuración del registro.

    > [!CAUTION]
    > La edición incorrecta del Registro puede dañar gravemente el sistema. Antes de realizar cambios en el Registro, debe hacer una copia de seguridad de los datos de valor guardados en el equipo.

    Cuando se habilitan las extensiones de comando, se ven afectados los siguientes comandos:
    - **assoc**

    - **call**

    - **chdir (CD)**

    - **color**

    - **del (borrar)**

    - **endlocal**

    - **for**

    - **ftype**

    - **goto**

    - **if**

    - **mkdir (MD)**

    - **popd**

    - **prompt**

    - **pushd**

    - **set**

    - **setlocal**

    - **shift**

    - **iniciar** (también incluye cambios en los procesos de comandos externos)

- Si habilita la expansión diferida de variables de entorno, puede usar el carácter de signo de exclamación para sustituir el valor de una variable de entorno en tiempo de ejecución.

- La finalización de los nombres de archivos y directorios no está habilitada de forma predeterminada. Puede habilitar o deshabilitar la finalización de un nombre de archivo para un proceso determinado del comando **cmd** con **/f:**{**on**  |  **OFF**}. Puede habilitar o deshabilitar la finalización de nombres de archivos y directorios para todos los procesos del comando **cmd** en un equipo o para una sesión de inicio de sesión de usuario mediante el establecimiento de los siguientes valores de **REG_DWORD** :

    - **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\CompletionChar\ REG_DWORD**

    - **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\PathCompletionChar\ REG_DWORD**

    - **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\CompletionChar\ REG_DWORD**

    - **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\PathCompletionChar\ REG_DWORD**

    Para establecer el valor de **REG_DWORD** , ejecute Regedit.exe y use el valor hexadecimal de un carácter de control para una función determinada (por ejemplo, **0 × 9** es Tab y **0 × 08** es el retroceso). La configuración especificada por el usuario tiene prioridad sobre la configuración del equipo y las opciones de línea de comandos tienen prioridad sobre la configuración del registro.

    > [!CAUTION]
    > La edición incorrecta del Registro puede dañar gravemente el sistema. Antes de realizar cambios en el Registro, debe hacer una copia de seguridad de los datos de valor guardados en el equipo.

- Si habilita la finalización de nombres de archivos y directorios mediante **/f: on**, use **Ctrl + D** para completar el nombre de directorio y **Ctrl + f** para completar el nombre de archivo. Para deshabilitar un carácter de finalización determinado en el registro, use el valor de espacio en blanco [**0 × 20**] porque no es un carácter de control válido.

  - Al presionar **Ctrl + D** o **Ctrl + F**, se procesa la finalización de los nombres de archivo y directorio. Estas funciones de combinación de teclas anexan un carácter comodín a la *cadena* (si no hay ninguna), crea una lista de rutas de acceso que coinciden y, a continuación, muestra la primera ruta de acceso coincidente.<p>Si ninguna de las rutas de acceso coincide, la función de finalización de nombres de archivo y directorio emite un pitido y no cambia la pantalla. Para desplazarse por la lista de rutas de acceso coincidentes, presione **Ctrl + D** o **Ctrl + F** varias veces. Para desplazarse por la lista hacia atrás, presione la tecla **MAYÚS** y **Ctrl + D** o **Ctrl + F** simultáneamente. Para descartar la lista guardada de rutas de acceso coincidentes y generar una nueva lista, edite la *cadena* y presione **Ctrl + D** o **Ctrl + F**. Si cambia entre **Ctrl + D** y **Ctrl + F**, se descarta la lista guardada de rutas de acceso coincidentes y se genera una nueva lista. La única diferencia entre las combinaciones de teclas **Ctrl + d** y **Ctrl + f** es que **Ctrl + d** solo coincide con los nombres de directorio y **Ctrl + f** coincide con los nombres de archivo y de directorio. Si usa la finalización de nombres de archivo y directorio en cualquiera de los comandos de directorio integrados (es decir, **CD**, **MD**o **Rd**), se supone que se ha completado el directorio.

  - La finalización de nombres de archivos y directorios procesa correctamente los nombres de archivo que contienen espacios en blanco o caracteres especiales si se colocan comillas alrededor de la ruta de acceso coincidente.

  - Debe usar comillas alrededor de los siguientes caracteres especiales:  & < >  [] {} ^ =;! ' +, ' ~ [espacio en blanco].

  - Si la información proporcionada contiene espacios, debe usar comillas alrededor del texto (por ejemplo, "nombre del equipo").

  - Si procesa la finalización de nombres de archivos y directorios desde una *cadena*, se descarta cualquier parte de la *ruta de acceso* situada a la derecha del cursor (en el punto de la *cadena* donde se procesó la finalización).

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
