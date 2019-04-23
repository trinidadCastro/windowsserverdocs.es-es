---
title: Crear una directiva de acceso
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
ms.assetid: 854bd064-2f86-4678-a940-a04b3e48ae10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c8a97cd145a695bc8755f9111291e5c8bba2e572
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881906"
---
# <a name="create-an-access-policy"></a>Crear una directiva de acceso

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para crear una directiva de acceso en la consola de cliente IPAM.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.  
  
> [!NOTE]  
> Puede crear una directiva de acceso para un usuario específico o para un grupo de usuarios en Active Directory. Cuando se crea una directiva de acceso, debe seleccionar un rol integrado de IPAM o un rol personalizado que ha creado. Para obtener más información sobre los roles personalizados, consulte [crear un rol de usuario para el Control de acceso](../../technologies/ipam/Create-a-User-Role-for-Access-Control.md).  
  
### <a name="to-create-an-access-policy"></a>Para crear una directiva de acceso  
  
1.  En el administrador del servidor, haga clic en **IPAM**. Aparece la consola de cliente IPAM.  
  
2.  En el panel de navegación, haga clic en **CONTROL de acceso**. En el panel de navegación inferior, haga clic en **las directivas de acceso**y, a continuación, haga clic en **Agregar directiva de acceso**.  
  
    ![Agregar directiva de acceso](../../media/Create-an-Access-Policy/ipam_CreateAP_01.jpg)  
  
3.  El **Agregar directiva de acceso** abre el cuadro de diálogo. En **configuración de usuario**, haga clic en **agregar**.  
  
    ![Agregar directiva de acceso](../../media/Create-an-Access-Policy/ipam_CreateAP_02.jpg)  
  
4.  El **Seleccionar usuario o grupo** abre el cuadro de diálogo. Haga clic en **ubicaciones**.  
  
    ![Ubicaciones de usuario o grupo](../../media/Create-an-Access-Policy/ipam_CreateAP_03.jpg)  
  
5.  El **ubicaciones** abre el cuadro de diálogo. Vaya a la ubicación que contiene la cuenta de usuario, seleccione la ubicación y, a continuación, haga clic en **Aceptar**. El **ubicaciones** cierra el cuadro de diálogo.  
  
    ![Seleccionar ubicación](../../media/Create-an-Access-Policy/ipam_CreateAP_04.jpg)  
  
6.  En el **Seleccionar usuario o grupo** cuadro de diálogo **escriba el nombre de objeto a seleccionar**, escriba el nombre de cuenta de usuario para el que desea crear una directiva de acceso. Haga clic en **Aceptar**.  
  
7.  En **Agregar directiva de acceso**, en **configuración de usuario**, **alias de usuario** ahora contiene la cuenta de usuario al que se aplica la directiva. En **configuración de acceso**, haga clic en **New**.  
  
    ![Nueva configuración de acceso](../../media/Create-an-Access-Policy/ipam_CreateAP_05.jpg)  
  
8.  En **Agregar directiva de acceso**, **configuración de acceso** cambia a **nueva configuración**.  
  
    ![Nombre del cuadro de diálogo Cambiar a la nueva configuración](../../media/Create-an-Access-Policy/ipam_CreateAP_06.jpg)  
  
9. Haga clic en **Seleccionar rol** para expandir la lista de roles. Seleccione uno de los roles integrados, o bien, si ha creado nuevos roles, seleccione uno de los roles que ha creado. Por ejemplo, si ha creado el rol de IPAMSrv que se aplicará al usuario, haga clic en **IPAMSrv**.  
  
    ![Seleccionar rol](../../media/Create-an-Access-Policy/ipam_CreateAP_07.jpg)  
  
10. Haga clic en **Agregar configuración**.  
  
    ![Agregar nueva configuración](../../media/Create-an-Access-Policy/ipam_CreateAP_08.jpg)  
  
11. El rol se agrega a la directiva de acceso. Para crear directivas de acceso adicionales, haga clic en **aplicar**y, a continuación, repita estos pasos para cada directiva que desea crear. Si no desea crear directivas adicionales, haga clic en **Aceptar**.  
  
    ![Haga clic en Aplicar o Aceptar](../../media/Create-an-Access-Policy/ipam_CreateAP_09.jpg)  
  
12. En el panel de información de consola del cliente IPAM, compruebe que se crea la nueva directiva de acceso.  
  
    ![Ver la nueva directiva de acceso](../../media/Create-an-Access-Policy/ipam_CreateAP_09a.jpg)  
  
## <a name="see-also"></a>Vea también  
[Control de acceso basado en roles](Role-based-Access-Control.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


