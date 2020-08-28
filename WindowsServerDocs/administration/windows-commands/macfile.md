---
title: macfile
description: Artículo de referencia del comando MacFile, que administra el servidor de archivos para servidores, volúmenes, directorios y archivos de Macintosh.
ms.topic: reference
ms.assetid: e2ce586c-b316-41d3-90f8-4be0d074cc0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 06095d99c6cfbdc51fd28f51f9bc06f08d959edf
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89023699"
---
# <a name="macfile"></a>macfile

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Administra el servidor de archivos para servidores, volúmenes, directorios y archivos de Macintosh. Puede automatizar las tareas administrativas mediante la inclusión de una serie de comandos en archivos por lotes y su inicio manual o en momentos predeterminados.

## <a name="modify-directories-in-macintosh-accessible-volumes"></a>Modificar directorios en volúmenes accesibles desde Macintosh

Para cambiar el nombre del directorio, la ubicación, el propietario, el grupo y los permisos de los volúmenes accesibles a Macintosh.

### <a name="syntax"></a>Sintaxis

```
macfile directory[/server:\\<computername>] /path:<directory> [/owner:<ownername>] [/group:<groupname>] [/permissions:<permissions>]
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /server:`\\<computername>` | Especifica el servidor en el que se va a cambiar un directorio. Si se omite, la operación se realiza en el equipo local. |
| /path`<directory>` | Especifica la ruta de acceso al directorio que desea cambiar. Este parámetro es obligatorio. **Nota:** El directorio debe existir, el uso del **directorio MacFile** no creará directorios. |
| /Owner`<ownername>` | Cambia el propietario del directorio. Si se omite, el nombre del propietario no cambiará. |
| /objetos`<groupname>` | Especifica o cambia el grupo principal de Macintosh que está asociado con el directorio. Si se omite, el grupo primario permanece inalterado. |
| los`<permissions>` | Establece permisos en el directorio para el propietario, el grupo principal y el mundo (todos). Debe ser un número de 11 dígitos, donde el número 1 concede el permiso y 0 revoca el permiso (por ejemplo, 11111011000). Si se omite este parámetro, los permisos permanecen sin cambios. |
| /? | Muestra la ayuda en el símbolo del sistema. |

##### <a name="position-of-permissions-digit"></a>Posición del dígito de permisos

La posición del dígito de permisos determina qué permiso se establece, incluido:

| Posición | Establece el permiso |
| -------- | --------------- |
| First | OwnerSeeFiles |
| Segundo | OwnerSeeFolders |
| Tercero | OwnerMakechanges |
| Cuarto | GroupSeeFiles |
| Quinto | GroupSeeFolders |
| Sexto | GroupMakechanges |
| Séptimo | WorldSeeFiles |
| Octava | WorldSeeFolders |
| Novena | WorldMakechanges |
| Décima | No se puede cambiar el nombre del directorio, moverlo o eliminarlo. |
| 11 | Los cambios se aplican al directorio actual y a todos los subdirectorios. |

##### <a name="remarks"></a>Observaciones

- Si la información proporcionada contiene espacios o caracteres especiales, use comillas alrededor del texto (por ejemplo, " `<computer name>` ").

- Use el **directorio MacFile** para convertir un directorio existente en un volumen accesible para Macintosh a disposición de los usuarios de Macintosh. El comando **MacFile Directory** no crea directorios.

- Use el comando administrador de archivos, el símbolo del sistema o la **nueva carpeta Macintosh** para crear un directorio en un volumen accesible desde Macintosh antes de usar el comando **MacFile Directory** .

#### <a name="examples"></a>Ejemplos

Para asignar *los permisos ver archivos*, *Ver carpetas*y *realizar cambios* en el propietario, para establecer *Ver* permisos de carpeta para todos los demás usuarios y evitar que el directorio se cambie de nombre, se mueva o se elimine, escriba:

```
macfile directory /path:e:\statistics\may sales /permissions:11111011000
```

En el caso de que el subdirectorio sea *ventas*, se encuentra en las *estadísticas*de volumen accesible desde Macintosh, en el E:\ unidad del servidor local.

## <a name="join-a-macintosh-files-data-and-resource-forks"></a>Unir los datos y las bifurcaciones de recursos de un archivo de Macintosh

Para especificar el servidor en el que se van a unir los archivos, que crearon el archivo, el tipo de archivo, donde se encuentra la bifurcación de datos, donde se encuentra la bifurcación de recursos y dónde debe ubicarse el archivo de salida.

### <a name="syntax"></a>Sintaxis

```
macfile forkize[/server:\\<computername>] [/creator:<creatorname>] [/type:<typename>]  [/datafork:<filepath>] [/resourcefork:<filepath>] /targetfile:<filepath>
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /server:`\\<computername>` | Especifica el servidor en el que se van a unir los archivos. Si se omite, la operación se realiza en el equipo local. |
| /Creator`<creatorname>` | Especifica el creador del archivo. El buscador de Macintosh usa la opción de línea de comandos **/Creator** para determinar la aplicación que creó el archivo. |
| /Type`<typename>` | Especifica el tipo de archivo. El buscador de Macintosh usa la opción de línea de comandos **/Type** para determinar el tipo de archivo dentro de la aplicación que creó el archivo. |
| /datafork:`<filepath>` | Especifica la ubicación de la bifurcación de datos que se va a combinar. Puede especificar una ruta de acceso remota. |
| /resourcefork:`<filepath>` | Especifica la ubicación de la bifurcación de recursos que se va a combinar. Puede especificar una ruta de acceso remota. |
| /targetfile`<filepath>` | Especifica la ubicación del archivo que se crea al unirse a una bifurcación de datos y una bifurcación de recursos, o especifica la ubicación del archivo cuyo tipo o creador se está cambiando. El archivo debe estar en el servidor especificado. Este parámetro es obligatorio. |
| /? | Muestra la ayuda en el símbolo del sistema. |

