---
title: Crear una directiva de acceso
description: Obtenga información sobre cómo crear una directiva de acceso en la consola de cliente de IPAM.
manager: brianlic
ms.topic: article
ms.assetid: 854bd064-2f86-4678-a940-a04b3e48ae10
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 3fd8490321070dc73658bd6c6cad08f09bb9025c
ms.sourcegitcommit: 84b97d34d606b6bf4b6ec8760a93107f1b311428
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99245492"
---
# <a name="create-an-access-policy"></a>Crear una directiva de acceso

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para crear una directiva de acceso en la consola de cliente de IPAM.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.

> [!NOTE]
> Puede crear una directiva de acceso para un usuario específico o para un grupo de usuarios en Active Directory. Al crear una directiva de acceso, debe seleccionar un rol de IPAM integrado o un rol personalizado que haya creado. Para obtener más información sobre los roles personalizados, consulte [crear un rol de usuario para Access Control](../../technologies/ipam/Create-a-User-Role-for-Access-Control.md).

### <a name="to-create-an-access-policy"></a>Para crear una directiva de acceso

1.  En Administrador del servidor, haga clic en  **IPAM**. Aparece la consola de cliente de IPAM.

2.  En el panel de navegación, haga clic en **control de acceso**. En el panel de navegación inferior, haga clic con el botón secundario en **directivas de acceso** y, a continuación, haga clic en **Agregar Directiva de acceso**.

    ![Captura de pantalla de Administrador del servidor que muestra las directivas de acceso optoin resaltadas y la opción Agregar Directiva de acceso disponible para la selección.](../../media/Create-an-Access-Policy/ipam_CreateAP_01.jpg)

3.  Se abre el cuadro de diálogo **Agregar Directiva de acceso** . En **configuración de usuario**, haga clic en **Agregar**.

    ![Captura de pantalla del cuadro de diálogo Agregar Directiva de acceso.](../../media/Create-an-Access-Policy/ipam_CreateAP_02.jpg)

4.  Se abre el cuadro de diálogo **Seleccionar usuario o grupo** . Haga clic en **Ubicaciones**.

    ![Ubicaciones de grupos o usuarios](../../media/Create-an-Access-Policy/ipam_CreateAP_03.jpg)

5.  Se abrirá el cuadro de diálogo **ubicaciones** . Vaya a la ubicación que contiene la cuenta de usuario, seleccione la ubicación y, a continuación, haga clic en **Aceptar**. Se cierra el cuadro de diálogo **ubicaciones** .

    ![Seleccionar ubicación](../../media/Create-an-Access-Policy/ipam_CreateAP_04.jpg)

6.  En el cuadro de diálogo **Seleccionar usuarios o grupos** , en **Escriba el nombre de objeto que desea seleccionar**, escriba el nombre de la cuenta de usuario para la que desea crear una directiva de acceso. Haga clic en **Aceptar**.

7.  En **Agregar Directiva de acceso**, en **configuración de usuario**, alias de **usuario** contiene ahora la cuenta de usuario a la que se aplica la Directiva. En **configuración de acceso**, haga clic en **nuevo**.

    ![Nueva configuración de acceso](../../media/Create-an-Access-Policy/ipam_CreateAP_05.jpg)

8.  En **Agregar Directiva de acceso**, la **configuración de acceso** cambia a **nueva configuración**.

    ![Nombre del cuadro de diálogo cambiar a nueva configuración](../../media/Create-an-Access-Policy/ipam_CreateAP_06.jpg)

9. Haga clic en **Seleccionar rol** para expandir la lista de roles. Seleccione uno de los roles integrados o, si ha creado nuevos roles, seleccione uno de los roles que ha creado. Por ejemplo, si ha creado el rol IPAMSrv para aplicar al usuario, haga clic en **IPAMSrv**.

    ![Seleccionar rol](../../media/Create-an-Access-Policy/ipam_CreateAP_07.jpg)

10. Haga clic en **Agregar configuración**.

    ![Agregar nueva configuración](../../media/Create-an-Access-Policy/ipam_CreateAP_08.jpg)

11. El rol se agrega a la Directiva de acceso. Para crear directivas de acceso adicionales, haga clic en **aplicar** y repita estos pasos para cada una de las directivas que desee crear. Si no desea crear directivas adicionales, haga clic en **Aceptar**.

    ![Haga clic en aplicar o en Aceptar.](../../media/Create-an-Access-Policy/ipam_CreateAP_09.jpg)

12. En el panel de información de la consola del cliente IPAM, compruebe que se ha creado la nueva Directiva de acceso.

    ![Ver la nueva Directiva de acceso](../../media/Create-an-Access-Policy/ipam_CreateAP_09a.jpg)

## <a name="see-also"></a>Consulte también
Access Control basado en [roles](Role-based-Access-Control.md) 
 [Administrar IPAM](Manage-IPAM.md)



