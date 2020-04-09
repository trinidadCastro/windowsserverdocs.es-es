---
title: Administrar dominios diferentes en Centro de administración de Active Directory
description: Seguridad de Windows Server
ms.prod: windows-server
ms.assetid: 166351c3-4076-48be-aa8f-797adf1e9d68
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 6690ffbc558db4026c3fe67168907ca953ad4081
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856988"
---
# <a name="manage-different-domains-in-active-directory-administrative-center"></a>Administrar dominios diferentes en Centro de administración de Active Directory

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

  Al abrir Active Directory administrativa, el dominio en el que ha iniciado sesión actualmente en este equipo \(el\) de dominio local aparece en el panel de navegación Centro de administración de Active Directory \(el panel izquierdo\). En función de los derechos del conjunto actual de credenciales de inicio de sesión, puede ver o administrar los Active Directory objetos de este dominio local.

 También puede usar el mismo conjunto de credenciales de inicio de sesión y la misma instancia de Centro de administración de Active Directory para ver o administrar los objetos de Active Directory de cualquier otro dominio del mismo bosque o un dominio de otro bosque que tenga una confianza establecida con el dominio local. Se admiten tanto confianzas\-como confianzas y dos confianzas\-.

> [!NOTE]
>  Si hay una única\-confianza entre el dominio A y el dominio B a través del cual los usuarios del dominio A pueden tener acceso a los recursos del dominio B, pero los usuarios del dominio B no pueden tener acceso a los recursos del dominio A, si ejecuta Centro de administración de Active Directory en el equipo donde el dominio A es el dominio local, puede conectarse al dominio B con el conjunto actual de Centro de administración de Active Directory credenciales de inicio de Pero si está ejecutando Centro de administración de Active Directory en el equipo donde el dominio B es el dominio local, no puede conectarse al dominio a con el mismo conjunto de credenciales en la misma instancia del Centro de administración de Active Directory.

 No se requiere una pertenencia a grupos mínima para llevar a cabo este procedimiento.

### <a name="windows-server-2012-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2012: para administrar un dominio externo en la instancia seleccionada de Centro de administración de Active Directory con el conjunto actual de credenciales de inicio de sesión

1.  Para abrir Centro de administración de Active Directory, en **Administrador del servidor**, haga clic en **herramientas**y, a continuación, haga clic en **centro de administración de Active Directory**.

    > [!NOTE]
    >  Otra manera de abrir Centro de administración de Active Directory es hacer clic en **Inicio**y, a continuación, escribir **DSAC. exe**.

2.  Para abrir **agregar nodos de navegación**, haga clic en **administrar**y, a continuación, haga clic en **agregar nodos de navegación** , tal como se muestra en la siguiente ilustración.

     ![Captura de pantalla que muestra la interfaz de usuario agregar nodos de navegación * *](media/ADDS_ADACAddNavNode.gif)

3.  En **agregar nodos de navegación**, haga clic en **conectar con otros dominios** , tal como se muestra en la siguiente ilustración.

     ![Captura de pantalla que muestra la interfaz de usuario agregar nodos de navegación * *](media/ADDS_ADACConnectToDomain.gif)

4.  En **conectar a**, escriba el nombre del dominio externo que desea administrar \(por ejemplo, **contoso.com**\)y, a continuación, haga clic en **Aceptar**.

5.  Cuando esté conectado correctamente al dominio externo, desplácese por las columnas de la ventana **agregar nodos de navegación** , seleccione el contenedor o contenedores que desee agregar al panel de navegación de centro de administración de Active Directory y, a continuación, haga clic en **Aceptar**.

### <a name="windows-server-2008-r2-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2008 R2: para administrar un dominio externo en la instancia seleccionada de Centro de administración de Active Directory con el conjunto actual de credenciales de inicio de sesión

1. Para abrir Centro de administración de Active Directory, haga clic en **Inicio**, en **herramientas administrativas**y, a continuación, en **centro de administración de Active Directory**.

   > [!NOTE]
   >  Otra manera de abrir Centro de administración de Active Directory es hacer clic en **Inicio**, en **Ejecutar**y, a continuación, escribir **DSAC. exe**.

2. Para abrir **agregar nodos de navegación**, cerca de la parte superior de la ventana de centro de administración de Active Directory, haga clic en **agregar nodos de navegación** , tal como se muestra en la siguiente ilustración.

    ![Captura de pantalla que muestra la interfaz de usuario agregar nodos de navegación * *](media/click_add_nav_nodes.gif)

   > [!NOTE]
   >  Otra manera de abrir **agregar nodos de navegación** es\-hacer clic en cualquier lugar del espacio vacío en el panel de navegación centro de administración de Active Directory y, a continuación, hacer clic en **agregar nodos de navegación**.

3. En **agregar nodos de navegación**, haga clic en **conectar con otros dominios** , tal como se muestra en la siguiente ilustración.

    ![Captura de pantalla que muestra * * agregar nodos de navegación * * * * conectar a otros dominios * * interfaz de usuario](media/add_nav_nodes.gif)

4. En **conectar a**, escriba el nombre del dominio externo que desea administrar \(por ejemplo, **contoso.com**\)y, a continuación, haga clic en **Aceptar**.

5. Cuando esté conectado correctamente al dominio externo, desplácese por las columnas de la ventana **agregar nodos de navegación** , seleccione el contenedor o contenedores que desee agregar al panel de navegación de centro de administración de Active Directory y, a continuación, haga clic en **Aceptar**.

   Para obtener más información sobre cómo personalizar el panel de navegación Centro de administración de Active Directory, consulte [personalizar el panel de navegación centro de administración de Active Directory](customize-the-active-directory-administrative-center-navigation-pane.md).

   También puede abrir Centro de administración de Active Directory mediante un conjunto de credenciales de inicio de sesión diferente del conjunto actual de credenciales de inicio de sesión. El comando del siguiente procedimiento puede ser útil si ha iniciado sesión en el equipo que ejecuta Centro de administración de Active Directory con las credenciales de usuario normales, pero desea usar Centro de administración de Active Directory en este equipo para administrar el dominio local como administrador. \(este comando también puede ser útil si desea usar Centro de administración de Active Directory para administrar de forma remota un dominio externo diferente del dominio local con un conjunto de credenciales diferente del conjunto actual de credenciales de inicio de sesión. Sin embargo, el dominio externo debe tener una confianza establecida con el dominio local.\)

   No se requiere una pertenencia a grupos mínima para llevar a cabo este procedimiento.

### <a name="to-manage-a-domain-using-logon-credentials-that-are-different-from-the-current-set-of-logon-credentials"></a>Para administrar un dominio con credenciales de inicio de sesión distintas del conjunto de credenciales de inicio de sesión actual

1.  Para abrir Centro de administración de Active Directory, en un símbolo del sistema, escriba el siguiente comando y, a continuación, presione ENTRAR:

     `runas /user:<domain\user> dsac`

     Donde `<domain\user>` es el conjunto de credenciales con las que desea abrir Centro de administración de Active Directory y `dsac` es el nombre del archivo ejecutable Centro de administración de Active Directory \(DSAC. exe\).

     Por ejemplo, escriba el comando siguiente y presione ENTRAR:

     `runas /user:contoso\administrator dsac`

2.  Cuando Centro de administración de Active Directory está abierto, desplácese por el panel de navegación para ver o administrar el dominio de Active Directory.

  

