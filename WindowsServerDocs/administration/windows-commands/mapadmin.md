---
title: mapadmin
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b17332c7-8622-4223-9c43-2fb9cf4d992d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5ea60f4d9753ed90c0d13ee48289b011aeafe6b0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839678"
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

&lt;equipo&gt; especifica el equipo remoto que ejecuta el servicio de Asignación de nombres de usuario que desea administrar. Puede especificar el equipo con un nombre de servicio de nombres Internet de Windows (WINS) o un nombre de sistema de nombres de dominio (DNS), o bien mediante la dirección del Protocolo de Internet (IP).

-u &lt;usuario&gt; especifica el nombre de usuario del usuario cuyas credenciales se van a utilizar. Puede que sea necesario agregar el nombre de dominio al nombre de usuario con el formato <em>dominio</em> **\\** <em>nombre de usuario</em>.

-p &lt;contraseña&gt; especifica la contraseña del usuario. Si se especifica la opción **-u** pero se omite la opción **-p** , se le solicitará la contraseña del usuario.
La acción específica que realiza **mapadmin** depende del argumento de comando que especifique:

### <a name="parameters"></a>Parámetros
### <a name="start"></a>inicio
inicia el servicio Asignación de nombres de usuario.

### <a name="stop"></a>stop
Detiene el servicio Asignación de nombres de usuario.

### <a name="config"></a>config
Especifica la configuración general para Asignación de nombres de usuario. Las siguientes opciones están disponibles con este argumento de comando: **-r &lt;dddd&gt;:&lt;hh&gt;:&lt;mm&gt;** -especifica el intervalo de actualización para la actualización de las bases de datos de Windows y NIS en días, horas y minutos. El intervalo mínimo es de 5 minutos.
**-i {yes | no}** : activa la asignación simple en (**sí**) o desactivada (**no**). De forma predeterminada, la asignación simple está activada.
**Agregar** : crea una nueva asignación para un usuario o grupo. Las siguientes opciones están disponibles con este argumento de comando:

|Opción|Definición|
|-----|-------|
|-Wu &lt;nombre&gt;|Especifica el nombre del usuario de Windows para el que se crea una nueva asignación.|
|-UU &lt;nombre&gt;|Especifica el nombre del usuario de UNIX para el que se crea una nueva asignación.|
|-WG &lt;grupo&gt;|Especifica el nombre del grupo de Windows para el que se crea una nueva asignación.|
|-UG &lt;grupo&gt;|Especifica el nombre del grupo de UNIX para el que se crea una nueva asignación.|
|-setprimary|Especifica que la nueva asignación es la asignación principal.|

**setprimary** : especifica qué asignación es la asignación principal para un usuario o grupo UNIX con varias asignaciones. Las siguientes opciones están disponibles con este argumento de comando:

|Opción|Definición|
|-----|-------|
|-Wu &lt;nombre&gt;|Especifica el usuario de Windows de la asignación principal. Si existe más de una asignación para el usuario, use la opción **-UU** para especificar la asignación principal.|
|-UU &lt;nombre&gt;|Especifica el usuario de UNIX de la asignación principal.|
|-WG &lt;grupo&gt;|Especifica el grupo de Windows de la asignación principal. Si existe más de una asignación para el grupo, use la opción **-UG** para especificar la asignación principal.|
|-UG &lt;grupo&gt;|Especifica el grupo UNIX de la asignación principal.|

**eliminar** : quita la asignación de un usuario o grupo. Las siguientes opciones están disponibles para este argumento de comando:

|Opción|Definición|
|-----|-------|
|-Wu &lt;usuario&gt;|Usuario de Windows para el que se eliminará la asignación, especificado como &lt;*WindowsDomain&gt;\\&lt;nombre de usuario&gt;* . Debe especificar las opciones **-Wu** o **-UU** , o ambas. Si especifica ambas opciones, se eliminará la asignación concreta identificada por las dos opciones. Si solo especifica la opción **-Wu** , se eliminarán todas las asignaciones para el usuario especificado.|
|-WG &lt;grupo&gt;|El grupo de Windows para el que se eliminará la asignación, especificado como &lt;WindowsDomain&gt;\\&lt;GroupName&gt;. Debe especificar la opción **-WG** o **-UG** , o ambas. Si especifica ambas opciones, se eliminará la asignación concreta identificada por las dos opciones. Si solo especifica la opción **-WG** , se eliminarán todas las asignaciones para el grupo especificado.|
|-UU &lt;usuario&gt;|El usuario de UNIX para el que se eliminará la asignación, especificado como &lt;nombre de usuario&gt;. Debe especificar las opciones **-Wu** o **-UU** , o ambas. Si especifica ambas opciones, se eliminará la asignación concreta identificada por las dos opciones. Si solo especifica la opción **-UU** , se eliminarán todas las asignaciones para el usuario especificado.|
|-UG &lt;grupo&gt;|El grupo UNIX para el que se eliminará la asignación, especificado como &lt;GroupName&gt;. Debe especificar la opción **-WG** o **-UG** , o ambas. Si especifica ambas opciones, se eliminará la asignación concreta identificada por las dos opciones. Si solo especifica la opción **-UG** , se eliminarán todas las asignaciones para el grupo especificado.|

