---
title: openfiles
description: Tema de referencia del comando openfiles, que permite a un administrador consultar, mostrar o desconectar archivos y directorios que se han abierto en un sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c3be561d-a11f-4bf1-9835-8e4e96fe98ec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e55dd88052f1bbfdebb02d1fb6d6f48261643c5c
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85472601"
---
# <a name="openfiles"></a>openfiles

Permite a un administrador consultar, mostrar o desconectar archivos y directorios que se han abierto en un sistema. Este comando también habilita o deshabilita la marca global **mantener la lista de objetos** del sistema.

## <a name="openfiles-disconnect"></a>openfiles/Disconnect

Permite a un administrador desconectar archivos y carpetas que se han abierto de forma remota a través de una carpeta compartida.

### <a name="syntax"></a>Sintaxis

```
openfiles /disconnect [/s <system> [/u [<domain>\]<username> [/p [<password>]]]] {[/id <openfileID>] | [/a <accessedby>] | [/o {read | write | read/write}]} [/op <openfile>]
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| modificado`<system>` | Especifica el sistema remoto al que se va a conectar (por nombre o dirección IP). No use barras diagonales inversas. Si no usa la opción **/s** , el comando se ejecuta de forma predeterminada en el equipo local. Este parámetro se aplica a todos los archivos y carpetas que se especifican en el comando. |
| /u`[<domain>\]<username>` | Ejecuta el comando con los permisos de la cuenta de usuario especificada. Si no usa la opción **/u** , se usan de forma predeterminada los permisos del sistema. |
| /p`[<password>]` | Especifica la contraseña de la cuenta de usuario especificada en la opción **/u** . Si no usa la opción **/p** , aparece un mensaje de contraseña cuando se ejecuta el comando. |
| /ID`<openfileID>` | Desconecta los archivos abiertos por el identificador de archivo especificado. Puede usar el carácter comodín (**&#42;**) con este parámetro.<p>Nota: puede usar el comando **openfiles/Query** para buscar el identificador de archivo. |
| /a`<accessedby>` | Desconecta todos los archivos abiertos asociados con el nombre de usuario especificado en el parámetro *accessedby* . Puede usar el carácter comodín (**&#42;**) con este parámetro. |
| /o`{read | write | read/write}` | Desconecta todos los archivos abiertos con el valor de modo de apertura especificado. Los valores válidos son **lectura**, **escritura**o **lectura/escritura**. Puede usar el carácter comodín (**&#42;**) con este parámetro. |
| /OP`<openfile>` | Desconecta todas las conexiones de archivos abiertas creadas por un nombre de archivo abierto específico. Puede usar el carácter comodín (**&#42;**) con este parámetro. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para desconectar todos los archivos abiertos con el *identificador de archivo 26843578*, escriba:

```
openfiles /disconnect /id 26843578
```

Para desconectar todos los archivos y directorios abiertos a los que tiene acceso el usuario *hiropln*, escriba:

```
openfiles /disconnect /a hiropln
```

Para desconectar todos los archivos y directorios abiertos con el *modo de lectura/escritura*, escriba:

```
openfiles /disconnect /o read/write
```

Para desconectar el directorio con el nombre de archivo abierto * C:\testshare \* , independientemente de quién tenga acceso a él, escriba:

```
openfiles /disconnect /a * /op c:\testshare\
```

Para desconectar todos los archivos abiertos en el equipo remoto *srvmain* a los que el usuario *hiropln*tiene acceso, independientemente de su identificador, escriba:

```
openfiles /disconnect /s srvmain /u maindom\hiropln /id *
```

## <a name="openfiles-query"></a>openfiles/Query

Consulta y muestra todos los archivos abiertos.

### <a name="syntax"></a>Sintaxis

```
openfiles /query [/s <system> [/u [<domain>\]<username> [/p [<password>]]]] [/fo {TABLE | LIST | CSV}] [/nh] [/v]
```

#### <a name="parameters"></a>Parámetros


| Parámetro | Descripción |
|--|--|
| modificado`<system>` | Especifica el sistema remoto al que se va a conectar (por nombre o dirección IP). No use barras diagonales inversas. Si no usa la opción **/s** , el comando se ejecuta de forma predeterminada en el equipo local. Este parámetro se aplica a todos los archivos y carpetas que se especifican en el comando. |
| /u`[<domain>\]<username>` | Ejecuta el comando con los permisos de la cuenta de usuario especificada. Si no usa la opción **/u** , se usan de forma predeterminada los permisos del sistema. |
| /p`[<password>]` | Especifica la contraseña de la cuenta de usuario especificada en la opción **/u** . Si no usa la opción **/p** , aparece un mensaje de contraseña cuando se ejecuta el comando. |
| [/FO `{TABLE | LIST | CSV}` ] | Muestra el resultado en el formato especificado. Los valores válidos son:<ul><li>**Tabla** : muestra la salida en una tabla.</li><li>**List** : muestra la salida en una lista.</li><li>**CSV** : muestra la salida en formato de valores separados por comas (CSV).</li></ul> |
| /NH | Suprime los encabezados de columna en la salida. Solo es válido cuando el parámetro **/FO** está establecido en **TABLE** o **CSV**. |
| /v | Especifica que se muestre información detallada (detallada) en la salida. |
| /? | Muestra la ayuda en el símbolo del sistema. |

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

Para consultar y Mostrar todos los archivos abiertos en el sistema remoto *srvmain* con las credenciales del usuario *hiropln* en el dominio *maindom* , escriba:

```
openfiles /query /s srvmain /u maindom\hiropln /p p@ssW23
```

> [!NOTE]
> En este ejemplo, la contraseña se proporciona en la línea de comandos. Para evitar que se muestre la contraseña, deje la opción **/p** . Se le pedirá la contraseña, que no se mostrará en la pantalla.

## <a name="openfiles-local"></a>openfiles/local

Habilita o deshabilita la marca global **mantener la lista de objetos** del sistema. Si se usa sin parámetros, **openfiles/local** muestra el estado actual de la marca global **mantener lista de objetos** .

> [!NOTE]
> Los cambios realizados mediante la opción **activado** o **desactivado** no surtirán efecto hasta que se reinicie el sistema. La habilitación de la marca global **mantener lista de objetos** puede ralentizar el sistema.

### <a name="syntax"></a>Sintaxis

```
openfiles /local [on | off]
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `[on | off]` | Habilita o deshabilita la marca global **mantener la lista de objetos** del sistema, que realiza un seguimiento de los identificadores de archivos locales. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para comprobar el estado actual de la marca global **mantener lista de objetos** , escriba:

```
openfiles /local
```

De forma predeterminada, la marca global **mantener lista de objetos** está deshabilitada y aparece el siguiente mensaje:`INFO: The system global flag 'maintain objects list' is currently disabled.`

Para habilitar la marca global **mantener lista de objetos** , escriba:

```
openfiles /local on
```

El siguiente mensaje aparece cuando la marca global está habilitada:`SUCCESS: The system global flag 'maintain objects list' is enabled. This will take effect after the system is restarted.`

Para deshabilitar la marca global **mantener lista de objetos** , escriba:

```
openfiles /local off
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
