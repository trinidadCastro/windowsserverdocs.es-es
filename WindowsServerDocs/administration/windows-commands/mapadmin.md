---
title: mapadmin
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b17332c7-8622-4223-9c43-2fb9cf4d992d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 19975f6f4e0e49cf55e4e80f6566f8596d6d33d8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724022"
---
# <a name="mapadmin"></a>mapadmin



Puede usar **Mapadmin** para administrar asignación de nombres de usuario para los servicios de Microsoft para Network File System.

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
La utilidad de línea de comandos **mapadmin** administra asignación de nombres de usuario en el equipo local o remoto en el que se ejecutan los servicios de Microsoft para Network File System. Si ha iniciado sesión con una cuenta que no tiene credenciales administrativas, puede especificar un nombre de usuario y una contraseña de una cuenta que sí los tenga.

Además de los argumentos de comando específicos, **mapadmin** acepta los siguientes argumentos y opciones:

&lt;equipo&gt; especifica el equipo remoto que ejecuta el servicio asignación de nombres de usuario que desea administrar. Puede especificar el equipo con un nombre de servicio de nombres Internet de Windows (WINS) o un nombre de sistema de nombres de dominio (DNS), o bien mediante la dirección del Protocolo de Internet (IP).

-u &lt;usuario&gt; especifica el nombre de usuario del usuario cuyas credenciales se van a utilizar. Es posible que sea necesario agregar el nombre de dominio al nombre de usuario con el formato <em>dominio</em>**\\**<em>nombre de usuario</em>.

-p &lt;password&gt; especifica la contraseña del usuario. Si se especifica la opción **-u** pero se omite la opción **-p** , se le solicitará la contraseña del usuario.
La acción específica que realiza **mapadmin** depende del argumento de comando que especifique:

### <a name="parameters"></a>Parámetros
### <a name="start"></a>start
inicia el servicio Asignación de nombres de usuario.

### <a name="stop"></a>stop
Detiene el servicio Asignación de nombres de usuario.

### <a name="config"></a>config
Especifica la configuración general para Asignación de nombres de usuario. Las siguientes opciones están disponibles con este argumento de comando **:- &lt;r&gt;dddd&lt;:&gt;HH&lt;:&gt; mm** -especifica el intervalo de actualización para la actualización de las bases de datos de Windows y NIS en días, horas y minutos. El intervalo mínimo es de 5 minutos.
**-i {yes | no}** : activa la asignación simple en (**sí**) o desactivada (**no**). De forma predeterminada, la asignación simple está activada.
**Agregar** : crea una nueva asignación para un usuario o grupo. Las siguientes opciones están disponibles con este argumento de comando:

|Opción|Definición|
|-----|-------|
|-Wu &lt;nombre&gt;|Especifica el nombre del usuario de Windows para el que se crea una nueva asignación.|
|-nombre &lt;UU&gt;|Especifica el nombre del usuario de UNIX para el que se crea una nueva asignación.|
|-Grupo &lt;WG&gt;|Especifica el nombre del grupo de Windows para el que se crea una nueva asignación.|
|-Grupo &lt;UG&gt;|Especifica el nombre del grupo de UNIX para el que se crea una nueva asignación.|
|-setprimary|Especifica que la nueva asignación es la asignación principal.|

**setprimary** : especifica qué asignación es la asignación principal para un usuario o grupo UNIX con varias asignaciones. Las siguientes opciones están disponibles con este argumento de comando:

|Opción|Definición|
|-----|-------|
|-Wu &lt;nombre&gt;|Especifica el usuario de Windows de la asignación principal. Si existe más de una asignación para el usuario, use la opción **-UU** para especificar la asignación principal.|
|-nombre &lt;UU&gt;|Especifica el usuario de UNIX de la asignación principal.|
|-Grupo &lt;WG&gt;|Especifica el grupo de Windows de la asignación principal. Si existe más de una asignación para el grupo, use la opción **-UG** para especificar la asignación principal.|
|-Grupo &lt;UG&gt;|Especifica el grupo UNIX de la asignación principal.|

**eliminar** : quita la asignación de un usuario o grupo. Las siguientes opciones están disponibles para este argumento de comando:

|Opción|Definición|
|-----|-------|
|-usuario &lt;de Wu&gt;|El usuario de Windows para el que se eliminará la asignación, especificado como &lt; *nombre&gt;de usuario de WindowsDomain&gt;\\&lt;*. Debe especificar las opciones **-Wu** o **-UU** , o ambas. Si especifica ambas opciones, se eliminará la asignación concreta identificada por las dos opciones. Si solo especifica la opción **-Wu** , se eliminarán todas las asignaciones para el usuario especificado.|
|-Grupo &lt;WG&gt;|El grupo de Windows para el que se eliminará la asignación, &lt;especificado&gt;\\&lt;como&gt;WindowsDomain nombre_grupo. Debe especificar la opción **-WG** o **-UG** , o ambas. Si especifica ambas opciones, se eliminará la asignación concreta identificada por las dos opciones. Si solo especifica la opción **-WG** , se eliminarán todas las asignaciones para el grupo especificado.|
|-usuario &lt;UU&gt;|El usuario de UNIX para el que se eliminará la asignación, &lt;especificado como&gt;nombre de usuario. Debe especificar las opciones **-Wu** o **-UU** , o ambas. Si especifica ambas opciones, se eliminará la asignación concreta identificada por las dos opciones. Si solo especifica la opción **-UU** , se eliminarán todas las asignaciones para el usuario especificado.|
|-Grupo &lt;UG&gt;|El grupo UNIX para el que se eliminará la asignación, especificado &lt;como&gt;nombre_grupo. Debe especificar la opción **-WG** o **-UG** , o ambas. Si especifica ambas opciones, se eliminará la asignación concreta identificada por las dos opciones. Si solo especifica la opción **-UG** , se eliminarán todas las asignaciones para el grupo especificado.|

