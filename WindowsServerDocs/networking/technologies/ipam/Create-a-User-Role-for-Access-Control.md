---
title: Crear un rol de usuario para el Control de acceso
description: Este tema es parte de la Guía de administración de administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae6a42db-a104-401b-a8e6-b85c47d30b46
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fa0ed71d399ad638a648946952fe170d93f69ceb
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-user-role-for-access-control"></a>Crear un rol de usuario para el Control de acceso

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para crear un nuevo rol de usuario de Control de acceso en la consola IPAM del cliente.  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar este procedimiento.  
  
> [!NOTE]  
> Después de crear una función, puedes crear una directiva de acceso para asignar el rol a un usuario específico o un grupo de Active Directory. Para obtener más información, consulta [crear una directiva de acceso](../../technologies/ipam/Create-an-Access-Policy.md).  
  
### <a name="to-create-a-role"></a>Para crear una función  
  
1.  En el administrador del servidor, haz clic en **IPAM**. Aparece la consola IPAM del cliente.  
  
2.  En el panel de navegación, haz clic en **el CONTROL de acceso**y haga clic en el panel de navegación inferior, **Roles**.  
  
    ![Funciones de control de acceso](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_01.jpg)  
  
3.  Haz clic en **Roles**y, a continuación, haz clic en **Agregar usuario función**.  
  
    ![Agrega el rol de usuario](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_02.jpg)  
  
4.  La **agregar o editar rol** abre el cuadro de diálogo. En **nombre**, escribe un nombre para la función que hace que la función de rol borrar. Por ejemplo, si quieres crear una función que permite a los administradores administrar registros de recursos de SRV de DNS, puede asignar el rol **IPAMSrv**. Si es necesario, desplázate hacia abajo en **operaciones** para encontrar el tipo de operaciones que desea definir la función. Para este ejemplo, desplázate hacia abajo hasta **las operaciones de administración de registros de recursos DNS**.  
  
    ![Operaciones de administración de registros de recursos DNS](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_03.jpg)  
  
5.  Expande **las operaciones de administración de registros de recursos DNS**y, a continuación, busque **operaciones con registros de SRV**.  
  
    ![Operaciones de registro SRV](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_04.jpg)  
  
6.  Expande y selecciona **operaciones con registros de SRV**y, a continuación, haz clic en **Aceptar**.  
  
    ![Selecciona las operaciones de registros SRV](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_05.jpg)  
  
7.  En la consola del cliente IPAM, haz clic en el rol que acabas de crear. En **vista de detalles,** se muestran las operaciones permitidas para el rol.  
  
    ![Detalles de función nueva](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_06.jpg)  
  
## <a name="see-also"></a>Consulta también  
[Control de acceso basadas en roles](Role-based-Access-Control.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


