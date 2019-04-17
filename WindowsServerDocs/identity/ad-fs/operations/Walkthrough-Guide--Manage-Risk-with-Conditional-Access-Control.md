---
ms.assetid: 3a840b63-78b7-4e62-af7b-497026bfdb93
title: "Guía paso a paso: administrar el riesgo con Control de acceso condicional"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 11d2d567f9264dca53a3426263a172649d7d7c11
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="walkthrough-guide-manage-risk-with-conditional-access-control"></a>Guía paso a paso: Administrar el riesgo con Control de acceso condicional

>Se aplica a: Windows Server 2012 R2


## <a name="about-this-guide"></a>Acerca de esta guía
Este tutorial proporciona instrucciones para la administración de riesgo con uno de los factores (datos del usuario) disponibles mediante el mecanismo de control de acceso condicional en servicios de federación de Active Directory (AD FS) en Windows Server 2012 R2. Para obtener más información acerca de los mecanismos de autorización y el control de acceso condicional en ADFS en Windows Server 2012 R2, consulta [administrar riesgo con el Control de acceso condicional](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md).

Este tutorial consta de las siguientes secciones:

-   [Paso 1: Configurar el entorno de laboratorio](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Paso 2: Comprobar el mecanismo de control de acceso predeterminada AD FS](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_2)

-   [Paso 3: Configurar la directiva de control de acceso condicional en función de los datos de usuario](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_3)

-   [Paso 4: Comprobar el mecanismo de control de acceso condicional](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_4)

## <a name="BKMK_1"></a>Paso 1: Configurar el entorno de laboratorio
Para completar este tutorial, necesitas un entorno que consta de los siguientes componentes:

-   Un dominio de Active Directory con un usuario de prueba y cuentas de grupo, la ejecución en Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 con su esquema actualizado a Windows Server 2012 R2 o a un dominio de Active Directory que se ejecutan en Windows Server 2012 R2

-   Un servidor de federación que se ejecuta en Windows Server 2012 R2

-   Un servidor web que hospeda la aplicación de muestra

-   Un equipo cliente desde el que puede obtener acceso a la aplicación de ejemplo

> [!WARNING]
> (Tanto en entornos de prueba o producción) se recomienda encarecidamente que no uses el mismo equipo que el servidor de federación y el servidor web.

En este entorno, el servidor de federación problemas de las notificaciones que son necesarios para que los usuarios pueden acceder a la aplicación de ejemplo. El servidor Web hospede una aplicación de muestra que confíe en los usuarios que presentan las notificaciones que los problemas de servidor de federación.