##### <a name="remarks"></a>Observaciones

- Si la información proporcionada contiene espacios o caracteres especiales, use comillas alrededor del texto (por ejemplo, " `<computer name>` ").

#### <a name="examples"></a>Ejemplos

Para crear el archivo *tree_app* en el volumen accesible para Macintosh *D:\Release*, mediante la bifurcación de recursos *C:\Cross\Mac\Appcode*, y para que este nuevo archivo aparezca en los clientes de Macintosh como una aplicación (las aplicaciones Macintosh usan el tipo *appl*) con el creador (firma) establecido en *Magnolia*, escriba:

```
macfile forkize /resourcefork:c:\cross\mac\appcode /type:APPL /creator:MAGNOLIA /targetfile:D:\Release\tree_app
```

Para cambiar el creador del archivo a *Microsoft Word 5,1*, para el archivo *Word.txt* en el directorio *D:\Word documents\Group files*, en el servidor * \\ Servera*, escriba:

```
macfile forkize /server:\\ServerA /creator:MSWD /type:TEXT /targetfile:d:\Word documents\Group files\Word.txt
```

## <a name="change-the-sign-in-message-and-limit-sessions"></a>Cambiar el mensaje de inicio de sesión y limitar las sesiones

Cambiar el mensaje de inicio de sesión que aparece cuando un usuario inicia sesión en el servidor de archivos para Macintosh y limitar el número de usuarios que pueden usar simultáneamente servidores de archivos e impresión para Macintosh.

### <a name="syntax"></a>Sintaxis

