---
title: Administración de usuarios de la colección de RDS
description: Aprende a administrar usuarios en Servicios de Escritorio remoto.
ms.topic: article
ms.assetid: 2727e1ab-69b8-46f3-9f6d-2540324fe596
author: christianmontoya
ms.author: chrimo
ms.date: 03/27/2018
manager: scottman
ms.openlocfilehash: a0ddb8ddc26df58e130315a3e1e0b70953c61dc4
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954812"
---
# <a name="manage-users-in-your-rds-collection"></a>Administración de usuarios de la colección de RDS

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

Como administrador, puedes administrar directamente los usuarios que tienen acceso a colecciones específicas. De esta forma, puedes crear una colección con aplicaciones estándar para trabajadores de la información y, a continuación, crear una colección independiente con aplicaciones de modelado intensivo de gráficos para ingenieros. Hay dos pasos principales para administrar el acceso de usuarios en una implementación de Servicios de Escritorio remoto (RDS):

1.    [Crear usuarios y grupos de Active Directory](#create-your-users-and-groups-in-active-directory)
2.    [Asignar usuarios y grupos a las colecciones](#assign-users-and-groups-to-collections)


## <a name="create-your-users-and-groups-in-active-directory"></a>Creación de usuarios y grupos de Active Directory

En una implementación de RDS, Active Directory Domain Services (AD DS) es el origen de todos los usuarios, grupos y demás objetos del dominio. Puedes administrar Active Directory directamente con PowerShell o puedes usar herramientas de interfaz de usuario integradas que agregan flexibilidad y facilidad de uso. Los siguientes pasos te indicarán cómo instalar estas herramientas, si no las tienes ya instaladas y, a continuación, cómo usarlas para administrar usuarios y grupos.

### <a name="install-ad-ds-tools"></a>Instalación de herramientas de AD DS

Los siguientes pasos explican cómo instalar las herramientas de AD DS en un servidor que ya ejecuta AD DS. Una vez instaladas, puedes crear usuarios o grupos.

1. Conéctate al servidor que está ejecutando Active Directory Domain Services. Para implementaciones de Azure:
   1. En Azure Portal, haz clic en **Examinar > Grupos de recursos** y, después, haz clic en el grupo de recursos para la implementación.
   2. Selecciona la máquina virtual de AD.
   3. Haz clic en **Conectar > Abrir** para abrir el cliente de Escritorio remoto. Si la opción **Conectar** está deshabilitada, es posible que la máquina virtual no tenga una dirección IP pública. Para darle una, realiza los pasos siguientes y, después, vuelve a intentar este paso.
      1. Haz clic en **Configuración > Interfaces de red** y luego en la interfaz de red correspondiente.
      2. Haz clic en **Configuración> Dirección IP**.
      3. Para la **dirección IP pública**, selecciona **Habilitado** y haz clic en **Dirección IP**.
      4. Si tienes una dirección IP pública existente que quieras usar, selecciónala de la lista. De lo contrario, haz clic en **Crear nuevo**, escribe un nombre y haz clic en **Aceptar** y **Guardar**.
   4. En el cliente, haz clic en **Conectar** y, a continuación, en **Usar otra cuenta**. Escribe el nombre de usuario y la contraseña de una cuenta de administrador de dominio.
   5. Haz clic en **Sí** cuando se te pida el certificado.
2. Instala las herramientas de AD DS:
   1. En el Administrador del servidor, haz clic en **Administrar > Agregar roles y características**.
   2. Haz clic en **Instalación basada en características o en roles** y, a continuación, haz clic en el servidor de AD actual. Sigue los pasos hasta llegar a la pestaña **Características**.
   3. Expande **Herramientas de administración remota del servidor > Herramientas de administración de roles > Herramientas de AD DS y AD LDS** y, después, selecciona **Herramientas de AD DS**.
   4. Selecciona **Reiniciar el servidor de destino automáticamente si es necesario**, y luego haz clic en **Instalar**.

### <a name="create-a-group"></a>Crear un grupo

Puedes usar grupos de AD DS para conceder acceso a un conjunto de usuarios que necesitan usar los mismos recursos remotos.

1. En el Administrador del servidor, haz clic en **Herramientas > Usuarios y equipos de Active Directory**.
2. Expande el dominio en el panel izquierdo para ver sus subcarpetas.
3. Haz clic con el botón derecho en la carpeta donde quieres crear el grupo y, después, haz clic en **Nuevo > Grupo**.
4. Escribe un nombre de grupo adecuado y selecciona **Global** y **Seguridad**.

### <a name="create-a-user-and-add-to-a-group"></a>Creación de un usuario e incorporación de este a un grupo
1. En el Administrador del servidor, haz clic en **Herramientas > Usuarios y equipos de Active Directory**.
2. Expande el dominio en el panel izquierdo para ver sus subcarpetas.
3. Haz clic con el botón derecho en **Usuarios** y, a continuación, haz clic en **Nuevo > Usuario**.
4. Escribe, como mínimo, un nombre y un nombre de inicio de sesión de usuario.
5. Escribe y confirma una contraseña para el usuario. Establece las opciones de usuario adecuadas, como **El usuario debe cambiar la contraseña en el siguiente inicio de sesión**.
6. Agrega el nuevo usuario a un grupo:
   1. En la carpeta **Usuarios**, haz clic con el botón derecho en el nuevo usuario.
   2. Haz clic en **Agregar a un grupo**.
   3. Escribe el nombre del grupo al que deseas agregar el usuario.

## <a name="assign-users-and-groups-to-collections"></a>Asignación de usuarios y grupos a colecciones
Ahora que has creado los usuarios y grupos en Active Directory, puedes agregar alguna granularidad sobre quién tiene acceso a las colecciones de Escritorio remoto de su implementación.

1. Conéctate al servidor que ejecuta el rol de agente de Conexión a Escritorio remoto mediante los pasos descritos anteriormente.
2. Agrega los demás servidores de Escritorio remoto al grupo del agente de Conexión a Escritorio remoto de los servidores administrados:
   1. En el Administrador del servidor, haz clic en **Administrar > Agregar servidores**.
   2. Haz clic en **Buscar ahora**.
   3. Haz clic en cada servidor de la implementación que esté ejecutando un rol de Servicios de Escritorio remoto y, a continuación, haz clic en **Aceptar**.
3. Edita una colección para asignar el acceso a determinados usuarios o grupos:
   1. En el Administrador del servidor, haz clic en **Servicios de Escritorio remoto > Información general** y, después, haz clic en una colección específica.
   2. En **Propiedades**, haz clic en **Tareas > Editar propiedades**.
   3. Haz clic en **Grupos de usuarios**.
   4. Haz clic en **Agregar** y especifica el usuario o grupo que quieres que acceda a la colección. También puedes quitar usuarios y grupos desde esta ventana. Para ello, selecciona el usuario o grupo que deseas quitar y, a continuación, haz clic en **Quitar**.

   >[!NOTE]
   > La ventana Grupos de usuarios nunca puede estar vacía. Para restringir el ámbito de los usuarios que tienen acceso a la colección, primero debes agregar usuarios o grupos específicos antes de quitar los grupos más amplios.