---
title: mapadmin
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b17332c7-8622-4223-9c43-2fb9cf4d992d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 007256fffde11899d930c9197cade6d3bf9be42c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868786"
---
# <a name="mapadmin"></a>mapadmin



Puede usar **Mapadmin** para administrar la asignación de nombres de usuario de Microsoft Services para Network File System.

## <a name="syntax"></a>Sintaxis
```
mapadmin [<computer>] [-u <user> [-p <password>]]
mapadmin [<computer>] [-u <user> [-p <password>]] {start | stop}
mapadmin [<computer>] [-u <user> [-p <password>]] config <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] add -wu <WindowsUser> -uu <UNIXUser> [-setprimary]
mapadmin [<computer>] [-u <user> [-p <password>]] add -wg <WindowsGroup> -ug <UNIXGroup> [-setprimary]
mapadmin [<computer>] [-u <user> [-p <password>]] setprimary -wu <WindowsUser> [-uu <UNIXUser>]
mapadmin [<computer>] [-u <user> [-p <password>]] setprimary -wg <WindowsGroup> [-ug <UNIXGroup>]
mapadmin [<computer>] [-u <user> [-p <password>]] delete <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] list <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] backup <filename> 
mapadmin [<computer>] [-u <user> [-p <password>]] restore <filename>
mapadmin [<computer>] [-u <user> [-p <password>]] adddomainmap -d <WindowsDomain> {-y <<NISdomain>> | -f <path>}
mapadmin [<computer>] [-u <user> [-p <password>]] removedomainmap -d <WindowsDomain> -y <<NISdomain>>
mapadmin [<computer>] [-u <user> [-p <password>]] removedomainmap -all
mapadmin [<computer>] [-u <user> [-p <password>]] listdomainmaps
```

## <a name="description"></a>Descripción
El **mapadmin** utilidad de línea de comandos administra la asignación de nombres de usuario en el equipo local o remoto que ejecuta Services de Microsoft para Network File System. Si ha iniciado sesión con una cuenta que no tiene credenciales administrativas, puede especificar un nombre de usuario y contraseña de una cuenta que sí.

Además de los argumentos de comando específico, **mapadmin** acepta los siguientes argumentos y opciones:

&lt;equipo&gt; especifica el equipo remoto que ejecuta el servicio de asignación de nombres de usuario que desea administrar. Puede especificar el equipo mediante un nombre de servicio de nombres Internet de Windows (WINS) o un nombre de sistema de nombres de dominio (DNS) o la dirección de protocolo de Internet (IP).

-u &lt;usuario&gt; especifica el nombre de usuario del usuario cuyas credenciales se van a usarse. Podría ser necesario agregar el nombre de dominio al nombre de usuario en el formulario *dominio***\\***nombre de usuario*.

-p &lt;contraseña&gt; especifica la contraseña del usuario. Si especifica la **-u** opción pero omita el **-p** opción, se le solicitará la contraseña del usuario.
La acción concreta que **mapadmin** realiza depende del argumento de comando que especifique:

## <a name="parameters"></a>Parámetros
### <a name="start"></a>start
inicia el servicio de asignación de nombres de usuario.

### <a name="stop"></a>stop
Detiene el servicio de asignación de nombres de usuario.

### <a name="config"></a>config
Especifica las opciones generales para la asignación de nombres de usuario. Las siguientes opciones están disponibles con este argumento de comando: **- r &lt;dddd&gt;:&lt;hh&gt;:&lt;mm&gt;**  : especifica el intervalo de actualización para actualización de las bases de datos de Windows y NIS en días, horas y minutos. El intervalo mínimo es 5 minutos.
**-i {sí | no}** -enciende el asignación simple (**Sí**) u off (**ningún**). De forma predeterminada, la asignación simple está activado.
**agregar** -crea una nueva asignación de un usuario o grupo. Las siguientes opciones están disponibles con este argumento de comando:

