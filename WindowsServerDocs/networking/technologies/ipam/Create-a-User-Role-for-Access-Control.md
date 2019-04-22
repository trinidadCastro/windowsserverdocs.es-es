---
title: Crear un rol de usuario para el control de acceso
description: Este tema forma parte de la Guía de administración de administración de direcciones IP (IPAM) en Windows Server 2016.
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
ms.openlocfilehash: 69d7acec19a460b51819bdc30ce40e21089c7bcf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823586"
---
# <a name="create-a-user-role-for-access-control"></a>Crear un rol de usuario para el control de acceso

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para crear un nuevo rol de usuario de Control de acceso en la consola de cliente IPAM.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.  
  
> [!NOTE]  
> Después de crear un rol, puede crear una directiva de acceso para asignar el rol a un usuario específico o un grupo de Active Directory. Para obtener más información, consulte [crear una directiva de acceso](../../technologies/ipam/Create-an-Access-Policy.md).  
  
### <a name="to-create-a-role"></a>Para crear un rol  
  
1.  En el administrador del servidor, haga clic en **IPAM**. Aparece la consola de cliente IPAM.  
  
2.  En el panel de navegación, haga clic en **CONTROL de acceso**y haga clic en el panel de navegación inferior, **Roles**.  
  
    ![Funciones de control de acceso](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_01.jpg)  
  
3.  Haga clic en **Roles**y, a continuación, haga clic en **Agregar rol de usuario**.  
  
    ![Agregar rol de usuario](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_02.jpg)  
  
4.  El **agregar o Editar función** abre el cuadro de diálogo. En **nombre**, escriba un nombre para el rol que hace que la función de rol a borrar. Por ejemplo, si desea crear un rol que permite a los administradores administrar los registros de recursos SRV de DNS, podría denominar la función **IPAMSrv**. Si es necesario, desplácese hacia abajo en **operaciones** para buscar el tipo de operaciones que desea definir para el rol. En este ejemplo, desplácese hacia abajo hasta **las operaciones de administración de registros de recursos DNS**.  
  
    ![Operaciones de administración de registros de recursos DNS](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_03.jpg)  
  
5.  Expanda **las operaciones de administración de registros de recursos DNS**y, a continuación, busque **operaciones con registros de SRV**.  
  
    ![Operaciones de registro SRV](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_04.jpg)  
  
6.  Expanda y seleccione **operaciones con registros de SRV**y, a continuación, haga clic en **Aceptar**.  
  
    ![Seleccione las operaciones de registro de SRV](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_05.jpg)  
  
7.  En la consola de cliente IPAM, haga clic en el rol que acaba de crear. En **vista de detalles,** se muestran las operaciones permitidas para el rol.  
  
    ![Detalles del nuevo rol](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_06.jpg)  
  
## <a name="see-also"></a>Vea también  
[Control de acceso basado en roles](Role-based-Access-Control.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


