---
title: Administración de dominios diferentes en el centro de administración de Active Directory
ms.prod: windows-server-threshold
description: Seguridad de Windows Server
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.assetid: 166351c3-4076-48be-aa8f-797adf1e9d68
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: bd5650724272422d09e87b7eecf10f825b00fabf
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447042"
---
# <a name="manage-different-domains-in-active-directory-administrative-center"></a>Administración de dominios diferentes en el centro de administración de Active Directory

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

  Al abrir administrativos de Active Directory, el dominio que actualmente ha iniciado sesión en este equipo \(el dominio local\) aparece en el panel de navegación del centro de administración de Active Directory \(el panel izquierdo\). Según los derechos de su conjunto actual de credenciales de inicio de sesión, puede ver o administrar los objetos de Active Directory de este dominio local.

 También puede usar el mismo conjunto de credenciales de inicio de sesión y la misma instancia de centro de administración de Active Directory para ver o administrar objetos de Active Directory en cualquier otro dominio en el mismo bosque o un dominio de otro bosque que tenga una confianza establecida con el equipo local dominio. Ambos uno\-modo en que confía y dos\-se admite el modo en que confía.

> [!NOTE]
>  Si hay una\-de confianza bidireccional entre un dominio y el dominio B a través del cual los usuarios en el dominio A pueden tener acceso a recursos en el dominio B pero los usuarios del dominio B no pueden tener acceso a recursos en el dominio A, si está ejecutando el centro de administración de Active Directory en el equipo donde el dominio A es el dominio local, puede conectarse al dominio B con el conjunto de credenciales de inicio de sesión y en la misma instancia de centro de administración de Active Directory actual. Pero si está ejecutando el centro de administración de Active Directory en el equipo donde el dominio B es el dominio local, no puede conectarse al dominio A con el mismo conjunto de credenciales en la misma instancia de centro de administración de Active Directory.

 No se requiere una pertenencia a grupos mínima para llevar a cabo este procedimiento.

### <a name="windows-server-2012-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2012: Para administrar un dominio externo de la instancia seleccionada del centro de administración de Active Directory con el conjunto actual de credenciales de inicio de sesión

1.  Para abrir el centro de administración de Active Directory, en **administrador del servidor**, haga clic en **herramientas**y, a continuación, haga clic en **centro de administración de Active Directory**.

    > [!NOTE]
    >  Otra forma de abrir el centro de administración de Active Directory es hacer clic en **iniciar**y, a continuación, escriba **dsac.exe**.

2.  Para abrir **agregar nodos de navegación**, haga clic en **administrar**, a continuación, haga clic en **agregar nodos de navegación** tal como se muestra en la siguiente ilustración.

     ![Captura de pantalla mostrando ** Agregar navegación nodos ** la interfaz de usuario](media/ADDS_ADACAddNavNode.gif)

3.  En **agregar nodos de navegación**, haga clic en **conectar a otros dominios** tal como se muestra en la siguiente ilustración.

     ![Captura de pantalla mostrando ** Agregar navegación nodos ** la interfaz de usuario](media/ADDS_ADACConnectToDomain.gif)

4.  En **conectarse a**, escriba el nombre del dominio externo que desea administrar \(por ejemplo, **contoso.com**\)y, a continuación, haga clic en **Aceptar**.

5.  Cuando está conectado correctamente al dominio externo, desplácese por las columnas en el **agregar nodos de navegación** ventana, seleccione los contenedores para agregar al panel de navegación del centro de administración de Active Directory, y a continuación, haga clic en **Aceptar**.

### <a name="windows-server-2008-r2-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2008 R2: Para administrar un dominio externo de la instancia seleccionada del centro de administración de Active Directory con el conjunto actual de credenciales de inicio de sesión

1. Para abrir el centro de administración de Active Directory, haga clic en **iniciar**, haga clic en **herramientas administrativas**y, a continuación, haga clic en **centro de administración de Active Directory**.

   > [!NOTE]
   >  Otra forma de abrir el centro de administración de Active Directory es hacer clic en **iniciar**, haga clic en **ejecutar**y, a continuación, escriba **dsac.exe**.

2. Para abrir **agregar nodos de navegación**, cerca de la parte superior de la ventana del centro de administración de Active Directory, haga clic en **agregar nodos de navegación** tal como se muestra en la siguiente ilustración.

    ![Captura de pantalla mostrando ** Agregar navegación nodos ** la interfaz de usuario](media/click_add_nav_nodes.gif)

   > [!NOTE]
   >  Otra forma de abrir **agregar nodos de navegación** a derecha\-haga clic en cualquier lugar en el espacio vacío en el panel de navegación del centro de administración de Active Directory y, a continuación, haga clic en **agregar nodos de navegación**.

3. En **agregar nodos de navegación**, haga clic en **conectar a otros dominios** tal como se muestra en la siguiente ilustración.

    ![Captura de pantalla mostrando ** Agregar navegación nodos ** ** conectar a otros dominios ** la interfaz de usuario](media/add_nav_nodes.gif)

4. En **conectarse a**, escriba el nombre del dominio externo que desea administrar \(por ejemplo, **contoso.com**\)y, a continuación, haga clic en **Aceptar**.

5. Cuando está conectado correctamente al dominio externo, desplácese por las columnas en el **agregar nodos de navegación** ventana, seleccione los contenedores para agregar al panel de navegación del centro de administración de Active Directory, y a continuación, haga clic en **Aceptar**.

   Para obtener más información acerca de cómo personalizar el panel de navegación del centro de administración de Active Directory, consulte [personalizar el panel de navegación del centro de administración de Active Directory](customize-the-active-directory-administrative-center-navigation-pane.md).

   También puede abrir el centro de administración de Active Directory mediante el uso de un conjunto de credenciales de inicio de sesión que es diferente de su conjunto actual de credenciales de inicio de sesión. El comando en el siguiente procedimiento puede ser útil si ha iniciado sesión el equipo que ejecuta el centro de administración de Active Directory con credenciales de usuario normal, pero desea usar el centro de administración de Active Directory en este equipo para administrar su dominio local como administrador. \(Este comando también puede ser útil si desea usar el centro de administración de Active Directory para administrar un dominio externo que sea diferente de su dominio local con un conjunto de credenciales que es diferente de su conjunto actual de credenciales de inicio de sesión de forma remota. Sin embargo, el dominio externo debe tener una confianza establecida con el dominio local.\)

   No se requiere una pertenencia a grupos mínima para llevar a cabo este procedimiento.

### <a name="to-manage-a-domain-using-logon-credentials-that-are-different-from-the-current-set-of-logon-credentials"></a>Para administrar un dominio con credenciales de inicio de sesión distintas del conjunto de credenciales de inicio de sesión actual

1.  Para abrir el centro de administración de Active Directory, en un símbolo del sistema, escriba el siguiente comando y, a continuación, presione ENTRAR:

     `runas /user:<domain\user> dsac`

     Donde `<domain\user>` es el conjunto de credenciales que desea abrir el centro de administración de Active Directory con y `dsac` es el nombre de archivo ejecutable del centro de administración de Active Directory \(Dsac.exe\).

     Por ejemplo, escriba el comando siguiente y presione ENTRAR:

     `runas /user:contoso\administrator dsac`

2.  Cuando se abra centro de administración de Active Directory, examine el panel de navegación para ver o administrar el dominio de Active Directory.

  

