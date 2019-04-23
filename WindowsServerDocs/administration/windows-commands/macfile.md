---
title: macfile
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e2ce586c-b316-41d3-90f8-4be0d074cc0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c50c53fc277626d1b83268f5e0c8dcc95161f35a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854056"
---
# <a name="macfile"></a>macfile

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Administra el servidor de archivos para servidores, volúmenes, directorios y archivos de Macintosh. Puede automatizar las tareas administrativas mediante la inclusión de una serie de comandos en archivos por lotes e iniciarse manualmente para probarlos o en momentos predeterminados. 
-   [Para modificar los directorios en los volúmenes Macintosh](#BKMK_Moddirs)
-   [Para unir la bifurcación de recursos y datos de un archivo de Macintosh](#BKMK_Joinforks)
-   [Para cambiar el mensaje de inicio de sesión y limitar las sesiones](#BKMK_LogonLimit)
-   [Para agregar, cambiar o quitar volúmenes Macintosh](#BKMK_addvol)

## <a name="BKMK_Moddirs"></a>Para modificar los directorios en los volúmenes Macintosh

### <a name="syntax"></a>Sintaxis
```
macfile directory[/server:\\<computerName>] /path:<directory> [/owner:<OwnerName>] [/group:<GroupName>] [/permissions:<Permissions>]
```

### <a name="parameters"></a>Parámetros
-   /server:\\\\<computerName> Especifica el servidor en el que se cambie a un directorio. Si se omite, la operación se realiza en el equipo local.
-   / Path:<directory> Obligatorio. Especifica la ruta de acceso al directorio que desea cambiar. El directorio debe existir. **directorio MacFile** no crea directorios.
-   / owner:<OwnerName> cambia el propietario del directorio. Si se omite, se cambia el propietario.
-   / Group:<GroupName> Especifica o cambia el grupo principal de Macintosh que está asociado con el directorio. Si se omite, el grupo principal permanece sin cambios.
-   / Permissions:<Permissions> Establece permisos en el directorio para el propietario, el grupo primario y el mundo (todos). Un número de dígitos 11 se usa para establecer permisos. El número 1 concede el permiso y 0 revoca el permiso (por ejemplo, 11111011000). Si se omite, los permisos permanecen sin cambios.
    La posición del dígito determina qué permiso se establece, como se describe en la tabla siguiente.
    
    |Posición|Establece el permiso para|
    |------|------------|
    |Primero|OwnerSeeFiles|
    |Second|OwnerSeeFolders|
    |Tercero|OwnerMakechanges|
    |Cuarto|GroupSeeFiles|
    |Quinto argumento|GroupSeeFolders|
    |Sexto|GroupMakechanges|
    |Séptimo|WorldSeeFiles|
    |Octavo argumento|WorldSeeFolders|
    |Noveno argumento|WorldMakechanges|
    |Décimo argumento|El directorio no se puede cambiar el nombre, mover o eliminar.|
    |Undécimo argumento|Los cambios se aplican en el directorio actual y todos los subdirectorios.|
    
-   /?
    Muestra la ayuda en el símbolo del sistema.
    
### <a name="remarks"></a>Comentarios
-   Si la información que se proporciona contiene espacios ni caracteres especiales, utilice comillas alrededor del texto (por ejemplo, **"***nombre del equipo***"**).
-   Use **macfiledirectory** poner a disposición de los usuarios de Macintosh un directorio existente en un volumen accesible para Macintosh. El **macfiledirectory** comando no crea directorios. Utilice el Administrador de archivos, el símbolo del sistema, o el **nueva carpeta de macintosh** comando para crear un directorio en un volumen accesible para Macintosh antes de usar el **macfile directory** comando.
### <a name="BKMK_Examples"></a>Ejemplos
El ejemplo siguiente cambia los permisos de las ventas de mayo de subdirectorio en el volumen accesible para Macintosh estadísticas, en la unidad E del servidor local. El ejemplo asigna permisos ver archivos, ver carpetas y hacer cambios al propietario y los permisos ver archivos y ver carpetas a todos los demás usuarios, evitando que el directorio de nombre, mover o eliminar.
```
macfile directory /path:"e:\statistics\may sales" /permissions:11111011000
```

## <a name="BKMK_Joinforks"></a>Para unir la bifurcación de recursos y datos de un archivo de Macintosh

### <a name="syntax"></a>Sintaxis
```
macfile forkize[/server:\\<computerName>] [/creator:<CreatorName>] [/type:<typeName>]  [/datafork:<Filepath>] [/resourcefork:<Filepath>] /targetfile:<Filepath>
```

### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/server:\\\\<computerName>|Especifica el servidor en el que se va a unir a los archivos. Si se omite, la operación se realiza en el equipo local.|
|/ Creator:<CreatorName>|Especifica el creador del archivo. El buscador de Macintosh utiliza el **/Creator** opción de línea de comandos para determinar la aplicación que creó el archivo.|
|/type:<typeName>|Especifica el tipo de archivo. El buscador de Macintosh utiliza el **/tipo** opción de línea de comandos para determinar el tipo de archivo dentro de la aplicación que creó el archivo.|
|/datafork:<Filepath>|Especifica la ubicación de la bifurcación de datos que va a combinarse. Puede especificar una ruta de acceso remoto.|
|/resourcefork:<Filepath>|Especifica la ubicación de la bifurcación de recursos que va a combinarse. Puede especificar una ruta de acceso remoto.|
|targetfile:<Filepath>|Obligatorio. Especifica la ubicación del archivo que se crea mediante la combinación de una bifurcación de datos y una bifurcación de recursos o especifica la ubicación del archivo cuyo tipo o creador que se va a cambiar. El archivo debe estar en el servidor especificado.|
|/?|Muestra la ayuda en el símbolo del sistema.|

### <a name="remarks"></a>Comentarios
-   Si la información que se proporciona contiene espacios ni caracteres especiales, utilice comillas alrededor del texto (por ejemplo, **"***nombre del equipo***"**).

### <a name="examples"></a>Ejemplos
Para crear el archivo treeapp en el volumen accesible para Macintosh D:\Release mediante la bifurcación de recursos C:\Cross\Mac\Appcode y para hacer que este nuevo archivo aparecen en los clientes de Macintosh como una aplicación (aplicaciones Macintosh utilizan el tipo APPL) con el creador (firma ) establecido como MAGNOLIA, tipo:
```
macfile forkize /resourcefork:c:\cross\mac\appcode /type:APPL /creator:MAGNOLIA /targetfile:D:\Release\treeapp
```
Para cambiar el creador del archivo a Microsoft Word 5.1, el archivo WOrd.txt en el directorio D:\Word Word\Archivos, en el servidor \\\SERverA, escriba:
```
macfile forkize /server:\\servera /creator:MSWD /type:TEXT /targetfile:"d:\Word documents\Group files\Word.txt"
```

## <a name="BKMK_LogonLimit"></a>Para cambiar el mensaje de inicio de sesión y limitar las sesiones
### <a name="syntax"></a>Sintaxis
```
macfile server [/server:\\<computerName>] [/maxsessions:{Number | unlimited}] [/loginmessage:<Message>]
```

### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/server:\\\\<computerName>|Especifica el servidor en el que se va a cambiar los parámetros. Si se omite, la operación se realiza en el equipo local.|
|/maxsessions:{Number &#124; unlimited}|Especifica el número máximo de usuarios que puede usar el archivo y servidores de impresión para Macintosh simultáneamente. Si se omite, el **maxsessions** configuración para el servidor permanece sin cambios.|
|loginmessage:<Message>|los cambios de los usuarios de Macintosh mensaje Consulte al iniciar sesión en el servidor de archivos para Macintosh. El número máximo de caracteres para el mensaje de inicio de sesión es 199. Si se omite, el **loginmessage** del mensaje para el servidor permanece sin cambios. Para quitar un mensaje de inicio de sesión existente, incluya el **loginmessage** parámetro, pero deje el *mensaje* variable en blanco.|
|/?|Muestra la ayuda en el símbolo del sistema.|

### <a name="remarks"></a>Comentarios
-   Si la información que se proporciona contiene espacios ni caracteres especiales, utilice comillas alrededor del texto (por ejemplo, **"***nombre del equipo***"**).

### <a name="examples"></a>Ejemplos
Para cambiar el número de archivo y el servidor de impresión para las sesiones de Macintosh que se permiten en el servidor local de la configuración actual a cinco sesiones y agregar el mensaje de inicio de sesión "Cierre la sesión del servidor para Macintosh cuando haya terminado.", escriba:
```
macfile server /maxsessions:5 /loginmessage:"Log off from Server for Macintosh when you are finished."
```

## <a name="BKMK_addvol"></a>Para agregar, cambiar o quitar volúmenes Macintosh
### <a name="syntax"></a>Sintaxis
```
macfile volume {/add|/set} [/server:\\<computerName>] /name:<volumeName>/path:<directory>[/readonly:{true | false}] [/guestsallowed:{true | false}] [/password:<Password>] [/maxusers:{<Number>>|unlimited}]
macfile volume /remove[/server:\\<computerName>] /name:<volumeName>
```

### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|{/add &#124; /set}|Obligatorio cuando va a agregar o cambiar un volumen accesible para Macintosh. Agrega o cambia el volumen especificado.|
|/server:\\\\<computerName>|Especifica el servidor en el que se va a agregar, cambiar o quitar un volumen. Si se omite, la operación se realiza en el equipo local.|
|/ Name:<volumeName>|Obligatorio. Especifica el nombre de volumen que se puede agregar, cambiar o quitar.|
|/ Path:<directory>|Requerido y válido solo cuando se va a agregar un volumen. Especifica la ruta de acceso al directorio raíz del volumen que se va a agregar.|
|/readonly:{true &#124; false}|Especifica si los usuarios pueden cambiar los archivos del volumen. Escriba true para especificar que los usuarios no pueden cambiar los archivos del volumen. Escriba false para especificar que los usuarios pueden cambiar los archivos del volumen. Si se omite al agregar un volumen, se permiten cambios en los archivos. Si se omite al cambiar un volumen, el **readonly** configuración para el volumen permanece sin cambios.|
|/guestsallowed:{true &#124; false}|Especifica si los usuarios que inician sesión como invitados pueden usar el volumen. Escriba true para especificar que los invitados pueden usar el volumen. Escriba false para especificar que los invitados no pueden usar el volumen. Si se omite al agregar un volumen, los invitados pueden usar el volumen. Si se omite al cambiar un volumen, el **guestsallowed** configuración para el volumen permanece sin cambios.|
|/password:<Password>|Especifica una contraseña que será necesario obtener acceso al volumen. Si se omite al agregar un volumen, no se crea ninguna contraseña. Si se omite al cambiar un volumen, no se cambia la contraseña.|
|/maxusers:{<Number>>&#124;unlimited}|Especifica el número máximo de usuarios que pueden utilizar simultáneamente los archivos en el volumen. Si se omite al agregar un volumen, un número ilimitado de usuarios puede usar el volumen. Si se omite al cambiar un volumen, el **maxusers** valor permanece sin cambios.|
|/remove|Obligatorio cuando va a quitar un volumen accesible a Macintosh. Quita el volumen especificado.|
|/?|Muestra la ayuda en el símbolo del sistema.|

### <a name="remarks"></a>Comentarios
-   Si la información que se proporciona contiene espacios ni caracteres especiales, utilice comillas alrededor del texto (por ejemplo, **"***nombre del equipo***"**).

### <a name="examples"></a>Ejemplos
Para crear un volumen denominado nos estadísticas de Marketing en el servidor local, utilizando el directorio de estadísticas de la unidad E y para especificar que el volumen no se puede tener acceso a los invitados, escriba:
```
macfile volume /add /name:"US Marketing Statistics" /guestsallowed:false /path:e:\Stats
```
Para cambiar el volumen creado anteriormente a ser de solo lectura y se requiere una contraseña y para establecer el número de usuarios máximos en cinco, tipo:
```
macfile volume /set /name:"US Marketing Statistics" /readonly:true /password:saturn /maxusers:5
```
Para agregar un volumen denominado diseño horizontal, en el servidor \\\Magnolia, usando el directorio árboles de la unidad E y para especificar que el volumen puede tener acceso a los invitados, escriba:
```
macfile volume /add /server:\\Magnolia /name:"Landscape Design" /path:e:\trees
```
Para quitar el volumen denominado informes de ventas en el servidor local, escriba:
```
macfile volume /remove /name:"Sales Reports"
```

## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
