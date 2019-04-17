---
title: "Administrar distintos dominios en el centro de administración de Active Directory"
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
ms.openlocfilehash: 5f253bd4952d8a347e97eafdb38d86fa98024b8d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="manage-different-domains-in-active-directory-administrative-center"></a>Administrar distintos dominios en el centro de administración de Active Directory

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

  Cuando abras administrativos de Active Directory, el dominio que se encuentra actualmente en este equipo \ (el domain\ local) aparece en el panel de navegación del centro de administración de Active Directory \(the left pane\). Dependiendo de los derechos del conjunto actual de credenciales de inicio de sesión, puede ver o administrar los objetos de Active Directory en este dominio local.

 También puedes usar el mismo conjunto de credenciales de inicio de sesión y la misma instancia del centro de administración de Active Directory para ver o administrar objetos de Active Directory en cualquier otro dominio en el mismo bosque, o en un dominio de otro bosque que tenga una confianza establecida con el equipo local dominio. Se admite one\ modo en que confía y two\ modo en que confía.

> [!NOTE]
>  Si hay una relación de confianza entre dominios A y B de dominio a través de one\ forma que los usuarios del dominio A pueden tener acceso a recursos en el dominio B, pero los usuarios del dominio B no pueden acceder a recursos en un dominio, si estás ejecutando el centro de administración de Active Directory en el equipo donde está el dominio A  el dominio local, que te conectes al dominio B con el conjunto de credenciales de inicio de sesión y en la misma instancia del centro de administración de Active Directory actual. Pero si estás ejecutando el centro de administración de Active Directory en el equipo donde dominio B es tu dominio local, no se puede conectar al dominio A con el mismo conjunto de credenciales en la misma instancia del centro de administración de Active Directory.

 No hay ninguna pertenencia mínimos necesario para completar este procedimiento.

### <a name="windows-server-2012-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2012: Para administrar un dominio externo en la instancia seleccionada del centro de administración de Active Directory mediante el conjunto actual de credenciales de inicio de sesión

1.  Para abrir el centro de administración de Active Directory, en **administrador del servidor**, haz clic en **herramientas**y, a continuación, haz clic en **centro de administración de Active Directory**.

    > [!NOTE]
    >  Otra forma para abrir el centro de administración de Active Directory es hacer clic en **inicio**y, a continuación, escribe **dsac.exe**.

2.  Para abrir **agregar nodos de navegación**, haz clic en **administrar**, a continuación, haz clic en **agregar nodos de navegación** como se muestra en la siguiente ilustración.

     ![Captura de pantalla mostrando ** Agregar navegación nodos ** interfaz de usuario](media/ADDS_ADACAddNavNode.gif)

3.  En **agregar nodos de navegación**, haz clic en **conectar a otros dominios** como se muestra en la siguiente ilustración.

     ![Captura de pantalla mostrando ** Agregar navegación nodos ** interfaz de usuario](media/ADDS_ADACConnectToDomain.gif)

4.  En **conectarse**, escribe el nombre de dominio externo que quieres administrar \ (por ejemplo, **contoso.com**\) y, a continuación, haz clic en **Aceptar**.

5.  Cuando está conectado correctamente al dominio externo, examinar las columnas de la **agregar nodos de navegación** ventana, seleccione los contenedores para agregar a tu panel de navegación del centro de administración de Active Directory y, a continuación, haz clic en **Aceptar**.

### <a name="windows-server-2008-r2-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2008 R2: Para administrar un dominio externo en la instancia seleccionada del centro de administración de Active Directory mediante el conjunto actual de credenciales de inicio de sesión

1.  Para abrir el centro de administración de Active Directory, haga clic en **inicio**, haz clic en **herramientas administrativas**y, a continuación, haz clic en **centro de administración de Active Directory**.

    > [!NOTE]
    >  Otra forma para abrir el centro de administración de Active Directory es hacer clic en **inicio**, haz clic en **ejecutar**y, a continuación, escribe **dsac.exe**.

2.  Para abrir **agregar nodos de navegación**, en la parte superior de la ventana del centro de administración de Active Directory, haga clic en **agregar nodos de navegación** como se muestra en la siguiente ilustración.

     ![Captura de pantalla mostrando ** Agregar navegación nodos ** interfaz de usuario](media/click_add_nav_nodes.gif)

    > [!NOTE]
    >  Otra manera de abrir **agregar nodos de navegación** es right\ y haga clic en cualquier lugar en el espacio vacío en el panel de navegación del centro de administración de Active Directory y, a continuación, haz clic en **agregar nodos de navegación**.

3.  En **agregar nodos de navegación**, haz clic en **conectar a otros dominios** como se muestra en la siguiente ilustración.

     ![Captura de pantalla mostrando ** Agregar navegación nodos ** ** conectarse a otros dominios ** interfaz de usuario](media/add_nav_nodes.gif)

4.  En **conectarse**, escribe el nombre de dominio externo que quieres administrar \ (por ejemplo, **contoso.com**\) y, a continuación, haz clic en **Aceptar**.

5.  Cuando está conectado correctamente al dominio externo, examinar las columnas de la **agregar nodos de navegación** ventana, seleccione los contenedores para agregar a tu panel de navegación del centro de administración de Active Directory y, a continuación, haz clic en **Aceptar**.

 Para obtener más información acerca de cómo personalizar el panel de navegación del centro de administración de Active Directory, consulte [personalizar el panel de navegación del centro de administración de Active Directory](customize-the-active-directory-administrative-center-navigation-pane.md).

 También puedes abrir el centro de administración de Active Directory mediante el uso de un conjunto de credenciales de inicio de sesión que sea diferente del conjunto actual de las credenciales de inicio de sesión. El comando en el siguiente procedimiento puede ser útil si ha iniciado sesión en el equipo que está ejecutando el centro de administración de Active Directory con credenciales de usuario normal, pero quieres usar el centro de administración de Active Directory en este equipo para administrar tu dominio local como administrador. \ (Este comando también puede ser útil si quieres usar el centro de administración de Active Directory para administrar remotamente un dominio externo que sea diferente a tu dominio local con un conjunto de credenciales que es diferente del conjunto actual de credenciales de inicio de sesión. Sin embargo, el dominio externo debe tener una confianza establecida con el dominio local. \)

 No hay ninguna pertenencia mínimos necesario para completar este procedimiento.

### <a name="to-manage-a-domain-using-logon-credentials-that-are-different-from-the-current-set-of-logon-credentials"></a>Para administrar un dominio con credenciales de inicio de sesión que son diferentes del conjunto actual de credenciales de inicio de sesión

1.  Para abrir el centro de administración de Active Directory, en un símbolo del sistema, escribe el siguiente comando y, a continuación, presiona ENTRAR:

     `runas /user:<domain\user> dsac`

     Donde `<domain\user>`es el conjunto de credenciales que quieres abrir el centro de administración de Active Directory con y `dsac`es el centro de administración de Active Directory archivo ejecutable nombre \(Dsac.exe\).

     Por ejemplo, escribe el siguiente comando y, a continuación, presiona ENTRAR:

     `runas /user:contoso\administrator dsac`

2.  Cuando está abierto centro de administración de Active Directory, desplazarse por el panel de navegación para ver o administrar tu dominio de Active Directory.

  