**lista** : muestra información acerca de las asignaciones de usuario y grupo. Las siguientes opciones están disponibles con este argumento de comando:

|Opción|Definición|
|-----|-------|
|-todo|muestra las asignaciones simples y avanzadas para usuarios y grupos.|
|-simple|enumera todos los usuarios y grupos asignados simples.|
|-avanzado|enumera todos los usuarios y grupos asignados avanzada. Las asignaciones se enumeran en el orden en el que se evalúan. Las asignaciones principales, marcadas con un asterisco (*), se enumeran primero, seguidas de las asignaciones secundarias, que se marcan con un acento circunflejo (^).|
|-Wu &lt;nombre&gt;|muestra la asignación de un usuario de Windows especificado.|
|-WG &lt;grupo&gt;|muestra la asignación de un grupo de Windows.|
|-UU &lt;nombre&gt;|muestra la asignación de un usuario de UNIX.|
|-UG &lt;grupo&gt;|muestra la asignación de un grupo de UNIX.|

**copia de seguridad** : guarda los datos de configuración y asignación de asignación de nombres de usuario en el archivo especificado por &lt;nombre de archivo&gt;.
**restore** : reemplaza los datos de configuración y asignación con datos del archivo (especificado por &lt;nombre de archivo&gt;) que se creó con el argumento del comando de **copia de seguridad** .
**adddomainmap** : agrega un mapa sencillo entre un dominio de Windows y un dominio NIS o archivos de contraseña y grupo. Las siguientes opciones están disponibles para este argumento de comando:

|Opción|Definición|
|-----|-------|
|-d &lt;WindowsDomain&gt;|Especifica el dominio de Windows que se va a asignar.|
|-y &lt;Dominionis&gt;|Especifica el dominio NIS que se va a asignar.&lt;br/&gt;&lt;br/&gt; **-n** &lt;Servidornis&gt; especifica el servidor NIS para el dominio NIS especificado con la opción **-y** .|
|-f &lt;ruta de acceso&gt;|Especifica la ruta de acceso completa del directorio que contiene los archivos de contraseña y de grupo que se van a asignar. Los archivos deben encontrarse en el equipo que se está administrando y no puede usar **mapadmin** para administrar un equipo remoto con el fin de configurar asignaciones basadas en archivos de grupos y contraseñas.|

**removedomainmap** : quita una asignación simple entre un dominio de Windows y un dominio NIS. Las siguientes opciones y argumentos están disponibles para este argumento de comando:

|Opción|Definición|
|-----|-------|
|-d &lt;WindowsDomain&gt;|Especifica el dominio de Windows del mapa que se va a quitar.|
|-y &lt;Dominionis&gt;|Especifica el dominio NIS del mapa que se va a quitar.|
|-todo|Especifica que se van a quitar todas las asignaciones simples entre dominios de Windows y NIS. También se quitarán todas las asignaciones simples entre un dominio de Windows y los archivos de grupo y contraseña.|

**listdomainmaps** : enumera los dominios de Windows que están asignados a los dominios NIS o a los archivos de contraseña y grupo.

## <a name="notes"></a>Notas
-   Si no se especifica un argumento de comando, **mapadmin** muestra la configuración actual de asignación de nombres de usuario.
-   para todas las opciones que especifican un nombre de usuario o grupo, se pueden usar los siguientes formatos:
-   en el caso de los usuarios de Windows, use el formato &lt;&gt;de dominio \\&lt;nombre de usuario&gt;, \\\\&lt;equipo&gt;\\&lt;nombre de usuario&gt;, \\&lt;equipo&gt;\\&lt;nombre de usuario&gt;, o &lt;equipo&gt;\\&lt;nombre de usuario&gt;
-   en el caso de los grupos de Windows, use el formato &lt;dominio&gt;\\&lt;&lt;nombre_grupo&gt;&gt;, \\\\&lt;equipo&gt;\\&lt;&lt;nombre_grupo&gt;&gt;, \\&lt;equipo&gt;\\&lt;&lt;nombre_grupo&gt;&gt;, o &lt;&gt;\\&lt;&lt;&gt;nombre_grupo &gt;
-   en el caso de los usuarios de UNIX, use el formato &lt;Dominionis&gt;\\&lt;nombre de usuario&gt;, &lt;nombre de usuario&gt;@&lt;Dominionis&gt;, nombre &lt;de usuario&gt;@PCNFSo PCNFS\\&lt;nombre de usuario&gt;
-   en el caso de los grupos de UNIX, use el formato &lt;Dominionis&gt;\\&lt;GroupName&gt;, &lt;nombre_grupo&gt;@&lt;Dominionis&gt;, &lt;nombre_grupo&gt;@PCNFSo PCNFS\\&lt;nombre_grupo&gt;

## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
