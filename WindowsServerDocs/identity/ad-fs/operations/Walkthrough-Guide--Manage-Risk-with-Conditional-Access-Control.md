---
ms.assetid: 3a840b63-78b7-4e62-af7b-497026bfdb93
title: 'Guía de tutorial: administración de riesgos con Control de acceso condicional'
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f034c2eeafe9d52569e8181bbbb2e582b1059d51
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188860"
---
# <a name="walkthrough-guide-manage-risk-with-conditional-access-control"></a>Guía de tutorial: Administración de riesgos con control de acceso condicional




## <a name="about-this-guide"></a>Acerca de esta guía
Este tutorial proporciona instrucciones para la administración de riesgos con uno de los factores (datos de usuario) disponibles a través del mecanismo de control de acceso condicional en Active Directory Federation Services (AD FS) en Windows Server 2012 R2. Para obtener más información acerca de los mecanismos de autorización y control de acceso condicional en AD FS en Windows Server 2012 R2, consulte [administración de riesgos con Control de acceso condicional](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md).

Este tutorial incluye las secciones siguientes:

-   [Paso 1: Configurar el entorno de laboratorio](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Paso 2: Comprobar el mecanismo de control de acceso predeterminado de AD FS](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_2)

-   [Paso 3: Configurar la directiva de control de acceso condicional según los datos de usuario](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_3)

-   [Paso 4: Comprobar el mecanismo de control de acceso condicional](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_4)

## <a name="BKMK_1"></a>Paso 1: Configuración del entorno de laboratorio
Para completar este tutorial, necesita un entorno con los siguientes componentes:

-   Un dominio de Active Directory con un usuario de prueba y cuentas de grupo que se ejecutan en Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 con el esquema actualizado a Windows Server 2012 R2 o un dominio de Active Directory que se ejecutan en Windows Server 2012 R2

-   Un servidor de federación que se ejecutan en Windows Server 2012 R2

-   Un servidor web que hospede la aplicación de ejemplo

-   Un equipo cliente desde el que pueda obtener acceso a la aplicación de ejemplo

> [!WARNING]
> Se recomienda (tanto en entornos de producción como de prueba) no usar el mismo equipo como servidor de federación y servidor web.

En este entorno, el servidor de federación emite las notificaciones necesarias para que los usuarios puedan tener acceso a la aplicación de ejemplo. El servidor web hospeda una aplicación de ejemplo que confiará en los usuarios que presenten las notificaciones emitidas por el servidor de federación.

