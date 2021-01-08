---
title: Crear un rol de usuario para Access Control
description: Obtenga información sobre cómo crear un nuevo rol de usuario Access Control en la consola de cliente de IPAM.
manager: brianlic
ms.topic: article
ms.assetid: ae6a42db-a104-401b-a8e6-b85c47d30b46
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 88c0f82c6e6168d97681e0d6f795dd4101afc6a7
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98039725"
---
# <a name="create-a-user-role-for-access-control"></a>Crear un rol de usuario para Access Control

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para crear una nueva función de usuario Access Control en la consola de cliente de IPAM.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.

> [!NOTE]
> Después de crear un rol, puede crear una directiva de acceso para asignar el rol a un usuario o grupo de Active Directory específico. Para obtener más información, consulte [crear una directiva de acceso](../../technologies/ipam/Create-an-Access-Policy.md).

### <a name="to-create-a-role"></a>Para crear un rol

1.  En Administrador del servidor, haga clic en  **IPAM**. Aparece la consola de cliente de IPAM.

2.  En el panel de navegación, haga clic en **control de acceso** y, en el panel de navegación inferior, haga clic en **roles**.

    ![Roles de control de acceso](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_01.jpg)

3.  Haga clic con el botón secundario en **roles** y, a continuación, haga clic en **Agregar rol de usuario**.

    ![Agregar rol de usuario](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_02.jpg)

4.  Se abre el cuadro de diálogo **Agregar o Editar rol** . En **nombre**, escriba un nombre para el rol que hace que la función de rol esté desactivada. Por ejemplo, si desea crear un rol que permita a los administradores administrar los registros de recursos SRV de DNS, puede asignar el nombre **IPAMSrv** al rol. Si es necesario, desplácese hacia abajo en **operaciones** para buscar el tipo de operaciones que desea definir para el rol. En este ejemplo, desplácese hacia abajo hasta **las operaciones de administración de registros de recursos DNS**.

    ![Operaciones de administración de registros de recursos DNS](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_03.jpg)

5.  Expanda **operaciones de administración de registros de recursos DNS** y, a continuación, busque **operaciones de registro SRV**.

    ![Operaciones de registro SRV](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_04.jpg)

6.  Expanda y seleccione **operaciones de registro SRV** y, a continuación, haga clic en **Aceptar**.

    ![Seleccionar operaciones de registro SRV](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_05.jpg)

7.  En la consola de cliente de IPAM, haga clic en el rol que acaba de crear. En la **vista detalles,** se muestran las operaciones permitidas para el rol.

    ![Detalles de nuevo rol](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_06.jpg)

## <a name="see-also"></a>Consulte también
Access Control basado en [roles](Role-based-Access-Control.md) 
 [Administrar IPAM](Manage-IPAM.md)



