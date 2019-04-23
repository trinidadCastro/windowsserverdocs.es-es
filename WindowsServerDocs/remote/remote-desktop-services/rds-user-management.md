---
title: Administrar usuarios de la colección de RDS
description: Obtenga información sobre cómo administrar usuarios en servicios de escritorio remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2727e1ab-69b8-46f3-9f6d-2540324fe596
author: christianmontoya
ms.author: chrimo
ms.date: 03/27/2018
manager: scottman
ms.openlocfilehash: 4b45061697926a3003712a88610cb17ef3c00c45
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859646"
---
# <a name="manage-users-in-your-rds-collection"></a>Administrar usuarios de la colección de RDS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Como administrador, puede administrar directamente los usuarios que tienen acceso a las colecciones específicas. De este modo, puede crear una colección de aplicaciones estándar para los trabajadores de información, pero, a continuación, crear una colección independiente con aplicaciones de modelado de gráficos para los ingenieros. Hay dos pasos principales para administrar el acceso de usuario en una implementación de servicios de escritorio remoto (RDS):

1.  [Crear usuarios y grupos en Active Directory](#create-your-users-and-groups-in-active-directory)
2.  [Asignar usuarios y grupos a las colecciones](#assign-users-and-groups-to-collections)


## <a name="create-your-users-and-groups-in-active-directory"></a>Crear los usuarios y grupos en Active Directory

En una implementación de RDS, servicios de dominio de Active Directory (AD DS) es el origen de todos los usuarios, grupos y otros objetos en el dominio. Puede administrar Active Directory directamente con PowerShell, o puede usar creado en las herramientas de interfaz de usuario que agregan flexibilidad y facilidad. Los siguientes pasos le guiarán para instalar estas herramientas, si no los tiene ya instalada y, a continuación, usar esas herramientas para administrar usuarios y grupos.

### <a name="install-ad-ds-tools"></a>Instalar herramientas de AD DS

Los siguientes pasos explican cómo instalar las herramientas de AD DS en un servidor que ejecute AD DS. Una vez instalado, puede crear usuarios o crear grupos.

1. Conéctese al servidor que ejecuta Servicios de dominio de Active Directory. Para las implementaciones de Azure:
   1. En el portal de Azure, haga clic en **examinar > grupos de recursos**y, a continuación, haga clic en el grupo de recursos para la implementación
   2. Seleccione la máquina virtual de AD.
   3. Haga clic en **Connect > Abrir** para abrir el cliente de escritorio remoto. Si **Connect** está deshabilitado, la máquina virtual no puede tener una dirección IP pública. Para darle una realizar los pasos siguientes, vuelva a intentarlo en este paso.
      1. Haga clic en **configuración > interfaces de red**y, a continuación, haga clic en la interfaz de red correspondiente.
      2. Haga clic en **configuración > dirección IP**.
      3. Para **dirección IP pública**, seleccione **habilitado**y, a continuación, haga clic en **dirección IP**.
      4. Si tiene una dirección IP pública existente que desea usar, selecciónelo en la lista. En caso contrario, haga clic en **crear nuevo**, escriba un nombre y, a continuación, haga clic en **Aceptar** y **guardar**.
   4. En el cliente, haga clic en **Connect**y, a continuación, haga clic en **usar otra cuenta**. Escriba el nombre de usuario y la contraseña para una cuenta de administrador de dominio.
   5. Haga clic en **Sí** cuando se le pregunte acerca del certificado.
2. Instalar las herramientas de AD DS:
   1. En el administrador del servidor, haga clic **administrar > Agregar Roles y características**.
   2. Haga clic en **instalación basada en roles o basada en características**y, a continuación, haga clic en el servidor de AD actual. Siga los pasos hasta llegar a la **características** ficha.
   3. Expanda **herramientas de administración remota del servidor > herramientas de administración de roles > AD DS y AD LDS herramientas**y, a continuación, seleccione **herramientas de AD DS**.
   4. Seleccione **reiniciar automáticamente el servidor de destino si es necesario**y, a continuación, haga clic en **instalar**.

### <a name="create-a-group"></a>Crear un grupo

Puede usar grupos de AD DS para conceder acceso a un conjunto de usuarios que necesitan usar los mismos recursos remotos.

1. En Administrador del servidor en el servidor que ejecuta AD DS, haga clic en **Herramientas > usuarios y equipos de usuarios de Active Directory**.
2. Expanda el dominio en el panel izquierdo para ver sus subcarpetas.
3. Haga clic en la carpeta donde desea crear el grupo y, a continuación, haga clic en **New > grupo**.
4. Escriba un nombre de grupo adecuado y luego seleccione **Global** y **seguridad**.

### <a name="create-a-user-and-add-to-a-group"></a>Cree un usuario y agregar a un grupo
1. En Administrador del servidor en el servidor que ejecuta AD DS, haga clic en **Herramientas > usuarios y equipos de usuarios de Active Directory**.
2. Expanda el dominio en el panel izquierdo para ver sus subcarpetas.
3. Haga clic en **usuarios**y, a continuación, haga clic en **nuevo > usuario**.
4. Escriba en como mínimo, un nombre y un nombre de inicio de sesión de usuario.
5. Escriba y confirme una contraseña para el usuario. Establecer las opciones de usuario apropiado, como **usuario debe cambiar la contraseña en el siguiente inicio de sesión**.
6. Agregar el nuevo usuario a un grupo:
   1. En el **usuarios** carpeta, haga clic en el nuevo usuario.
   2. Haga clic en **agregar a un grupo**.
   3. Escriba el nombre del grupo al que desea agregar el usuario.

## <a name="assign-users-and-groups-to-collections"></a>Asignar usuarios y grupos a las colecciones
Ahora que ha creado los usuarios y grupos en Active Directory, puede agregar algunos granularidad sobre quién tiene acceso a las colecciones de escritorio remoto en su implementación.

1. Conectarse al servidor que ejecuta el rol de agente de conexión de escritorio remoto (RD Connection Broker), siga los pasos descritos anteriormente.
2. Agregue los demás servidores de escritorio remoto al grupo del agente de conexión a Escritorio remoto de servidores administrados:
   1. En el administrador del servidor, haga clic **administrar > Agregar servidores**.
   2. Haz clic en **Buscar ahora**.
   3. Haga clic en cada servidor en la implementación que se está ejecutando un rol de servicios de escritorio remoto y, a continuación, haga clic en **Aceptar**.
3. Editar una colección para asignar acceso a determinados usuarios o grupos:
   1. En el administrador del servidor, haga clic **Remote Desktop Services > información general sobre**y, a continuación, haga clic en una recopilación específica.
   2. En **propiedades**, haga clic en **tareas > Editar propiedades**.
   3. Haga clic en **grupos de usuarios**.
   4. Haga clic en **agregar** y escriba el usuario o grupo que desea tener acceso a la colección. También puede quitar los usuarios y grupos desde esta ventana, seleccione el usuario o grupo que desea quitar y, a continuación, haga clic en **quitar**. 
   
   >[!NOTE] 
   > La ventana de grupos del usuario nunca puede estar vacía. Para restringir el ámbito de los usuarios que tienen acceso a la colección, primero debe agregar usuarios o grupos específicos antes de quitar los grupos más amplios.