|Opción|Definición|
|-----|-------|
|-wu &lt;name&gt;|Especifica el nombre del usuario de Windows para el que se está creando una nueva asignación.|
|-uu &lt;name&gt;|Especifica el nombre del usuario de UNIX para el que se está creando una nueva asignación.|
|-wg &lt;grupo&gt;|Especifica el nombre del grupo de Windows para el que se está creando una nueva asignación.|
|-ug &lt;grupo&gt;|Especifica el nombre del grupo de UNIX para el que se está creando una nueva asignación.|
|-setprimary|Especifica que la nueva asignación es la asignación principal.|

**setprimary** : Especifica que la asignación es la asignación principal para un grupo o usuario de UNIX con varias asignaciones. Las siguientes opciones están disponibles con este argumento de comando:

|Opción|Definición|
|-----|-------|
|-wu &lt;name&gt;|Especifica el usuario de Windows de la asignación principal. Si existe más de una asignación para el usuario, utilice el **- uu** opción para especificar la asignación principal.|
|-uu &lt;name&gt;|Especifica el usuario de UNIX de la asignación principal.|
|-wg &lt;grupo&gt;|Especifica el grupo de Windows de la asignación principal. Si existe más de una asignación para el grupo, utilice el **- ug** opción para especificar la asignación principal.|
|-ug &lt;grupo&gt;|Especifica el grupo de UNIX de la asignación principal.|

**eliminar** -quita la asignación de un usuario o grupo. Las siguientes opciones están disponibles para este argumento de comando:

|Opción|Definición|
|-----|-------|
|-wu &lt;user&gt;|El usuario de Windows para el que se eliminarán la asignación especificada como &lt; *WindowsDomain&gt;\\&lt;nombre de usuario&gt;*. Debe especificar el **- wu** o **- uu** , o ambas. Si especifica ambas opciones, se eliminará la asignación concreto identificada por las dos opciones. Si especifica solo el **- wu** opción, todas las asignaciones para el usuario especificado se eliminarán.|
|-wg &lt;grupo&gt;|El grupo de Windows para el que se eliminarán la asignación especificada como &lt;WindowsDomain&gt;\\&lt;groupname&gt;. Debe especificar el **- wg** o **- ug** , o ambas. Si especifica ambas opciones, se eliminará la asignación concreto identificada por las dos opciones. Si especifica solo el **- wg** opción, todas las asignaciones para el grupo especificado se eliminarán.|
|-uu &lt;user&gt;|El usuario de UNIX para los que se eliminarán la asignación especificada como &lt;nombre de usuario&gt;. Debe especificar el **- wu** o **- uu** , o ambas. Si especifica ambas opciones, se eliminará la asignación concreto identificada por las dos opciones. Si especifica solo el **- uu** opción, todas las asignaciones para el usuario especificado se eliminarán.|
|-ug &lt;grupo&gt;|El grupo de UNIX para el que se eliminarán la asignación especificada como &lt;groupname&gt;. Debe especificar el **- wg** o **- ug** , o ambas. Si especifica ambas opciones, se eliminará la asignación concreto identificada por las dos opciones. Si especifica solo el **- ug** opción, todas las asignaciones para el grupo especificado se eliminarán.|

**lista** -muestra información acerca de las asignaciones de usuario y grupo. Las siguientes opciones están disponibles con este argumento de comando:

|Opción|Definición|
|-----|-------|
|-todas|se muestran las asignaciones simples y avanzadas para usuarios y grupos.|
|-simple|Enumera todos los usuarios asignados simple y grupos.|
|-avanzada|Enumera todos los usuarios asignados avanzados y grupos. Mapas aparecen en el orden en que se evalúan. Mapas principales, marcados con un asterisco (*), se muestran en primer lugar, seguida de mapas de secundaria, que se marcan con un acento circunflejo (^).|
|-wu &lt;name&gt;|Enumera la asignación de un usuario de Windows especificado.|
|-wg &lt;grupo&gt;|Enumera la asignación de un grupo de Windows.|
|-uu &lt;name&gt;|Enumera la asignación de un usuario de UNIX.|
|-ug &lt;grupo&gt;|Enumera la asignación de un grupo de UNIX.|

