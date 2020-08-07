---
title: mapadmin
description: Artículo de referencia para el comando mapadmin, que administra Asignación de nombres de usuario para los servicios de Microsoft para Network File System.
ms.topic: article
ms.assetid: b17332c7-8622-4223-9c43-2fb9cf4d992d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3334ae31b3abb85dcc3df046d8199c9b72ea3944
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87886572"
---
# <a name="mapadmin"></a>mapadmin

La utilidad de línea de comandos **mapadmin** administra asignación de nombres de usuario en el equipo local o remoto en el que se ejecutan los servicios de Microsoft para Network File System. Si ha iniciado sesión con una cuenta que no tiene credenciales administrativas, puede especificar un nombre de usuario y una contraseña de una cuenta que sí los tenga.

## <a name="syntax"></a>Sintaxis

```
mapadmin [<computer>] [-u <user> [-p <password>]]
mapadmin [<computer>] [-u <user> [-p <password>]] {start | stop}
mapadmin [<computer>] [-u <user> [-p <password>]] config <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] add -wu <windowsuser> -uu <UNIXuser> [-setprimary]
mapadmin [<computer>] [-u <user> [-p <password>]] add -wg <windowsgroup> -ug <UNIXgroup> [-setprimary]
mapadmin [<computer>] [-u <user> [-p <password>]] setprimary -wu <Windowsuser> [-uu <UNIXuser>]
mapadmin [<computer>] [-u <user> [-p <password>]] setprimary -wg <Windowsgroup> [-ug <UNIXgroup>]
mapadmin [<computer>] [-u <user> [-p <password>]] delete <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] list <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] backup <filename>
mapadmin [<computer>] [-u <user> [-p <password>]] restore <filename>
mapadmin [<computer>] [-u <user> [-p <password>]] adddomainmap -d <Windowsdomain> {-y <<NISdomain>> | -f <path>}
mapadmin [<computer>] [-u <user> [-p <password>]] removedomainmap -d <Windowsdomain> -y <<NISdomain>>
mapadmin [<computer>] [-u <user> [-p <password>]] removedomainmap -all
mapadmin [<computer>] [-u <user> [-p <password>]] listdomainmaps
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<computer>` | Especifica el equipo remoto que ejecuta el servicio Asignación de nombres de usuario que desea administrar. Puede especificar el equipo con un nombre de servicio de nombres Internet de Windows (WINS) o un nombre de sistema de nombres de dominio (DNS), o bien mediante la dirección del Protocolo de Internet (IP). |
| -u`<user>` | Especifica el nombre del usuario cuyas credenciales se van a usar. Puede que sea necesario agregar el nombre de dominio al nombre de usuario con el formato *dominio\nombredeusuario*. |
| -p`<password>` | Especifica la contraseña del usuario. Si se especifica la opción **-u** pero se omite la opción **-p** , se le solicitará la contraseña del usuario. |
| `start | stop` | Inicia o detiene el servicio Asignación de nombres de usuario. |
| config | Especifica la configuración general para Asignación de nombres de usuario. Las siguientes opciones están disponibles con este parámetro:<ul><li>**-r `<dddd>:<hh>:<mm>` :** especifica el intervalo de actualización para la actualización de las bases de datos de Windows y NIS en días, horas y minutos. El intervalo mínimo es de 5 minutos.</li><li>**-i `{yes | no}` : activa la** asignación simple en (**sí**) o desactivada (**no**). De forma predeterminada, la asignación está activada.</li></ul> |
| add | Crea una nueva asignación para un usuario o grupo. Las siguientes opciones están disponibles con este parámetro:<ul><li>**-Wu `<name>` :** especifica el nombre del usuario de Windows para el que se crea una nueva asignación.</li><li>**-UU `<name>` :** especifica el nombre del usuario de UNIX para el que se crea una nueva asignación.</li><li>**-WG `<group>` :** especifica el nombre del grupo de Windows para el que se crea una nueva asignación.</li><li>**-UG `<group>` :** especifica el nombre del grupo de UNIX para el que se crea una nueva asignación.</li><li>**-setprimary:** Especifica que la nueva asignación es la asignación principal.</li></ul> |
| setprimary | Especifica qué asignación es la asignación principal para un usuario o grupo UNIX con varias asignaciones. Las siguientes opciones están disponibles con este parámetro:<ul><li>**-Wu `<name>` :** especifica el usuario de Windows de la asignación principal. Si existe más de una asignación para el usuario, use la opción **-UU** para especificar la asignación principal.</li><li>**-UU `<name>` :** especifica el usuario de UNIX de la asignación principal.</li><li>**-WG `<group>` :** especifica el grupo de Windows de la asignación principal. Si existe más de una asignación para el grupo, use la opción **-UG** para especificar la asignación principal.</li><li>**-UG `<group>` :** especifica el grupo UNIX de la asignación principal.</li></ul> |
| delete | Quita la asignación de un usuario o grupo. Las siguientes opciones están disponibles para este parámetro:<ul><li>**-Wu `<user>` :** especifica el usuario de Windows para el que se eliminará la asignación, especificado como `<windowsdomain>\<username>` .<p>Debe especificar las opciones **-Wu** o **-UU** , o ambas. Si especifica ambas opciones, se eliminará la asignación concreta identificada por las dos opciones. Si solo especifica la opción **-Wu** , se eliminarán todas las asignaciones para el usuario especificado.</li><li>**-UU `<user>` :** especifica el usuario de UNIX para el que se eliminará la asignación, especificado como `<username>` .<p>Debe especificar las opciones **-Wu** o **-UU** , o ambas. Si especifica ambas opciones, se eliminará la asignación concreta identificada por las dos opciones. Si solo especifica la opción **-UU** , se eliminarán todas las asignaciones para el usuario especificado.</li><li>**-WG `<group>` :** especifica el grupo de Windows para el que se eliminará la asignación, especificado como `<windowsdomain>\<username>` .<p>Debe especificar la opción **-WG** o **-UG** , o ambas. Si especifica ambas opciones, se eliminará la asignación concreta identificada por las dos opciones. Si solo especifica la opción **-WG** , se eliminarán todas las asignaciones para el grupo especificado.</li><li>**-UG `<group>` :** especifica el grupo de UNIX para el que se eliminará la asignación, especificado como `<groupname>` .<p>Debe especificar la opción **-WG** o **-UG** , o ambas. Si especifica ambas opciones, se eliminará la asignación concreta identificada por las dos opciones. Si solo especifica la opción **-UG** , se eliminarán todas las asignaciones para el grupo especificado.</li></ul> |
| list | Muestra información acerca de las asignaciones de usuario y grupo. Las siguientes opciones están disponibles con este parámetro:<ul><li>**-todo:** Muestra las asignaciones simples y avanzadas para usuarios y grupos.</li><li>**-simple:** Enumera todos los usuarios y grupos asignados simples.</li><li>**-avanzado:** Enumera todos los usuarios y grupos asignados avanzada. Las asignaciones se enumeran en el orden en el que se evalúan. Las asignaciones principales, marcadas con un asterisco (*), se enumeran primero, seguidas de las asignaciones secundarias, que están marcadas con un acento circunflejo `(^)` . </li> <li> * *-Wu `<name>` :** muestra la asignación de un usuario de Windows especificado.</li><li>**-WG `<group>` :** muestra la asignación de un grupo de Windows.</li><li>**-UU `<name>` :** muestra la asignación de un usuario de Unix.</li><li>**-UG `<group>` :** muestra la asignación de un grupo de Unix.</li></ul> |
| copia de seguridad | Guarda Asignación de nombres de usuario configuración y asigna los datos al archivo especificado por `<filename>` . |
| Restauración | Reemplaza los datos de configuración y asignación con los datos del archivo (especificado por `<filename>` ) que se creó mediante el parámetro **backup** . |
| adddomainmap | Agrega un mapa sencillo entre un dominio de Windows y un dominio NIS o archivos de contraseña y grupo. Las siguientes opciones están disponibles para este parámetro:<ul><li>**-d `<windowsdomain>` :** especifica el dominio de Windows que se va a asignar.</li><li>**-y `<NISdomain>` :** especifica el dominio NIS que se va a asignar. Debe usar el parámetro **-n `<NISserver>` ** para especificar el servidor NIS para el dominio NIS especificado por la opción **-y** .</li><li>**-f `<path>` :** especifica la ruta de acceso completa del directorio que contiene los archivos de contraseña y de grupo que se van a asignar. Los archivos deben encontrarse en el equipo que se está administrando y no puede usar **mapadmin** para administrar un equipo remoto con el fin de configurar asignaciones basadas en archivos de grupos y contraseñas.</li></ul> |
| removedomainmap | Quita una asignación simple entre un dominio de Windows y un dominio NIS. Las siguientes opciones y el argumento están disponibles para este parámetro:<ul><li>**-d `<windowsdomain>` :** especifica el dominio de Windows del mapa que se va a quitar.</li><li>**-y `<NISdomain>` :** especifica el dominio NIS del mapa que se va a quitar.</li><li>**-todo:** Especifica que se van a quitar todas las asignaciones simples entre dominios de Windows y NIS. También se quitarán todas las asignaciones simples entre un dominio de Windows y los archivos de grupo y contraseña.</li></ul> |
| listdomainmaps | Enumera los dominios de Windows que están asignados a los dominios NIS o a los archivos de contraseña y grupo. |

#### <a name="remarks"></a>Observaciones

- Si no especifica ningún valor de parámetros, el comando **mapadmin** muestra la configuración actual de asignación de nombres de usuario.

- Para todas las opciones que especifican un nombre de usuario o grupo, se pueden usar los siguientes formatos:

    - En el caso de los usuarios de Windows, use los formatos: `<domain>\<username>` , `\\<computer>\<username>` , `\<computer>\<username>` o.`<computer>\<username>`

    - En el caso de los grupos de Windows, use los formatos: `<domain>\<groupname>` , `\\<computer>\<groupname>` , `\<computer>\<groupname>` o.`<computer>\<groupname>`

    - En el caso de los usuarios de UNIX, use los formatos: `<NISdomain>\<username>` , `<username>@<NISdomain>` , `<username>@PCNFS` o.`PCNFS\<username>`

    - En el caso de los grupos de UNIX, use los formatos: `<NISdomain>\<groupname>` , `<groupname>@<NISdomain>` , `<groupname>@PCNFS` o.`PCNFS\<groupname>`

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
