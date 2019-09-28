---
title: mapadmin
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fc4b76c1989298ea83c480b9c838ce0fc18fef5f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373758"
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

&lt;computer @ no__t-1 especifica el equipo remoto que ejecuta el servicio Asignación de nombres de usuario que desea administrar. Puede especificar el equipo con un nombre de servicio de nombres Internet de Windows (WINS) o un nombre de sistema de nombres de dominio (DNS), o bien mediante la dirección del Protocolo de Internet (IP).

-u &lt;user @ no__t-1 especifica el nombre de usuario del usuario cuyas credenciales se van a utilizar. Puede que sea necesario agregar el nombre de dominio al nombre de usuario con el formato <em>dominio</em> **\\** <em>nombre de usuario</em>.

-p &lt;password @ no__t-1 especifica la contraseña del usuario. Si se especifica la opción **-u** pero se omite la opción **-p** , se le solicitará la contraseña del usuario.
La acción específica que realiza **mapadmin** depende del argumento de comando que especifique:

## <a name="parameters"></a>Parámetros
### <a name="start"></a>start
inicia el servicio Asignación de nombres de usuario.

### <a name="stop"></a>stop
Detiene el servicio Asignación de nombres de usuario.

### <a name="config"></a>configurar
Especifica la configuración general para Asignación de nombres de usuario. Las siguientes opciones están disponibles con este argumento de comando: **-r &lt;dddd @ no__t-2: &lt;HH @ no__t-4: &lt;mm @ no__t-6** -especifica el intervalo de actualización para la actualización de las bases de datos de Windows y NIS en días, horas y minutos. El intervalo mínimo es de 5 minutos.
**-i {yes | no}** : activa la asignación simple en (**sí**) o desactivada (**no**). De forma predeterminada, la asignación simple está activada.
**Agregar** : crea una nueva asignación para un usuario o grupo. Las siguientes opciones están disponibles con este argumento de comando:

|Opción|Definición|
|-----|-------|
|-Wu &lt;name @ no__t-1|Especifica el nombre del usuario de Windows para el que se crea una nueva asignación.|
|-UU &lt;name @ no__t-1|Especifica el nombre del usuario de UNIX para el que se crea una nueva asignación.|
|-WG &lt;group @ no__t-1|Especifica el nombre del grupo de Windows para el que se crea una nueva asignación.|
|-UG &lt;group @ no__t-1|Especifica el nombre del grupo de UNIX para el que se crea una nueva asignación.|
|-setprimary|Especifica que la nueva asignación es la asignación principal.|

**setprimary** : especifica qué asignación es la asignación principal para un usuario o grupo UNIX con varias asignaciones. Las siguientes opciones están disponibles con este argumento de comando:

|Opción|Definición|
|-----|-------|
|-Wu &lt;name @ no__t-1|Especifica el usuario de Windows de la asignación principal. Si existe más de una asignación para el usuario, use la opción **-UU** para especificar la asignación principal.|
|-UU &lt;name @ no__t-1|Especifica el usuario de UNIX de la asignación principal.|
|-WG &lt;group @ no__t-1|Especifica el grupo de Windows de la asignación principal. Si existe más de una asignación para el grupo, use la opción **-UG** para especificar la asignación principal.|
|-UG &lt;group @ no__t-1|Especifica el grupo UNIX de la asignación principal.|

**eliminar** : quita la asignación de un usuario o grupo. Las siguientes opciones están disponibles para este argumento de comando:

|Opción|Definición|
|-----|-------|
|-Wu &lt;user @ no__t-1|El usuario de Windows para el que se eliminará la asignación, especificado como &lt;*WindowsDomain @ no__t-2 @ no__t-3 @ no__t-4User Name @ no__t-5*. Debe especificar las opciones **-Wu** o **-UU** , o ambas. Si especifica ambas opciones, se eliminará la asignación concreta identificada por las dos opciones. Si solo especifica la opción **-Wu** , se eliminarán todas las asignaciones para el usuario especificado.|
|-WG &lt;group @ no__t-1|El grupo de Windows para el que se eliminará la asignación, especificado como &lt;WindowsDomain @ no__t-1 @ no__t-2 @ no__t-3groupname @ no__t-4. Debe especificar la opción **-WG** o **-UG** , o ambas. Si especifica ambas opciones, se eliminará la asignación concreta identificada por las dos opciones. Si solo especifica la opción **-WG** , se eliminarán todas las asignaciones para el grupo especificado.|
|-UU &lt;user @ no__t-1|El usuario de UNIX para el que se eliminará la asignación, especificado como @no__t nombre 0User @ no__t-1. Debe especificar las opciones **-Wu** o **-UU** , o ambas. Si especifica ambas opciones, se eliminará la asignación concreta identificada por las dos opciones. Si solo especifica la opción **-UU** , se eliminarán todas las asignaciones para el usuario especificado.|
|-UG &lt;group @ no__t-1|El grupo UNIX para el que se eliminará la asignación, especificado como &lt;groupname @ no__t-1. Debe especificar la opción **-WG** o **-UG** , o ambas. Si especifica ambas opciones, se eliminará la asignación concreta identificada por las dos opciones. Si solo especifica la opción **-UG** , se eliminarán todas las asignaciones para el grupo especificado.|

