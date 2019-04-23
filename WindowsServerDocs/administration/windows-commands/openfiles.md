---
title: openfiles
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c3be561d-a11f-4bf1-9835-8e4e96fe98ec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 83175d8529d9204c6b6d969a3db2aee2775bd0c4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852386"
---
# <a name="openfiles"></a>openfiles



Permite a un administrador de consulta, mostrar o desconectar los archivos y directorios que se han abierto en un sistema. También habilita o deshabilita el indicador global Mantener lista de objetos del sistema.

En este tema incluye información acerca de los siguientes comandos:
-   [openfiles /disconnect](#BKMK_disconnect)
-   [openfiles /query](#BKMK_query)
-   [openfiles /local](#BKMK_local)

## <a name="BKMK_disconnect"></a>openfiles /disconnect

Permite a un administrador desconectar archivos y carpetas que se han abierto de forma remota a través de una carpeta compartida.

### <a name="syntax"></a>Sintaxis

```
openfiles /disconnect [/s <System> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/id <OpenFileID>] | [/a <AccessedBy>] | [/o {read | write | read/write}]} [/op <OpenFile>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/s \<system >|Especifica el sistema remoto para conectarse a (por nombre o dirección IP). No use barras diagonales inversas. Si no usa el **/s** opción, el comando se ejecuta en el equipo local de forma predeterminada. Este parámetro se aplica a todos los archivos y carpetas que se especifican en el comando.|
|/u [\<Domain>\]<UserName>|Ejecuta el comando con los permisos de la cuenta de usuario especificado. Si no usa el **/u** opción, el sistema de permisos se utilizan de forma predeterminada.|
|/p [\<Password>]|Especifica la contraseña de la cuenta de usuario que se especifica en el **/u** opción. Si no usa el **/p** opción, una solicitud de contraseña aparece cuando se ejecuta el comando.|
|/id \<OpenFileID>|Desconecta archivos abiertos por el identificador de archivo especificado. El carácter comodín (**&#42;**) se puede usar con este parámetro.</br>Nota: Puede usar el **openfiles /query** comando para buscar el identificador de archivo.|
|/a \<Con_acceso_por >|Desconecta todos los archivos abiertos asociados con el nombre de usuario que se especifica en el *Con_acceso_por* parámetro. El carácter comodín (**&#42;**) se puede usar con este parámetro.|
|/o {leer \| escribir \| lectura/escritura}|Desconecta todos los archivos abiertos con el valor del modo de apertura especificada. Los valores válidos son de lectura, escritura o lectura/escritura. El carácter comodín (**&#42;**) se puede usar con este parámetro.|
|/ Op. \<OpenFile >|Desconecta todas las conexiones de archivos abiertos que se crean mediante un nombre de archivo abiertos específicos. El carácter comodín (**&#42;**) se puede usar con este parámetro.|
|/?|Muestra la ayuda en el símbolo del sistema.|

### <a name="examples"></a>Ejemplos

Para desconectar a todos los archivos abiertos con el archivo 26843578 ID, escriba:
```
openfiles /disconnect /id 26843578
```
Para desconectar a todos los archivos abiertos y directorios accesibles por el usuario "hiropln", escriba:
```
openfiles /disconnect /a hiropln
```
Para desconectar a todos los archivos abiertos y directorios con el modo de lectura/escritura, escriba:
```
openfiles /disconnect /o read/write
```
Para desconectar el directorio con el nombre de archivo abierto "C:\TestShare\", independientemente de quién tiene acceso a él, tipo:
```
openfiles /disconnect /a * /op "c:\testshare\"
```
Para desconectar a todos los archivos abiertos en el equipo remoto "srvmain" que se tiene acceso el usuario "hiropln", independientemente de su identificador, escriba:
```
openfiles /disconnect /s srvmain /u maindom\hiropln /id *
```

## <a name="BKMK_query"></a>openfiles /query

Consulta y muestra todos los archivos abiertos.

### <a name="syntax"></a>Sintaxis

```
openfiles /query [/s <System> [/u [<Domain>\]<UserName> [/p [<Password>]]]] [/fo {TABLE | LIST | CSV}] [/nh] [/v]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/s \<system >|Especifica el sistema remoto para conectarse a (por nombre o dirección IP). No use barras diagonales inversas. Si no usa el **/s** opción, el comando se ejecuta en el equipo local de forma predeterminada. Este parámetro se aplica a todos los archivos y carpetas que se especifican en el comando.|
|/u [\<Domain>\]<UserName>|Ejecuta el comando con los permisos de la cuenta de usuario especificado. Si no usa el **/u** opción, el sistema de permisos se utilizan de forma predeterminada.|
|/p [\<Password>]|Especifica la contraseña de la cuenta de usuario que se especifica en el **/u** opción. Si no usa el **/p** opción, una solicitud de contraseña aparece cuando se ejecuta el comando.|
|[/ FO {tabla \| lista \| CSV}]|Muestra el resultado en el formato especificado. Los valores válidos para *formato* son:</br>TABLA:  Muestra los resultados en una tabla.</br>LISTA: Muestra los resultados en una lista.</br>CSV: Muestra la salida en formato de valores separados por comas.|
|/nh|Suprime el encabezado de columna en la salida. Solo es válido cuando el **/fo** parámetro está establecido en **tabla** o **CSV**.|
|/v|Especifica que se muestra información detallada en la salida.|
|/?|Muestra la ayuda en el símbolo del sistema.|

### <a name="examples"></a>Ejemplos

Para consultar y mostrar todos los archivos abiertos, escriba:
```
openfiles /query
```
Para consultar y mostrar todos los archivos abiertos en formato de tabla sin encabezados, escriba:
```
openfiles /query /fo table /nh
```
Para consultar y mostrar todos los archivos abiertos en formato de lista con información detallada, escriba:
```
openfiles /query /fo list /v
```
Para consultar y mostrar todos los archivos abiertos en el sistema remoto "srvmain" con las credenciales del usuario "hiropln" en el dominio "maindom", escriba:
```
openfiles /query /s srvmain /u maindom\hiropln /p p@ssW23
```

> [!NOTE]
> En este ejemplo, la contraseña se proporciona en la línea de comandos. Para evitar que muestre la contraseña, omita el **/p** opción. Se le pedirá la contraseña, que no se mostrará en la pantalla.

## <a name="BKMK_local"></a>openfiles /local

Habilita o deshabilita el indicador global Mantener lista de objetos del sistema. Si se utiliza sin parámetros, **openfiles /local** muestra el estado actual del indicador global Mantener lista de objetos.

### <a name="syntax"></a>Sintaxis

```
openfiles /local [on | off]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[en \| off]|Habilita o deshabilita el indicador global Mantener lista de objetos, que realiza un seguimiento de los identificadores de archivos local del sistema.|
|/?|Muestra la ayuda en el símbolo del sistema.|

### <a name="remarks"></a>Comentarios

-   Habilitación de la marca global de mantener la lista de objetos puede ralentizar el sistema.
-   Los cambios realizados mediante el **en** o **desactivar** opción no surtirán efecto hasta que reinicie el sistema.

### <a name="examples"></a>Ejemplos

Para comprobar el estado actual del indicador global Mantener lista de objetos, escriba:
```
openfiles /local
```
De forma predeterminada, la marca global de mantener la lista de objetos está deshabilitada y se muestra el resultado siguiente:
```
INFO: The system global flag 'maintain objects list' is currently disabled.
```
Para habilitar la marca global de mantener la lista de objetos, escriba:
```
openfiles /local on
```
Cuando se habilita la marca global, se muestra el mensaje siguiente:
```
SUCCESS: The system global flag 'maintain objects list' is enabled.
         This will take effect after the system is restarted.
```
Para deshabilitar la marca global de mantener la lista de objetos, escriba:
```
openfiles /local off
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)