Para obtener instrucciones sobre cómo configurar este entorno, consulta [configurar el entorno de laboratorio de AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

## <a name="BKMK_2"></a>Paso 2: Comprobar el mecanismo de control de acceso predeterminada AD FS
En este paso se comprueba el valor predeterminado de mecanismo de control de acceso de AD FS, donde el usuario se redirige a la página de inicio de sesión de AD FS, proporciona las credenciales válidas y se concede acceso a la aplicación. Puedes usar la **Robert Hatley** cuenta de AD y la **claimapp** muestra la aplicación que has configurado en [configurar el entorno de laboratorio de AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

#### <a name="to-verify-the-default-ad-fs-access-control-mechanism"></a>Para comprobar el valor predeterminado AD FS mecanismo de control de acceso a

1.  En el equipo cliente, abre una ventana del explorador y navegar a la aplicación de muestra: **https://webserv1.contoso.com/claimapp**.

    Esta acción redirige automáticamente la solicitud al servidor de federación y se le pide que inicies sesión con un nombre de usuario y una contraseña.

2.  Escribe las credenciales de la **Robert Hatley** cuenta de AD que creaste en [configurar el entorno de laboratorio de AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Tendrán acceso a la aplicación.

## <a name="BKMK_3"></a>Paso 3: Configurar la directiva de control de acceso condicional en función de los datos de usuario
En este paso se establece una directiva de control de acceso en función de los datos de pertenencia al grupo de usuario. En otras palabras, configurarás un **regla de autorización de emisión** en el servidor de federación para un usuario de confianza confianza terceros que representa tu aplicación de muestra: **claimapp**. La lógica de la regla, **Robert Hatley** usuario AD se emitirá notificaciones que requieren acceso a esta aplicación ya pertenece a un **finanzas** grupo. Has agregado la **Robert Hatley** cuenta a la **finanzas** grupo [configurar el entorno de laboratorio de AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

Puedes completar esta tarea con cualquier consola de administración de AD FS o a través de Windows PowerShell.

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-the-ad-fs-management-console"></a>Para configurar la directiva de control de acceso condicional en función de los datos de usuario a través de la consola de administración de AD FS

1.  En la consola de administración de AD FS, desplácese hasta **relaciones de confianza**y luego **confiar confía en las partes**.

2.  Selecciona la confianza de terceros confianza que representa la aplicación de muestra (**claimapp**) y, a continuación, ya sea en el **acciones** panel o haciendo clic en esta confianza de terceros de confianza, seleccione **editar reglas de notificación**.

3.  En la **editar reglas de notificación para claimapp** , selecciona **las reglas de autorización de emisión** pestaña y haz clic en **Agregar regla**.

4.  En la **emisión autorización reclamar regla Asistente para agregar**, en la **página Seleccionar plantilla de regla**, selecciona **permitir o denegar a los usuarios en función de una notificación entrante** reclama la plantilla de regla y, a continuación, haz clic en **siguiente**.

5.  En la **configurar regla** página, realiza lo siguiente y, a continuación, haz clic en **finalizar**:

    1.  Escribe un nombre para la regla de notificación, por ejemplo **TestRule**.

    2.  Selecciona **SID del grupo** como **tipo de notificación entrante**.

    3.  Haz clic en **examinar**, escribe en **finanzas** para el nombre de tu anuncio probar grupo y a resolver el **el valor de notificación entrante** campo.

    4.  Selecciona el **denegar el acceso a los usuarios con esta notificación entrante** opción.

6.  En la **editar reglas de notificación para claimapp** ventana, asegúrate de eliminar el **permitir acceso a todos los usuarios** regla que se crea de forma predeterminada cuando creaste esta confianza de terceros de confianza.

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-windows-powershell"></a>Para configurar la directiva de control de acceso condicional en función de los datos de usuario a través de Windows PowerShell

1.  En el servidor de federación, abre la ventana de comandos de Windows PowerShell y ejecuta el siguiente comando:


    `$rp = Get-AdfsRelyingPartyTrust -Name claimapp`


2.  En la misma ventana de comandos de Windows PowerShell, ejecuta el siguiente comando:


    `$GroupAuthzRule = '@RuleTemplate = "Authorization" @RuleName = "Foo" c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i)<group_SID>$"] =>issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");'
    Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -IssuanceAuthorizationRules $GroupAuthzRule`

> [!NOTE]
> Asegúrate de que reemplazar < group_SID > con el valor del SID de tu anuncio **finanzas** grupo.

## <a name="BKMK_4"></a>Paso 4: Comprobar el mecanismo de control de acceso condicional
En este paso se comprueba la directiva de control de acceso condicional que configuraste en el paso anterior. Puedes usar el siguiente procedimiento para comprobar que **Robert Hatley** usuario AD puede acceder a la aplicación de muestra porque pertenece a la **finanzas** grupo y los usuarios de anuncios que no pertenecen a la **finanzas** grupo no puede acceder a la aplicación de ejemplo.

1.  En el equipo cliente, abre una ventana del explorador y navegar a la aplicación de muestra: **https://webserv1.contoso.com/claimapp**

    Esta acción redirige automáticamente la solicitud al servidor de federación y se le pide que inicies sesión con un nombre de usuario y una contraseña.

2.  Escribe las credenciales de la **Robert Hatley** cuenta de AD que creaste en [configurar el entorno de laboratorio de AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Tendrán acceso a la aplicación.

3.  Escribe las credenciales de otro usuario de anuncios que no pertenece a la **finanzas** grupo. (Para obtener más información sobre cómo crear cuentas de usuario en Active Directory, consulte [https://technet.microsoft.com/library/cc7833232.aspx](https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx).

    En este punto, debido a la directiva de control de acceso que configuraste en el paso anterior, se muestra un mensaje de "acceso denegado" para este usuario de anuncios que no pertenece a la **finanzas** grupo. El texto del mensaje predeterminado es **no está autorizado para acceder a este sitio. Haz clic aquí para cerrar sesión y vuelve a iniciarla o ponte en contacto con el Administrador de permisos.** Sin embargo, este texto es totalmente personalizable. Para obtener más información sobre cómo personalizar la experiencia de inicio de sesión, consulta [personalizar las páginas AD FS Sign-in](https://technet.microsoft.com/library/dn280950.aspx).

## <a name="see-also"></a>Consulta también
[Administrar el riesgo con el Control de acceso condicional](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)
[configurar el entorno de laboratorio de AD FS en Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