**lista** : muestra información acerca de las asignaciones de usuario y grupo. Las siguientes opciones están disponibles con este argumento de comando:

|Opción|Definición|
|-----|-------|
|-todo|muestra las asignaciones simples y avanzadas para usuarios y grupos.|
|-simple|enumera todos los usuarios y grupos asignados simples.|
|-avanzado|enumera todos los usuarios y grupos asignados avanzada. Las asignaciones se enumeran en el orden en el que se evalúan. Las asignaciones principales, marcadas con un asterisco (*), se enumeran primero, seguidas de las asignaciones secundarias, que se marcan con un acento circunflejo (^).|
|-Wu &lt;name @ no__t-1|muestra la asignación de un usuario de Windows especificado.|
|-WG &lt;group @ no__t-1|muestra la asignación de un grupo de Windows.|
|-UU &lt;name @ no__t-1|muestra la asignación de un usuario de UNIX.|
|-UG &lt;group @ no__t-1|muestra la asignación de un grupo de UNIX.|

**copia de seguridad** : guarda los datos de configuración y asignación de asignación de nombres de usuario en el archivo especificado por &lt;filename @ no__t-2.
**restore** : reemplaza los datos de configuración y asignación con los datos del archivo (especificado por &lt;filename @ no__t-2) que se creó con el argumento del comando de **copia de seguridad** .
**adddomainmap** : agrega un mapa sencillo entre un dominio de Windows y un dominio NIS o archivos de contraseña y grupo. Las siguientes opciones están disponibles para este argumento de comando:

|Opción|Definición|
|-----|-------|
|-d &lt;WindowsDomain @ no__t-1|Especifica el dominio de Windows que se va a asignar.|
|-y &lt;NISdomain @ no__t-1|Especifica el dominio NIS que se va a asignar. &lt;br/&gt; @ no__t-2BR/&gt; **-n** &lt;nisServer @ no__t-6 especifica el servidor NIS para el dominio NIS especificado con la opción **-y** .|
|-f &lt;path @ no__t-1|Especifica la ruta de acceso completa del directorio que contiene los archivos de contraseña y de grupo que se van a asignar. Los archivos deben encontrarse en el equipo que se está administrando y no puede usar **mapadmin** para administrar un equipo remoto con el fin de configurar asignaciones basadas en archivos de grupos y contraseñas.|

**removedomainmap** : quita una asignación simple entre un dominio de Windows y un dominio NIS. Las siguientes opciones y argumentos están disponibles para este argumento de comando:

|Opción|Definición|
|-----|-------|
|-d &lt;WindowsDomain @ no__t-1|Especifica el dominio de Windows del mapa que se va a quitar.|
|-y &lt;NISdomain @ no__t-1|Especifica el dominio NIS del mapa que se va a quitar.|
|-todo|Especifica que se van a quitar todas las asignaciones simples entre dominios de Windows y NIS. También se quitarán todas las asignaciones simples entre un dominio de Windows y los archivos de grupo y contraseña.|

**listdomainmaps** : enumera los dominios de Windows que están asignados a los dominios NIS o a los archivos de contraseña y grupo.

## <a name="notes"></a>Notas
-   Si no se especifica un argumento de comando, **mapadmin** muestra la configuración actual de asignación de nombres de usuario.
-   para todas las opciones que especifican un nombre de usuario o grupo, se pueden usar los siguientes formatos:
-   para usuarios de Windows, use el formato &lt;domain @ no__t-1 @ no__t-2 @ no__t-3user Name @ no__t-4, \\ @ no__t-6 @ no__t-7computer @ no__t-8 @ no__t-9 @ no__t-10user Name @ no__t-11, &gt;2 @ no__t-13computer @ no__t-14 @ no__t-15 @ no__t-16user Name @ No__ t-17 o 8computer @ no__t-19 @ no__t-20 @ no__t-21user Name @ no__t-22
-   para grupos de Windows, use el formato &lt;domain @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4groupname @ no__t-5 @ no__t-6, \\ @ no__t-8 @ no__t-9computer @ no__t-10 @ no__t-11 @ no__t-12 @ no__t-13groupname @ no__t-14 @ no__t-15, &gt;6 @ no__t-17computer @ No_ _ t-18 @ no__t-19 @ no__t-20 @ no__t-21groupname @ no__t-22 @ no__t-23 o \\4computer @ no__t-25 @ no__t-26 @ no__t-27 @ no__t-28groupname @ no__t-29 @ no__t-30
-   para los usuarios de UNIX, use el formato &lt;NISdomain @ no__t-1 @ no__t-2 @ no__t-3user Name @ no__t-4, &lt;user Name @ no__t-6 @ no__t-7 @ no__t-8NISdomain @ no__t-9, User &gt;0name @ no__t-11 @ no__t-12 o PCNFS @ no__t-13 @ no__t-14user Name @ no__t-15
-   para los grupos de UNIX, use el formato &lt;NISdomain @ no__t-1 @ no__t-2 @ no__t-3groupname @ no__t-4, &lt;groupname @ no__t-6 @ no__t-7 @ no__t-8NISdomain @ no__t-9, &gt;0groupname @ no__t-11 @ no__t-12 o PCNFS @ no__t-13 @ no__t-14groupname @ no__t-15

## <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
