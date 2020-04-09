---
title: openfiles
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c3be561d-a11f-4bf1-9835-8e4e96fe98ec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f684acc48fbb279ced8ce1dfb3a930ff15f3bf13
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837828"
---
# <a name="openfiles"></a>openfiles



Permite a un administrador consultar, mostrar o desconectar archivos y directorios que se han abierto en un sistema. También habilita o deshabilita la marca global mantener la lista de objetos del sistema.

En este tema se incluye información acerca de los siguientes comandos:
-   [openfiles/Disconnect](#BKMK_disconnect)
-   [openfiles/Query](#BKMK_query)
-   [openfiles/local](#BKMK_local)

## <a name="openfiles-disconnect"></a><a name=BKMK_disconnect></a>openfiles/Disconnect

Permite a un administrador desconectar archivos y carpetas que se han abierto de forma remota a través de una carpeta compartida.

### <a name="syntax"></a>Sintaxis

```
openfiles /disconnect [/s <System> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/id <OpenFileID>] | [/a <AccessedBy>] | [/o {read | write | read/write}]} [/op <OpenFile>]
```

#### <a name="parameters"></a>Parámetros

|            Parámetro             |                                                                                                                                 Descripción                                                                                                                                  |
|----------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /s \<System >           | Especifica el sistema remoto al que se va a conectar (por nombre o dirección IP). No use barras diagonales inversas. Si no usa la opción **/s** , el comando se ejecuta de forma predeterminada en el equipo local. Este parámetro se aplica a todos los archivos y carpetas que se especifican en el comando. |
|    /u [\<\]de > de dominio <UserName>     |                                                          Ejecuta el comando con los permisos de la cuenta de usuario especificada. Si no usa la opción **/u** , se usan de forma predeterminada los permisos del sistema.                                                           |
|         /p [\<contraseña >]         |                                               Especifica la contraseña de la cuenta de usuario especificada en la opción **/u** . Si no usa la opción **/p** , aparece un mensaje de contraseña cuando se ejecuta el comando.                                                |
|        /ID \<OpenFileID >         |                                       Desconecta los archivos abiertos por el identificador de archivo especificado. El carácter comodín ( **&#42;** ) se puede usar con este parámetro.</br>Nota: puede usar el comando **openfiles/Query** para buscar el identificador de archivo.                                       |
|         /a \<AccessedBy >         |                                                Desconecta todos los archivos abiertos asociados con el nombre de usuario que se especifica en el parámetro *AccessedBy* . El carácter comodín ( **&#42;** ) se puede usar con este parámetro.                                                 |
| /o {lectura \| escritura \| lectura/escritura} |                                               Desconecta todos los archivos abiertos con el valor de modo de apertura especificado. Los valores válidos son lectura, escritura o lectura/escritura. El carácter comodín ( **&#42;** ) se puede usar con este parámetro.                                                |
|         /OP \<OpenFile >          |                                                           Desconecta todas las conexiones de archivos abiertas creadas por un nombre de archivo abierto específico. El carácter comodín ( **&#42;** ) se puede usar con este parámetro.                                                           |
|                /?                |                                                                                                                     Muestra la Ayuda en el símbolo del sistema.                                                                                                                     |

### <a name="examples"></a>Ejemplos

Para desconectar todos los archivos abiertos con el identificador de archivo 26843578, escriba:
```
openfiles /disconnect /id 26843578
```
Para desconectar todos los archivos y directorios abiertos a los que tiene acceso el usuario hiropln, escriba:
```
openfiles /disconnect /a hiropln
```
Para desconectar todos los archivos y directorios abiertos con el modo de lectura/escritura, escriba:
```
openfiles /disconnect /o read/write
```
Para desconectar el directorio con el nombre de archivo abierto C:\TestShare\, independientemente de quién tenga acceso a él, escriba:
```
openfiles /disconnect /a * /op c:\testshare\
```
Para desconectar todos los archivos abiertos en el equipo remoto srvmain a los que el usuario hiropln tiene acceso, independientemente de su identificador, escriba:
```
openfiles /disconnect /s srvmain /u maindom\hiropln /id *
```

## <a name="openfiles-query"></a><a name=BKMK_query></a>openfiles/Query

Consulta y muestra todos los archivos abiertos.

### <a name="syntax"></a>Sintaxis

```
openfiles /query [/s <System> [/u [<Domain>\]<UserName> [/p [<Password>]]]] [/fo {TABLE | LIST | CSV}] [/nh] [/v]
```

#### <a name="parameters"></a>Parámetros

|          Parámetro           |                                                                                                                                 Descripción                                                                                                                                  |
|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         /s \<System >         | Especifica el sistema remoto al que se va a conectar (por nombre o dirección IP). No use barras diagonales inversas. Si no usa la opción **/s** , el comando se ejecuta de forma predeterminada en el equipo local. Este parámetro se aplica a todos los archivos y carpetas que se especifican en el comando. |
|  /u [\<\]de > de dominio <UserName>   |                                                          Ejecuta el comando con los permisos de la cuenta de usuario especificada. Si no usa la opción **/u** , se usan de forma predeterminada los permisos del sistema.                                                           |
|       /p [\<contraseña >]       |                                               Especifica la contraseña de la cuenta de usuario especificada en la opción **/u** . Si no usa la opción **/p** , aparece un mensaje de contraseña cuando se ejecuta el comando.                                                |
| [/FO {TABLE \| LIST \| CSV}] |                             Muestra el resultado en el formato especificado. Los valores válidos para *Format* son:</br>TABLA: muestra la salida en una tabla.</br>LIST: muestra la salida en una lista.</br>CSV: muestra la salida en formato de valores separados por comas.                              |
|             /NH              |                                                                                Suprime el encabezado de columna en la salida. Solo es válido cuando el parámetro **/FO** está establecido en **TABLE** o **CSV**.                                                                                 |
|              /v              |                                                                                                       Especifica que la información detallada se muestra en la salida.                                                                                                        |
|              /?              |                                                                                                                     Muestra la Ayuda en el símbolo del sistema.                                                                                                                     |

### <a name="examples"></a>Ejemplos

Para consultar y Mostrar todos los archivos abiertos, escriba:
```
openfiles /query
```
Para consultar y Mostrar todos los archivos abiertos en formato de tabla sin encabezados, escriba:
```
openfiles /query /fo table /nh
```
Para consultar y Mostrar todos los archivos abiertos en formato de lista con información detallada, escriba:
```
openfiles /query /fo list /v
```
Para consultar y Mostrar todos los archivos abiertos en el sistema remoto srvmain con las credenciales del usuario hiropln en el dominio maindom, escriba:
```
openfiles /query /s srvmain /u maindom\hiropln /p p@ssW23
```

> [!NOTE]
> En este ejemplo, la contraseña se proporciona en la línea de comandos. Para evitar que se muestre la contraseña, deje la opción **/p** . Se le pedirá la contraseña, que no se mostrará en la pantalla.

## <a name="openfiles-local"></a><a name=BKMK_local></a>openfiles/local

Habilita o deshabilita la marca global mantener la lista de objetos del sistema. Si se usa sin parámetros, **openfiles/local** muestra el estado actual de la marca global mantener lista de objetos.

### <a name="syntax"></a>Sintaxis

```
openfiles /local [on | off]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[on \| OFF]|Habilita o deshabilita la marca global mantener la lista de objetos del sistema, que realiza un seguimiento de los identificadores de archivos locales.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

### <a name="remarks"></a>Comentarios

-   La habilitación de la marca global mantener lista de objetos puede ralentizar el sistema.
-   Los cambios realizados mediante la opción **activado** o **desactivado** no surtirán efecto hasta que se reinicie el sistema.

### <a name="examples"></a>Ejemplos

Para comprobar el estado actual de la marca global mantener lista de objetos, escriba:
```
openfiles /local
```
De forma predeterminada, la marca global mantener lista de objetos está deshabilitada y se muestra el siguiente resultado:
```
INFO: The system global flag 'maintain objects list' is currently disabled.
```
Para habilitar la marca global mantener lista de objetos, escriba:
```
openfiles /local on
```
Cuando la marca global está habilitada, se muestra el siguiente mensaje:
```
SUCCESS: The system global flag 'maintain objects list' is enabled.
         This will take effect after the system is restarted.
```
Para deshabilitar la marca global mantener lista de objetos, escriba:
```
openfiles /local off
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)