**lista** : muestra información acerca de las asignaciones de usuario y grupo. Las siguientes opciones están disponibles con este argumento de comando:

|Opción|Definición|
|-----|-------|
|-todo|muestra las asignaciones simples y avanzadas para usuarios y grupos.|
|-simple|enumera todos los usuarios y grupos asignados simples.|
|-avanzado|enumera todos los usuarios y grupos asignados avanzada. Las asignaciones se enumeran en el orden en el que se evalúan. Las asignaciones principales, marcadas con un asterisco (*), se enumeran primero, seguidas de las asignaciones secundarias, que se marcan con un acento circunflejo (^).|
|-Wu &lt;nombre&gt;|muestra la asignación de un usuario de Windows especificado.|
|-Grupo &lt;WG&gt;|muestra la asignación de un grupo de Windows.|
|-nombre &lt;UU&gt;|muestra la asignación de un usuario de UNIX.|
|-Grupo &lt;UG&gt;|muestra la asignación de un grupo de UNIX.|

**copia de seguridad** : guarda los datos de configuración y asignación de asignación de nombres de usuario &lt;en&gt;el archivo especificado por filename.
**restore** : reemplaza los datos de configuración y asignación con los datos del archivo &lt;(&gt;especificado por el nombre de archivo) que se creó mediante el argumento de comando de **copia de seguridad** .
**adddomainmap** : agrega un mapa sencillo entre un dominio de Windows y un dominio NIS o archivos de contraseña y grupo. Las siguientes opciones están disponibles para este argumento de comando:

|Opción|Definición|
|-----|-------|
|-d &lt;WindowsDomain&gt;|Especifica el dominio de Windows que se va a asignar.|
|-y &lt;dominionis&gt;|Especifica el dominio NIS que se va a asignar. &lt;br/&gt;&lt;br/&gt;**-n** &lt;servidornis&gt; especifica el servidor NIS para el dominio NIS especificado con la opción **-y** .|
|-f &lt;ruta de acceso&gt;|Especifica la ruta de acceso completa del directorio que contiene los archivos de contraseña y de grupo que se van a asignar. Los archivos deben encontrarse en el equipo que se está administrando y no puede usar **mapadmin** para administrar un equipo remoto con el fin de configurar asignaciones basadas en archivos de grupos y contraseñas.|

**removedomainmap** : quita una asignación simple entre un dominio de Windows y un dominio NIS. Las siguientes opciones y argumentos están disponibles para este argumento de comando:

|Opción|Definición|
|-----|-------|
|-d &lt;WindowsDomain&gt;|Especifica el dominio de Windows del mapa que se va a quitar.|
|-y &lt;dominionis&gt;|Especifica el dominio NIS del mapa que se va a quitar.|
|-todo|Especifica que se van a quitar todas las asignaciones simples entre dominios de Windows y NIS. También se quitarán todas las asignaciones simples entre un dominio de Windows y los archivos de grupo y contraseña.|

**listdomainmaps** : enumera los dominios de Windows que están asignados a los dominios NIS o a los archivos de contraseña y grupo.

## <a name="notes"></a>Notas
-   Si no se especifica un argumento de comando, **mapadmin** muestra la configuración actual de asignación de nombres de usuario.
-   para todas las opciones que especifican un nombre de usuario o grupo, se pueden usar los siguientes formatos:
-   en el caso de los usuarios de &lt;Windows&gt;\\&lt;, use&gt;el \\ \\ &lt;formato&gt;\\&lt;dominio nombre&gt;de \\ &lt;usuario&gt;\\&lt;, nombre&gt;de usuario &lt;de&gt;\\&lt;equipo, nombre de usuario de equipo o nombre de usuario de equipo.&gt;
-   en el caso de los grupos de &lt;Windows&gt;\\&lt;&lt;,&gt;&gt;use \\ \\ &lt;el&gt;\\&lt;&lt;formato&gt;&gt;dominio \\ &lt;nombre_grupo&gt;\\, equipo nombre_grupo &lt;,&gt;equipo&lt;&lt;nombre_grupo&lt;o equipo nombre_grupo\\&lt;&gt;&gt;&gt;&gt;
-   para los usuarios de UNIX, use &lt;el&gt;\\&lt;formato dominionis&gt;nombre &lt;de usuario&gt;@&lt;,&gt;nombre de &lt;usuario&gt;@PCNFSdominionis, nombre\\&lt;de usuario o nombre de usuario PCNFS.&gt;
-   en el caso de los grupos de &lt;UNIX&gt;\\&lt;,&gt;use &lt;el&gt;@&lt;formato&gt;dominionis &lt;GroupName&gt;@PCNFS, GroupName dominionis\\&lt;, GroupName o PCNFS GroupName.&gt;

## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