**copia de seguridad** : nombre de usuario guarda la configuración de mapa y asignación de datos en el archivo especificado por &lt;filename&gt;.
**restaurar** -reemplaza los datos de configuración y asignación con datos del archivo (especificada por &lt;filename&gt;) que se creó utilizando el **copia de seguridad** argumento de comando.
**adddomainmap** -agrega una asignación simple entre un dominio de Windows y un archivo de dominio o la contraseña y grupo de NIS. Las siguientes opciones están disponibles para este argumento de comando:

|Opción|Definición|
|-----|-------|
|-d &lt;WindowsDomain&gt;|Especifica el dominio de Windows que se puede asignar.|
|-y &lt;Dominionis&gt;|Especifica el dominio NIS a asignarse. &lt;br /&gt;&lt;br /&gt;**- n** &lt;Servidornis&gt; especifica el servidor NIS para el dominio NIS especificado con el **- y**opción.|
|-f &lt;ruta de acceso&gt;|Especifica la ruta de acceso completa del directorio que contiene los archivos de contraseña y grupo que debe asignarse. Los archivos deben estar ubicados en el equipo administrado y no puede usar **mapadmin** para administrar un equipo remoto para configurar las asignaciones basadas en contraseña y grupo de archivos.|

**removedomainmap** -quita una asignación simple entre un dominio de Windows y un dominio NIS. Las siguientes opciones y argumentos están disponibles para este argumento de comando:

|Opción|Definición|
|-----|-------|
|-d &lt;WindowsDomain&gt;|Especifica el dominio de Windows de la asignación que se va a quitar.|
|-y &lt;Dominionis&gt;|Especifica el dominio NIS del mapa que se va a quitar.|
|-todas|Especifica que todas las asignaciones simples entre dominios de Windows y NIS se va a quitar. Esto también quitará cualquier asignación simple entre un archivo de dominio y la contraseña y grupo de Windows.|

**listdomainmaps** -enumera los dominios de Windows que se asignan a dominios NIS o la contraseña y grupo de archivos.

## <a name="notes"></a>Notas
-   Si no especifica un argumento de comando, **mapadmin** muestra la configuración actual de asignación de nombres de usuario.
-   para todas las opciones que especifican un nombre de grupo o usuario, se pueden usar los siguientes formatos:
-   para los usuarios de Windows, use el formulario &lt;dominio&gt;\\&lt;nombre de usuario&gt;, \\ \\ &lt;equipo&gt;\\&lt;usuario nombre&gt;, \\ &lt;equipo&gt;\\&lt;nombre de usuario&gt;, o &lt;equipo&gt;\\&lt;usuario nombre&gt;
-   para los grupos de Windows, use el formulario &lt;dominio&gt;\\&lt;&lt;groupname&gt;&gt;, \\ \\ &lt;equipo&gt; \\ &lt; &lt;groupname&gt;&gt;, \\ &lt;equipo&gt;\\&lt;&lt;groupname&gt; &gt;, o &lt;equipo&gt;\\&lt;&lt;groupname&gt;&gt;
-   para los usuarios de UNIX, use el formulario &lt;Dominionis&gt;\\&lt;nombre de usuario&gt;, &lt;nombre de usuario&gt;@&lt;Dominionis&gt;, usuario &lt;nombre&gt;@PCNFS, o PCNFS\\&lt;nombre de usuario&gt;
-   para los grupos de UNIX, use el formulario &lt;Dominionis&gt;\\&lt;groupname&gt;, &lt;groupname&gt;@&lt;Dominionis&gt;, &lt;groupname&gt;@PCNFS, o PCNFS\\&lt;groupname&gt;

## <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