Para obtener instrucciones sobre cómo configurar este entorno, consulte [configurar el entorno de laboratorio para AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

## <a name="BKMK_2"></a>Paso 2: Comprobar el mecanismo de control de acceso predeterminado de AD FS
En este paso comprobará el mecanismo de control de acceso predeterminado de AD FS, en el que se redirige al usuario a la página de inicio de sesión de AD FS, el usuario proporciona credenciales válidas y se le concede acceso a la aplicación. Puede usar el **Robert Hatley** cuenta de AD y el **claimapp** aplicación de ejemplo que configuró en [configurar el entorno de laboratorio para AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

#### <a name="to-verify-the-default-ad-fs-access-control-mechanism"></a>Para comprobar el mecanismo de control de acceso predeterminado de AD FS

1.  En el equipo cliente, abra una ventana del explorador y vaya a la aplicación de ejemplo: **https://webserv1.contoso.com/claimapp**.

    Esta acción redirige automáticamente la solicitud al servidor de federación y se le solicita que inicie sesión con un nombre de usuario y una contraseña.

2.  Escriba las credenciales de la **Robert Hatley** cuenta de AD que creó en [configurar el entorno de laboratorio para AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Se le concederá acceso a la aplicación.

## <a name="BKMK_3"></a>Paso 3: configurar la directiva de control de acceso condicional según los datos de usuario
En este paso configurará una directiva de control de acceso basándose en los datos de pertenencia a grupos del usuario. Es decir, configurará una **Regla de autorización de emisión** en el servidor de federación para una relación de confianza para usuario autenticado que represente a la aplicación de ejemplo **claimapp**. Según la lógica de la regla, **Robert Hatley** AD se emitirán al usuario las notificaciones necesarias para tener acceso a esta aplicación porque pertenece a un **Finance** grupo. Ha agregado el **Robert Hatley** cuenta a la **Finance** grupo [configurar el entorno de laboratorio para AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

Puede completar esta tarea mediante la Consola de administración de AD FS o Windows PowerShell.

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-the-ad-fs-management-console"></a>Para configurar la directiva de control de acceso condicional según los datos de usuario mediante la Consola de administración de AD FS

1.  En la Consola de administración de AD FS, vaya a **Relaciones de confianza**y, a continuación, a **Relaciones de confianza para usuario autenticado**.

2.  Seleccione la relación de confianza para usuario autenticado que represente a la aplicación de ejemplo (**claimapp**) y, a continuación, en el panel **Acciones** o haciendo clic con el botón secundario en esta relación de confianza para usuario autenticado, seleccione **Editar reglas de notificación**.

3.  En la ventana **Editar reglas de notificación para claimapp** , seleccione la pestaña **Reglas de autorización de emisión** y haga clic en **Agregar regla**.

4.  En la página **Seleccionar plantilla de regla**del **Asistente para agregar reglas de notificación de autorización de emisión**, seleccione la plantilla de regla de notificación **Permitir o denegar el acceso a los usuarios según una notificación entrante** y haga clic en **Siguiente**.

5.  En la página **Configurar regla** , realice todas las acciones siguientes y, a continuación, haga clic en **Finalizar**:

    1.  Escriba un nombre para la regla de notificación, por ejemplo, **TestRule**.

    2.  Seleccione **SID de grupo** como **Tipo de notificación entrante**.

    3.  Haga clic en **Examinar**, escriba **Finance** como nombre del grupo de prueba de AD y resuélvalo para el campo **Valor de notificación entrante** .

    4.  Seleccione la opción **Denegar acceso a los usuarios con esta notificación entrante** .

6.  En la ventana **Editar reglas de notificación para claimapp** , asegúrese de eliminar la regla **Permitir el acceso a todos los usuarios** que se creó de manera predeterminada al crear esta relación de confianza para usuario autenticado.

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-windows-powershell"></a>Para configurar la directiva de control de acceso condicional según los datos de usuario mediante Windows PowerShell

1.  En el servidor de federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando:


    `$rp = Get-AdfsRelyingPartyTrust -Name claimapp`


2.  En la misma ventana de Windows PowerShell, ejecute el siguiente comando:


    `$GroupAuthzRule = '@RuleTemplate = "Authorization" @RuleName = "Foo" c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i)<group_SID>$"] =>issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");'
    Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -IssuanceAuthorizationRules $GroupAuthzRule`

> [!NOTE]
> Asegúrese de reemplazar <group_SID> con el valor del SID de grupo de AD **Finance**.

## <a name="BKMK_4"></a>Paso 4: comprobar el mecanismo de control de acceso condicional
En este paso comprobará la directiva de control de acceso condicional que configuró en el paso anterior. Puede usar el siguiente procedimiento para comprobar que el usuario de AD **Robert Hatley** puede tener acceso a la aplicación de ejemplo porque pertenece al grupo **Finance** y que los usuarios de AD que no pertenecen al grupo **Finance** no pueden tener acceso a la aplicación de ejemplo.

1.  En el equipo cliente, abra una ventana del explorador y vaya a la aplicación de ejemplo: **https://webserv1.contoso.com/claimapp**

    Esta acción redirige automáticamente la solicitud al servidor de federación y se le solicita que inicie sesión con un nombre de usuario y una contraseña.

2.  Escriba las credenciales de la **Robert Hatley** cuenta de AD que creó en [configurar el entorno de laboratorio para AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Se le concederá acceso a la aplicación.

3.  Escriba las credenciales de otro usuario de AD que no pertenezca al grupo **Finance**. (Para obtener más información sobre cómo crear cuentas de usuario en AD, consulte [ https://technet.microsoft.com/library/cc7833232.aspx ](https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx).

    En este momento, debido a la directiva de control de acceso que configuró en el paso anterior, se muestra un mensaje de "acceso denegado" para este usuario de AD que no pertenece a la **Finance** grupo. El texto del mensaje predeterminado es **no está autorizado para tener acceso a este sitio. Haga clic aquí para cerrar la sesión y vuelva a iniciarla o póngase en contacto con su administrador para los permisos.** No obstante, este texto se puede personalizar. Para obtener más información sobre cómo personalizar la experiencia de inicio de sesión, consulte [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).

## <a name="see-also"></a>Vea también
[Administración de riesgos con Control de acceso condicional](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)
[configurar el entorno de laboratorio para AD FS en Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