```
macfile server [/server:\\<computername>] [/maxsessions:{number | unlimited}] [/loginmessage:<message>]
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- |------------ |
| /server:`\\<computername>` | Especifica el servidor en el que se van a cambiar los parámetros. Si se omite, la operación se realiza en el equipo local. |
| maxsessions`{number | unlimited}` | Especifica el número máximo de usuarios que pueden usar simultáneamente servidores de archivos e impresión para Macintosh. Si se omite, la configuración de **maxsessions** para el servidor permanece sin cambios. |
| /loginmessage`<message>` | Cambia el mensaje que los usuarios de Macintosh ven al iniciar sesión en el servidor de archivos para Macintosh. El número máximo de caracteres para el mensaje de inicio de sesión es 199. Si se omite, el mensaje **loginmessage** para el servidor permanece inalterado. Para quitar un mensaje de inicio de sesión existente, incluya el parámetro **/loginmessage** , pero deje la variable de *mensaje* en blanco. |
| /? | Muestra la ayuda en el símbolo del sistema. |

##### <a name="remarks"></a>Observaciones

- Si la información proporcionada contiene espacios o caracteres especiales, use comillas alrededor del texto (por ejemplo, " `<computer name>` ").

#### <a name="examples"></a>Ejemplos

Para cambiar el número de sesiones permitidas del servidor de impresión y de archivos para Macintosh en el servidor local a cinco sesiones, y para agregar el mensaje de inicio de sesión "cerrar sesión en el servidor para Macintosh cuando haya terminado", escriba:

```
macfile server /maxsessions:5 /loginmessage:Sign off from Server for Macintosh when you are finished
```

## <a name="add-change-or-remove-macintosh-accessible-volumes"></a>Agregar, cambiar o quitar volúmenes accesibles desde Macintosh

Para agregar, cambiar o quitar un volumen accesible desde Macintosh.

### <a name="syntax"></a>Sintaxis

```
macfile volume {/add|/set} [/server:\\<computername>] /name:<volumename>/path:<directory>[/readonly:{true | false}] [/guestsallowed:{true | false}] [/password:<password>] [/maxusers:{<number>>|unlimited}]
macfile volume /remove[/server:\\<computername>] /name:<volumename>
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `{/add | /set}` | Obligatorio al agregar o cambiar un volumen accesible desde Macintosh. Agrega o cambia el volumen especificado. |
| /server:`\\<computername>` | Especifica el servidor en el que se va a agregar, cambiar o quitar un volumen. Si se omite, la operación se realiza en el equipo local. |
| /Name`<volumename>` | Necesario. Especifica el nombre del volumen que se va a agregar, cambiar o quitar. |
| /path`<directory>` | Obligatorio y válido solo cuando se agrega un volumen. Especifica la ruta de acceso al directorio raíz del volumen que se va a agregar. |
| /ReadOnly`{true | false}` | Especifica si los usuarios pueden cambiar los archivos del volumen. Use **true** para especificar que los usuarios no pueden cambiar los archivos del volumen. Use **false** para especificar que los usuarios pueden cambiar los archivos del volumen. Si se omite al agregar un volumen, se permiten los cambios en los archivos. Si se omite al cambiar un volumen, el valor **ReadOnly** del volumen permanece inalterado. |
| /guestsallowed`{true | false}` | Especifica si los usuarios que inician sesión como invitados pueden usar el volumen. Use **true** para especificar que los invitados pueden usar el volumen. Use **false** para especificar que los invitados no pueden usar el volumen. Si se omite al agregar un volumen, los invitados pueden usar el volumen. Si se omite al cambiar un volumen, el valor de **guestsallowed** para el volumen permanece inalterado. |
| /Password`<password>` | Especifica una contraseña que será necesaria para obtener acceso al volumen. Si se omite al agregar un volumen, no se crea ninguna contraseña. Si se omite al cambiar un volumen, la contraseña permanece inalterada. |
| /maxusers:`{<number>> | unlimited}` | Especifica el número máximo de usuarios que pueden utilizar simultáneamente los archivos en el volumen. Si se omite al agregar un volumen, un número ilimitado de usuarios puede usar el volumen. Si se omite al cambiar un volumen, el valor de **maxusers** permanece inalterado.                                                 |
| /remove | Obligatorio cuando se quita un volumen accesible a Macintosh. quita el volumen especificado. |
| /? | Muestra la ayuda en el símbolo del sistema. |

##### <a name="remarks"></a>Observaciones

- Si la información proporcionada contiene espacios o caracteres especiales, use comillas alrededor del texto (por ejemplo, " `<computer name>` ").

#### <a name="examples"></a>Ejemplos

Para crear un volumen llamado *estadísticas de marketing de EE. UU.* en el servidor local, use el directorio de *estadísticas* de la unidad e y para especificar que los invitados no pueden tener acceso al volumen, escriba:

```
macfile volume /add /name:US Marketing Statistics /guestsallowed:false /path:e:\Stats
```

Para cambiar el volumen creado anteriormente para que sea de solo lectura, para que requiera una contraseña y para establecer el número máximo de usuarios en cinco, escriba:

```
macfile volume /set /name:US Marketing Statistics /readonly:true /password:saturn /maxusers:5
```

Para agregar un volumen denominado *diseño de paisajes*, en el servidor * \\ Magnolia*, use el directorio *árboles* de la unidad e y para especificar que los invitados pueden tener acceso al volumen, escriba:

```
macfile volume /add /server:\\Magnolia /name:Landscape Design /path:e:\trees
```

Para quitar el volumen denominado *informes de ventas* en el servidor local, escriba:

```
macfile volume /remove /name:Sales Reports
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
