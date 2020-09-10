---
title: Administrar dominios diferentes en Centro de administración de Active Directory
description: Seguridad de Windows Server
ms.assetid: 166351c3-4076-48be-aa8f-797adf1e9d68
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: c793c30c6637a050d2b2cbb055ec289e3c3e8f20
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89638033"
---
# <a name="manage-different-domains-in-active-directory-administrative-center"></a>Administrar dominios diferentes en Centro de administración de Active Directory

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

  Al abrir Active Directory administrativa, el dominio en el que ha iniciado sesión actualmente en este equipo \( aparece en el panel \) de navegación centro de administración de Active Directory del panel \( izquierdo \) . Según los derechos de su conjunto actual de credenciales de inicio de sesión, podrá ver o administrar los objetos de Active Directory de este dominio local.

 También puede usar el mismo conjunto de credenciales de inicio de sesión y la misma instancia de Centro de administración de Active Directory para ver o administrar los objetos de Active Directory de cualquier otro dominio del mismo bosque o un dominio de otro bosque que tenga una confianza establecida con el dominio local. \-Se admiten confianzas unidireccionales y \- confianzas bidireccionales.

> [!NOTE]
>  Si hay una confianza unidireccional \- entre el dominio a y el dominio b a través del cual los usuarios del dominio a pueden tener acceso a los recursos del dominio b, pero los usuarios del dominio b no pueden tener acceso a los recursos del dominio a, si ejecuta centro de administración de Active Directory en el equipo donde el dominio a es el dominio local, puede conectarse al dominio b con el conjunto actual de centro de administración de Active Directory credenciales de inicio de sesión No obstante, si ejecuta el Centro de administración de Active Directory en el equipo en el que el dominio B es el dominio local, no podrá conectarse al dominio A con el mismo conjunto de credenciales de inicio de sesión en la misma instancia del Centro de administración de Active Directory.

 No se requiere una pertenencia a grupos mínima para llevar a cabo este procedimiento.

### <a name="windows-server-2012-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2012: para administrar un dominio externo en la instancia seleccionada de Centro de administración de Active Directory con el conjunto actual de credenciales de inicio de sesión

1.  Para abrir Centro de administración de Active Directory, en **Administrador del servidor**, haga clic en **herramientas**y, a continuación, haga clic en **centro de administración de Active Directory**.

    > [!NOTE]
    >  Otra manera de abrir Centro de administración de Active Directory es hacer clic en **Inicio**y, a continuación, escribir **dsac.exe**.

2.  Para abrir **agregar nodos de navegación**, haga clic en **administrar**y, a continuación, haga clic en **agregar nodos de navegación** , tal como se muestra en la siguiente ilustración.

     ![Captura de pantalla que muestra la interfaz de usuario agregar nodos de navegación * *](media/ADDS_ADACAddNavNode.gif)

3.  En **agregar nodos de navegación**, haga clic en **conectar con otros dominios** , tal como se muestra en la siguiente ilustración.

     ![Captura de pantalla que muestra la interfaz de usuario agregar nodos de navegación * *](media/ADDS_ADACConnectToDomain.gif)

4.  En **conectar a**, escriba el nombre del dominio externo que desea administrar, \( por ejemplo, **contoso.com** \) y, a continuación, haga clic en **Aceptar**.

5.  Cuando esté conectado correctamente al dominio externo, desplácese por las columnas de la ventana **Agregar nodos de navegación**, seleccione los contenedores que desee agregar al panel de navegación del Centro de administración de Active Directory y haga clic en **Aceptar**.

### <a name="windows-server-2008-r2-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2008 R2: para administrar un dominio externo en la instancia seleccionada de Centro de administración de Active Directory con el conjunto actual de credenciales de inicio de sesión

1. Para abrir el Centro de administración de Active Directory, haga clic en **Inicio**, en **Herramientas administrativas** y, a continuación, en **Centro de administración de Active Directory**.

   > [!NOTE]
   >  Otra forma de abrir Centro de administración de Active Directory es hacer clic en **Inicio** y en **Ejecutar** y, a continuación, escribir **dsac.exe**.

2. Para abrir **agregar nodos de navegación**, cerca de la parte superior de la ventana de centro de administración de Active Directory, haga clic en **agregar nodos de navegación** , tal como se muestra en la siguiente ilustración.

    ![Captura de pantalla que muestra la interfaz de usuario agregar nodos de navegación * *](media/click_add_nav_nodes.gif)

   > [!NOTE]
   >  Otra manera de abrir **agregar nodos de navegación** es hacer clic con el botón secundario en \- cualquier lugar del espacio vacío en el panel de navegación centro de administración de Active Directory y, a continuación, hacer clic en **agregar nodos de navegación**.

3. En **agregar nodos de navegación**, haga clic en **conectar con otros dominios** , tal como se muestra en la siguiente ilustración.

    ![Captura de pantalla que muestra * * agregar nodos de navegación * * * * conectar a otros dominios * * interfaz de usuario](media/add_nav_nodes.gif)

4. En **conectar a**, escriba el nombre del dominio externo que desea administrar, \( por ejemplo, **contoso.com** \) y, a continuación, haga clic en **Aceptar**.

5. Cuando esté conectado correctamente al dominio externo, desplácese por las columnas de la ventana **Agregar nodos de navegación**, seleccione los contenedores que desee agregar al panel de navegación del Centro de administración de Active Directory y haga clic en **Aceptar**.

   Para obtener más información sobre cómo personalizar el panel de navegación Centro de administración de Active Directory, consulte [personalizar el panel de navegación centro de administración de Active Directory](customize-the-active-directory-administrative-center-navigation-pane.md).

   También puede abrir el Centro de administración de Active Directory con un conjunto de credenciales de inicio de sesión distinto del actual. El comando del procedimiento siguiente puede resultar útil si ha iniciado sesión en el equipo que ejecuta el Centro de administración de Active Directory con credenciales de usuario normales pero desea usar el Centro de administración de Active Directory de este equipo para administrar el dominio local como administrador. \(Este comando también puede ser útil si desea usar Centro de administración de Active Directory para administrar de forma remota un dominio externo que sea diferente del dominio local con un conjunto de credenciales que sea diferente del conjunto actual de credenciales de inicio de sesión. Sin embargo, el dominio externo debe tener una confianza establecida con el dominio local.\)

   No se requiere una pertenencia a grupos mínima para llevar a cabo este procedimiento.

### <a name="to-manage-a-domain-using-logon-credentials-that-are-different-from-the-current-set-of-logon-credentials"></a>Para administrar un dominio con credenciales de inicio de sesión distintas del conjunto de credenciales de inicio de sesión actual

1.  Para abrir el Centro de administración de Active Directory, escriba el comando siguiente en el símbolo del sistema y presione ENTRAR:

     `runas /user:<domain\user> dsac`

     Donde `<domain\user>` es el conjunto de credenciales que desea abrir centro de administración de Active Directory con y `dsac` es el centro de administración de Active Directory nombre del archivo ejecutable \(Dsac.exe\) .

     Por ejemplo, escriba el siguiente comando y presione ENTRAR:

     `runas /user:contoso\administrator dsac`

2.  Cuando el Centro de administración de Active Directory esté abierto, desplácese por el panel de navegación para ver o administrar el dominio de Active Directory.



