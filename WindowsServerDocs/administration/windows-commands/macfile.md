---
title: macfile
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e2ce586c-b316-41d3-90f8-4be0d074cc0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a4384ce8d4f23966aea278e90b9ddddc42da6e77
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724232"
---
# <a name="macfile"></a>macfile

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Administra el servidor de archivos para servidores, volúmenes, directorios y archivos de Macintosh. Puede automatizar las tareas administrativas mediante la inclusión de una serie de comandos en archivos por lotes y su inicio manual o en momentos predeterminados. 
-   [Para modificar directorios en volúmenes accesibles desde Macintosh](#BKMK_Moddirs)
-   [Para unir los datos y las bifurcaciones de recursos de un archivo de Macintosh](#BKMK_Joinforks)
-   [Para cambiar el mensaje de inicio de sesión y las sesiones de límite](#BKMK_LogonLimit)
-   [Para agregar, cambiar o quitar volúmenes accesibles desde Macintosh](#BKMK_addvol)

## <a name="to-modify-directories-in-macintosh-accessible-volumes"></a><a name=BKMK_Moddirs></a>Para modificar directorios en volúmenes accesibles desde Macintosh

### <a name="syntax"></a>Sintaxis
```
macfile directory[/server:\\<computerName>] /path:<directory> [/owner:<OwnerName>] [/group:<GroupName>] [/permissions:<Permissions>]
```

#### <a name="parameters"></a>Parámetros
-   /Server:\\ \\ <computerName> especifica el servidor en el que se va a cambiar un directorio. Si se omite, la operación se realiza en el equipo local.
-   /path:<directory> obligatorio. Especifica la ruta de acceso al directorio que desea cambiar. El directorio debe existir. el **directorio MacFile** no crea directorios.
-   /Owner:<OwnerName> cambia el propietario del directorio. Si se omite, el propietario permanece inalterado.
-   /Group:<GroupName> especifica o cambia el grupo principal de Macintosh que está asociado con el directorio. Si se omite, el grupo primario permanece inalterado.
-   /Permissions:<Permissions> establece permisos en el directorio para el propietario, el grupo principal y el mundo (todos). Se usa un número de 11 dígitos para establecer los permisos. El número 1 concede el permiso y 0 revoca el permiso (por ejemplo, 11111011000). Si se omite, los permisos permanecen inalterados.
    La posición del dígito determina qué permiso se establece, tal y como se describe en la tabla siguiente.

    |Posición|Establece el permiso para|
    |------|------------|
    |Primero|OwnerSeeFiles|
    |Segundo|OwnerSeeFolders|
    |Tercero|OwnerMakechanges|
    |Cuarto|GroupSeeFiles|
    |Quinto|GroupSeeFolders|
    |Sexto|GroupMakechanges|
    |Séptimo|WorldSeeFiles|
    |Octava|WorldSeeFolders|
    |Novena|WorldMakechanges|
    |Décima|No se puede cambiar el nombre del directorio, moverlo o eliminarlo.|
    |11|Los cambios se aplican al directorio actual y a todos los subdirectorios.|

-   /?
    Muestra la ayuda en el símbolo del sistema.

### <a name="remarks"></a>Observaciones
- Si la información proporcionada contiene espacios o caracteres especiales, use comillas alrededor del texto (por ejemplo, * * * * nombre del<em>equipo</em>* * * *).
- Use **macfiledirectory** para hacer que un directorio existente en un volumen accesible a Macintosh esté disponible para los usuarios de Macintosh. El comando **macfiledirectory** no crea directorios. Use el comando administrador de archivos, el símbolo del sistema o la **nueva carpeta Macintosh** para crear un directorio en un volumen accesible desde Macintosh antes de usar el comando **MacFile Directory** .
  ### <a name="examples"></a>Ejemplos
  Para cambiar los permisos del subdirectorio, puede haber ventas en las estadísticas de volumen accesible a Macintosh, en la unidad E del servidor local. En el ejemplo se asignan los permisos ver archivos, ver carpetas y realizar cambios al propietario y ver los permisos de carpetas para todos los demás usuarios, a la vez que se impide que el directorio se cambie de nombre, se mueva o se elimine.
  ```
  macfile directory /path:e:\statistics\may sales /permissions:11111011000
  ```

## <a name="to-join-a-macintosh-files-data-and-resource-forks"></a><a name=BKMK_Joinforks></a>Para unir los datos y las bifurcaciones de recursos de un archivo de Macintosh

### <a name="syntax"></a>Sintaxis
```
macfile forkize[/server:\\<computerName>] [/creator:<CreatorName>] [/type:<typeName>]  [/datafork:<Filepath>] [/resourcefork:<Filepath>] /targetfile:<Filepath>
```

#### <a name="parameters"></a>Parámetros

|         Parámetro          |                                                                                                           Descripción                                                                                                            |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Server\\\\<computerName> |                                                            Especifica el servidor en el que se van a unir los archivos. Si se omite, la operación se realiza en el equipo local.                                                            |
|   /Creator<CreatorName>   |                                      Especifica el creador del archivo. El buscador de Macintosh usa la opción de línea de comandos **/Creator** para determinar la aplicación que creó el archivo.                                       |
|      /Type<typeName>      |                                 Especifica el tipo de archivo. El buscador de Macintosh usa la opción de línea de comandos **/Type** para determinar el tipo de archivo dentro de la aplicación que creó el archivo.                                 |
|    /datafork:<Filepath>    |                                                                   Especifica la ubicación de la bifurcación de datos que se va a combinar. Puede especificar una ruta de acceso remota.                                                                   |
|  /resourcefork:<Filepath>  |                                                                 Especifica la ubicación de la bifurcación de recursos que se va a combinar. Puede especificar una ruta de acceso remota.                                                                 |
|   /targetfile<Filepath>   | Necesario. Especifica la ubicación del archivo que se crea uniendo una bifurcación de datos y una bifurcación de recursos, o especifica la ubicación del archivo cuyo tipo o creador se está cambiando. El archivo debe estar en el servidor especificado. |
|             /?             |                                                                                               Muestra la ayuda en el símbolo del sistema.                                                                                               |

### <a name="remarks"></a>Observaciones
- Si la información proporcionada contiene espacios o caracteres especiales, use comillas alrededor del texto (por ejemplo, * * * * nombre del<em>equipo</em>* * * *).

### <a name="examples"></a>Ejemplos
Para crear el archivo treeapp en el volumen accesible para Macintosh D:\Release, mediante la bifurcación de recursos C:\Cross\Mac\Appcode, y para que este nuevo archivo aparezca en los clientes de Macintosh como una aplicación (las aplicaciones Macintosh usan el tipo APPL) con el creador (firma) establecido en MAGNOLIA, escriba:
```
macfile forkize /resourcefork:c:\cross\mac\appcode /type:APPL /creator:MAGNOLIA /targetfile:D:\Release\treeapp
```
Para cambiar el creador del archivo a Microsoft Word 5,1, para el archivo WOrd. txt en el directorio D:\Word documents\Group files, en \\el servidor \SERverA, escriba:
```
macfile forkize /server:\\servera /creator:MSWD /type:TEXT /targetfile:d:\Word documents\Group files\Word.txt
```

## <a name="to-change-the-logon-message-and-limit-sessions"></a><a name=BKMK_LogonLimit></a>Para cambiar el mensaje de inicio de sesión y las sesiones de límite
### <a name="syntax"></a>Sintaxis
```
macfile server [/server:\\<computerName>] [/maxsessions:{Number | unlimited}] [/loginmessage:<Message>]
```

#### <a name="parameters"></a>Parámetros

|               Parámetro                |                                                                                                                                                                           Descripción                                                                                                                                                                            |
|----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /Server\\\\<computerName>       |                                                                                                                        Especifica el servidor en el que se van a cambiar los parámetros. Si se omite, la operación se realiza en el equipo local.                                                                                                                         |
| /maxsessions: {Number &#124; Unlimited} |                                                                                         Especifica el número máximo de usuarios que pueden usar simultáneamente servidores de archivos e impresión para Macintosh. Si se omite, la configuración de **maxsessions** para el servidor permanece sin cambios.                                                                                         |
|        /loginmessage<Message>         | cambia el mensaje que los usuarios de Macintosh ven al iniciar sesión en el servidor de archivos para Macintosh. El número máximo de caracteres para el mensaje de inicio de sesión es 199. Si se omite, el mensaje **loginmessage** para el servidor permanece inalterado. Para quitar un mensaje de inicio de sesión existente, incluya el parámetro **/loginmessage** , pero deje la variable de *mensaje* en blanco. |
|                   /?                   |                                                                                                                                                               Muestra la ayuda en el símbolo del sistema.                                                                                                                                                               |

### <a name="remarks"></a>Observaciones
- Si la información proporcionada contiene espacios o caracteres especiales, use comillas alrededor del texto (por ejemplo, * * * * nombre del<em>equipo</em>* * * *).

### <a name="examples"></a>Ejemplos
Para cambiar el número de sesiones del servidor de impresión y de archivos para Macintosh que se permiten en el servidor local de la configuración actual a cinco sesiones, y para agregar el mensaje de inicio de sesión al cerrar la sesión del servidor para Macintosh cuando haya terminado, escriba:
```
macfile server /maxsessions:5 /loginmessage:Log off from Server for Macintosh when you are finished.
```

## <a name="to-add-change-or-remove-macintosh-accessible-volumes"></a><a name=BKMK_addvol></a>Para agregar, cambiar o quitar volúmenes accesibles desde Macintosh
### <a name="syntax"></a>Sintaxis
```
macfile volume {/add|/set} [/server:\\<computerName>] /name:<volumeName>/path:<directory>[/readonly:{true | false}] [/guestsallowed:{true | false}] [/password:<Password>] [/maxusers:{<Number>>|unlimited}]
macfile volume /remove[/server:\\<computerName>] /name:<volumeName>
```

#### <a name="parameters"></a>Parámetros

|              Parámetro               |                                                                                                                                                                       Descripción                                                                                                                                                                        |
|--------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          {/Add &#124;/set}          |                                                                                                                      Obligatorio cuando se agrega o cambia un volumen accesible desde Macintosh. agrega o cambia el volumen especificado.                                                                                                                       |
|      /Server\\\\<computerName>      |                                                                                                             Especifica el servidor en el que se va a agregar, cambiar o quitar un volumen. Si se omite, la operación se realiza en el equipo local.                                                                                                              |
|          /Name<volumeName>          |                                                                                                                                          Necesario. Especifica el nombre del volumen que se va a agregar, cambiar o quitar.                                                                                                                                           |
|          /path<directory>           |                                                                                                                Obligatorio y válido solo cuando se agrega un volumen. Especifica la ruta de acceso al directorio raíz del volumen que se va a agregar.                                                                                                                 |
|    /ReadOnly: {true &#124; false}     | Especifica si los usuarios pueden cambiar los archivos del volumen. Escriba true para especificar que los usuarios no pueden cambiar los archivos del volumen. Escriba false para especificar que los usuarios pueden cambiar los archivos del volumen. Si se omite al agregar un volumen, se permiten los cambios en los archivos. Si se omite al cambiar un volumen, el valor **ReadOnly** del volumen permanece inalterado. |
|  /guestsallowed: {true &#124; false}  |      Especifica si los usuarios que inician sesión como invitados pueden usar el volumen. Escriba true para especificar que los invitados pueden usar el volumen. Escriba false para especificar que los invitados no pueden usar el volumen. Si se omite al agregar un volumen, los invitados pueden usar el volumen. Si se omite al cambiar un volumen, el valor de **guestsallowed** para el volumen permanece inalterado.       |
|         /Password<Password>         |                                                                               Especifica una contraseña que será necesaria para obtener acceso al volumen. Si se omite al agregar un volumen, no se crea ninguna contraseña. Si se omite al cambiar un volumen, la contraseña permanece inalterada.                                                                               |
| /maxusers: {<Number>>&#124;Unlimited} |                                                 Especifica el número máximo de usuarios que pueden utilizar simultáneamente los archivos en el volumen. Si se omite al agregar un volumen, un número ilimitado de usuarios puede usar el volumen. Si se omite al cambiar un volumen, el valor de **maxusers** permanece inalterado.                                                 |
|               /remove                |                                                                                                                                Obligatorio cuando se quita un volumen accesible desde Macintosh. quita el volumen especificado.                                                                                                                                |
|                  /?                  |                                                                                                                                                           Muestra la ayuda en el símbolo del sistema.                                                                                                                                                           |

### <a name="remarks"></a>Observaciones
- Si la información proporcionada contiene espacios o caracteres especiales, use comillas alrededor del texto (por ejemplo, * * * * nombre del<em>equipo</em>* * * *).

### <a name="examples"></a>Ejemplos
Para crear un volumen llamado estadísticas de marketing de EE. UU. en el servidor local, use el directorio de estadísticas de la unidad E y para especificar que los invitados no pueden tener acceso al volumen, escriba:
```
macfile volume /add /name:US Marketing Statistics /guestsallowed:false /path:e:\Stats
```
Para cambiar el volumen creado anteriormente para que sea de solo lectura y requerir una contraseña, y para establecer el número máximo de usuarios en cinco, escriba:
```
macfile volume /set /name:US Marketing Statistics /readonly:true /password:saturn /maxusers:5
```
Para agregar un volumen denominado diseño de paisajes, en el \\servidor \Magnolia, use el directorio árboles de la unidad e y para especificar que los invitados pueden tener acceso al volumen, escriba:
```
macfile volume /add /server:\\Magnolia /name:Landscape Design /path:e:\trees
```
Para quitar el volumen denominado informes de ventas en el servidor local, escriba:
```
macfile volume /remove /name:Sales Reports
```

## <a name="additional-references"></a>Referencias adicionales
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
