---
ms.assetid: 5fd4063d-34dc-4b15-9a88-cc6c1fff455a
title: 'Guía de tutorial: administración de riesgos con autenticación multifactor adicional para aplicaciones confidenciales'
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: bd21f2d6e8dcb167aa2c614d096807305a7728d6
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188888"
---
# <a name="walkthrough-guide-manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>Guía de tutorial: administración de riesgos con la autenticación multifactor adicional para aplicaciones confidenciales




## <a name="about-this-guide"></a>Acerca de esta guía
Este tutorial proporciona instrucciones para configurar la autenticación multifactor (MFA) en servicios de federación de Active Directory (AD FS) en Windows Server 2012 R2 se basan en datos de pertenencia a grupos del usuario.

Para obtener más información acerca de los mecanismos de autenticación y MFA en AD FS, consulte [administración de riesgos con autenticación multifactor adicional para aplicaciones confidenciales](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).

Este tutorial incluye las secciones siguientes:

-   [Paso 1: Configurar el entorno de laboratorio](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Paso 2: Comprobar el mecanismo de autenticación de AD FS predeterminado](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2)

-   [Paso 3: Configurar MFA en el servidor de federación](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_3)

-   [Paso 4: Comprobar el mecanismo de MFA](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_4)

## <a name="BKMK_1"></a>Paso 1: Configuración del entorno de laboratorio
Para completar este tutorial, necesita un entorno con los siguientes componentes:

-   Un dominio de Active Directory con un usuario de prueba y cuentas de grupo que se ejecutan en Windows Server 2012 R2 o un dominio de Active Directory que se ejecutan en Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 con el esquema actualizado a Windows Server 2012 R2

-   Un servidor de federación que se ejecutan en Windows Server 2012 R2

-   Un servidor web que hospede la aplicación de ejemplo

-   Un equipo cliente desde el que pueda obtener acceso a la aplicación de ejemplo

> [!WARNING]
> Se recomienda (tanto en entornos de producción como de prueba) no usar el mismo equipo como servidor de federación y servidor web.

En este entorno, el servidor de federación emite las notificaciones necesarias para que los usuarios puedan tener acceso a la aplicación de ejemplo. El servidor web hospeda una aplicación de ejemplo que confiará en los usuarios que presenten las notificaciones emitidas por el servidor de federación.

Para obtener instrucciones sobre cómo configurar este entorno, consulte [configurar el entorno de laboratorio para AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

## <a name="BKMK_2"></a>Paso 2: Comprobar el mecanismo de autenticación de AD FS predeterminado
En este paso comprobará el mecanismo de control de acceso predeterminado de AD FS (**Autenticación mediante formularios** para la extranet y **Autenticación de Windows** para la intranet), en el que se redirige al usuario a la página de inicio de sesión de AD FS, el usuario proporciona credenciales válidas y se le concede acceso a la aplicación. Puede usar el **Robert Hatley** cuenta de AD y el **claimapp** aplicación de ejemplo que configuró en [configurar el entorno de laboratorio para AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

1.  En el equipo cliente, abra una ventana del explorador y vaya a la aplicación de ejemplo: **https://webserv1.contoso.com/claimapp**.

    Esta acción redirige automáticamente la solicitud al servidor de federación y se le solicita que inicie sesión con un nombre de usuario y una contraseña.

2.  Escriba las credenciales de la **Robert Hatley** cuenta de AD que creó en [configurar el entorno de laboratorio para AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Se le concederá acceso a la aplicación.

## <a name="BKMK_3"></a>Paso 3: Configurar MFA en el servidor de federación
Hay dos partes para configurar MFA en AD FS en Windows Server 2012 R2:

-   [Seleccione un método de autenticación adicionales](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_5)

-   [Configuración de directiva de MFA](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_6)

### <a name="BKMK_5"></a>Seleccione un método de autenticación adicionales
Para configurar MFA debe seleccionar un método de autenticación adicional. En este tutorial, para el método de autenticación adicional, puede elegir entre las siguientes opciones:

-   Seleccione [autenticación de certificado](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_7) método que está disponible en AD FS en Windows Server 2012 R2 de forma predeterminada

-   Configure y seleccione [Windows Azure Multi-Factor Authentication](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_8)

#### <a name="BKMK_7"></a>Autenticación de certificado
Complete uno de los dos procedimientos siguientes para seleccionar la autenticación de certificado como método de autenticación adicional:

###### <a name="to-configure-certificate-authentication-as-an-additional-authentication-method-via-the-ad-fs-management-console"></a>Para configurar la autenticación de certificado como método de autenticación adicional mediante la Consola de administración de AD FS

1.  En la Consola de administración de AD FS del servidor de federación, navegue hasta el nodo **Directivas de autenticación** y, en la sección **Autenticación multifactor** , haga clic en el vínculo **Editar** junto al subapartado **Configuración global** .

2.  En la ventana **Editar directiva de autenticación global** , seleccione **Autenticación de certificado** como método de autenticación adicional y, a continuación, haga clic en **Aceptar**.

###### <a name="to-configure-certificate-authentication-as-an-additional-authentication-method-via-windows-powershell"></a>Para configurar la autenticación de certificado como método de autenticación adicional mediante la Windows PowerShell

1.  En el servidor de federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando:

    ```
    Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider CertificateAuthentication

    ```

    > [!WARNING]
    > Para comprobar si este comando se ejecutó correctamente, puede ejecutar el comando `Get-AdfsGlobalAuthenticationPolicy` .

#### <a name="BKMK_8"></a>Windows Azure Multi-factor Authentication
Complete los siguientes procedimientos para descargar, configurar y seleccionar **Autenticación multifactor de Windows Azure** como autenticación adicional en el servidor de federación:

1.  [Crear un proveedor de autenticación multifactor a través de las ventanas de Azure Portal](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_a)

2.  [Descargar el servidor de Windows Azure Multi-factor Authentication](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_b)

3.  [Instalar al servidor de Windows Azure Multi-factor Authentication en el servidor de federación](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_c)

4.  [Configurar la autenticación multifactor de Windows Azure como método de autenticación adicional](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_d)

##### <a name="BKMK_a"></a>Crear un proveedor de autenticación multifactor a través de las ventanas de Azure Portal

1.  Inicie sesión en Microsoft Azure Portal como administrador.

2.  En la izquierda, seleccione Active Directory.

3.  En la parte superior de la página Active Directory, seleccione **Proveedores de autenticación multifactor**.  A continuación, en la parte inferior, haga clic en **Nuevo**.

4.  En **Servicios de aplicaciones->Active Directory**, seleccione **Proveedor de autenticación multifactor** y seleccione **Creación rápida**.

5.  En **Servicios de aplicaciones**, seleccione **Proveedores de autenticación activos**y seleccione **Creación rápida**.

6.  Rellene los campos siguientes y seleccione **Crear**.

    1.  **Nombre** -el nombre del proveedor de Multi-factor Authentication.

    2.  **Modelo de uso** -el modelo de uso del proveedor de autenticación multifactor.

        -   **Por autenticación** -modelo de compra con cargo por autenticación. Se suele utilizar para escenarios en los que se usa la autenticación multifactor de Windows Azure en una aplicación de consumo.

        -   **Por usuario habilitado** -modelo de compra con cargo por usuario habilitado.  Se suele usar para escenarios de uso por parte de empleados, como Office 365.

        Para obtener más información acerca de los modelos de uso, consulte [Información de precios de Windows Azure](http://www.windowsazure.com/pricing/details/active-directory/).

    3.  **Directorio** -inquilino Windows Azure Active Directory que está asociado el proveedor de autenticación multifactor. Este dato es opcional, ya que no es necesario que el proveedor esté vinculado con Active Directory de Windows Azure al proteger aplicaciones locales.

7.  Una vez que haga clic en Crear, se creará el proveedor de autenticación multifactor y debería mostrarse un mensaje que indica  que se ha creado correctamente el proveedor de autenticación multifactor.  Haga clic en **Aceptar**.

A continuación, debe descargar el Servidor de autenticación multifactor de Windows Azure. Para ello, puede iniciar el portal de autenticación multifactor de Windows Azure a través del portal de Windows Azure.

##### <a name="BKMK_b"></a>Descargar el servidor de Windows Azure Multi-factor Authentication

1.  Inicie sesión en el portal de Windows Azure como administrador y haga clic en el proveedor de autenticación multifactor que creó en el procedimiento anterior. A continuación, haga clic en el botón **Administrar** .

    Se inicia el portal de **Autenticación multifactor de Windows Azure**.

2.  En el portal de **Autenticación multifactor de Windows Azure** , haga clic en **Descargas**y, a continuación, haga clic en **Descargar** para descargar una copia del Servidor de autenticación multifactor de Windows Azure.

En cuanto haya descargado el archivo ejecutable para el Servidor de autenticación multifactor de Windows Azure, debe instalarlo en el servidor de federación.

##### <a name="BKMK_c"></a>Instalar al servidor de Windows Azure Multi-factor Authentication en el servidor de federación

1.  Descargue el archivo ejecutable del Servidor de autenticación multifactor de Windows Azure y haga doble clic en él.  Se iniciará la instalación.

2.  En la pantalla del contrato de licencia, lea el contrato, seleccione **Acepto** y haga clic en **Siguiente**.

3.  Asegúrese de que la carpeta de destino sea correcta y haga clic en **Siguiente**.

4.  Cuando la instalación se haya completado, haga clic en **Finalizar**.

Ahora ya puede iniciar el Servidor de autenticación multifactor de Windows Azure que ha instalado en el servidor de federación para configurarlo como método de autenticación adicional.

##### <a name="BKMK_d"></a>Configurar la autenticación multifactor de Windows Azure como método de autenticación adicional

1.  Inicie **Autenticación multifactor de Windows Azure** desde su ubicación de instalación en el servidor de federación y, en la página principal, active la casilla **Omitir el asistente de configuración de autenticación** y haga clic en **Siguiente**.

2.  Para activar el Servidor de autenticación multifactor, vuelva a la página del portal de administración de autenticación multifactor desde donde lo descargó y haga clic en el botón **Generar credenciales de activación** . En la interfaz de usuario del Servidor de autenticación multifactor, indique las credenciales que se generaron y haga clic en **Activar**.

3.  A continuación, la interfaz de usuario del **Servidor de autenticación multifactor** le solicitará que ejecute el **Asistente de configuración multiservidor**.  Seleccione **No**.

    > [!IMPORTANT]
    > Puede omitir el **Asistente de configuración multiservidor**, dado el entorno de laboratorio con tan solo un servidor de federación que se usa para completar este tutorial. No obstante, si su entorno contiene varios servidores de federación, debe instalar el Servidor de autenticación multifactor y completar el **Asistente de configuración multiservidor** en cada servidor para habilitar la replicación entre los servidores multifactor que se ejecuten en sus servidores de federación.

4.  En la interfaz de usuario del **Servidor de autenticación multifactor** , seleccione el icono **Usuarios** , haga clic en **Importar desde Active Directory**, seleccione la cuenta **Robert Hatley** para aprovisionarla en la autenticación multifactor de Windows Azure y, a continuación, haga clic en **Importar**.

5.  En la lista **Usuarios** , seleccione la cuenta **Robert Hatley** , haga clic en **Editar**y, en la ventana **Editar usuario** , proporcione un número de teléfono móvil para esta cuenta, asegúrese de que la casilla **Habilitado** esté activada y, a continuación, haga clic en **Aplicar**.

6.  En la lista **Usuarios** , seleccione la cuenta **Robert Hatley** y haga clic en **Prueba**. En la ventana **Usuario de prueba** , proporcione las credenciales de la cuenta de **Robert Hatley** . Cuando suene el teléfono móvil, presione "#" para completar la comprobación de la cuenta.

7.  En la interfaz de usuario del **Servidor de autenticación multifactor** , seleccione el icono de **AD FS** , asegúrese de que las casillas **Permitir inscripción de usuarios**, **Permitir a los usuarios seleccionar el método** (incluidos los métodos **Llamada telefónica** y **Mensaje de texto**), **Usar preguntas de seguridad para reserva** y **Habilitar registro** estén activadas, haga clic en **Instalar adaptador de AD FS**y complete el asistente de instalación del **Adaptador de AD FS para autenticación multifactor** .

    > [!NOTE]
    > El asistente de instalación del **Adaptador de AD FS para autenticación multifactor** crea un grupo de seguridad denominado **PhoneFactor Admins** en Active Directory y, a continuación, agrega la cuenta del servicio AD FS del servicio de federación a este grupo.
    > 
    > Se recomienda comprobar en el controlador de dominio que el grupo **PhoneFactor Admins** se haya creado y que la cuenta del servicio AD FS forme parte de este grupo.
    > 
    > Si es necesario, agregue la cuenta del servicio AD FS al grupo **PhoneFactor Admins** del controlador de dominio manualmente.

    Para obtener más detalles acerca de la instalación del adaptador de AD FS, haga clic en el vínculo de ayuda ubicado en la esquina superior derecha del Servidor de autenticación multifactor.

8.  Para registrar el adaptador en el servicio de federación, en el servidor de federación, inicie la ventana de comandos de Windows PowerShell y ejecute el siguiente comando: `\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`. El adaptador se registra como **WindowsAzureMultiFactorAuthentication**.  Debe reiniciar el servicio AD FS para que el registro surta efecto.

9. Para configurar la autenticación multifactor de Windows Azure como método de autenticación adicional, en la Consola de administración de AD FS, navegue hasta el nodo **Directivas de autenticación** y, en la sección **Autenticación multifactor**, haga clic en el vínculo **Editar** junto al subapartado **Configuración global**. En la ventana **Editar directiva de autenticación global** , seleccione **Autenticación multifactor** como método de autenticación adicional y, a continuación, haga clic en **Aceptar**.

    > [!NOTE]
    > Para personalizar el nombre y la descripción del método de autenticación multifactor de Windows Azure, así como de cualquier método de autenticación de terceros, tal como aparecen en la interfaz de usuario de AD FS, ejecute el cmdlet **Set-AdfsAuthenticationProviderWebContent**. Para obtener más información, vea [https://technet.microsoft.com/library/dn479401.aspx](https://technet.microsoft.com/library/dn479401.aspx)

### <a name="BKMK_6"></a>Configuración de directiva de MFA
Para habilitar MFA, debe configurar la directiva de MFA en el servidor de federación. Para este tutorial, según nuestra directiva de MFA, **Robert Hatley** cuenta es necesaria para someter a MFA porque pertenece a la **Finance** grupo que configuró en [configurar el entorno de laboratorio para AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

Puede configurar la directiva de MFA mediante la Consola de administración de AD FS o con Windows PowerShell.

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-the-ad-fs-management-console"></a>Para configurar la directiva MFA basándose en datos de pertenencia a grupos del usuario para "claimapp" mediante la consola de administración de AD FS

1.  En la Consola de administración de AD FS del servidor de federación, navegue hasta el nodo **Directivas de autenticación**\\**Por relación de confianza para usuario autenticado** y seleccione la relación de confianza para usuario autenticado que representa la aplicación de ejemplo (**claimapp**).

2.  En la página **Acciones** o haciendo clic con el botón secundario en **claimapp**, seleccione **Editar autenticación multifactor personalizada**.

3.  En la ventana **Editar relación de confianza para usuario autenticado para claimapp**, haga clic en el botón **Agregar** junto a la lista **Usuarios/Grupos**. Escriba en **Finance** para el nombre del grupo de AD que creó en [configurar el entorno de laboratorio para AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)y haga clic en **comprobar nombres**, y cuando el nombre es puede resolver, haga clic en **Aceptar**.

4.  Haga clic en **Aceptar** en la ventana **Editar relación de confianza para usuario autenticado para claimapp**.

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-windows-powershell"></a>Para configurar la directiva MFA basándose en datos de pertenencia a grupos del usuario para "claimapp" mediante Windows PowerShell

1.  En el servidor de federación, abra la ventana de comandos de Windows PowerShell y ejecute el siguiente comando:

    ```
    $rp = Get-AdfsRelyingPartyTrust -Name claimapp
    ```

2.  En la misma ventana de Windows PowerShell, ejecute el siguiente comando:

    ```
    $GroupMfaClaimTriggerRule = 'c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i) <group_SID>$"] => issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
    Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -AdditionalAuthenticationRules $GroupMfaClaimTriggerRule

    ```

    > [!NOTE]
    > Asegúrese de reemplazar <group_SID> con el valor del SID de grupo de AD **Finanzas**.

## <a name="BKMK_4"></a>Paso 4: Comprobar el mecanismo de MFA
En este paso comprobará la funcionalidad de MFA que configuró en el paso anterior. Puede usar el procedimiento siguiente para comprobar que el usuario de AD de **Robert Hatley** puede tener acceso a la aplicación de ejemplo y que debe someterse a MFA porque pertenece al grupo **Finanzas** .

1.  En el equipo cliente, abra una ventana del explorador y vaya a la aplicación de ejemplo: **https://webserv1.contoso.com/claimapp**.

    Esta acción redirige automáticamente la solicitud al servidor de federación y se le solicita que inicie sesión con un nombre de usuario y una contraseña.

2.  Escriba las credenciales de la cuenta de AD de **Robert Hatley** .

    En este punto, a causa de la directiva de MFA que configuró, se solicitará al usuario que realice una autenticación adicional. El mensaje predeterminado es **Por razones de seguridad, necesitamos más información para comprobar su cuenta.** No obstante, este texto se puede personalizar. Para obtener más información sobre cómo personalizar la experiencia de inicio de sesión, consulte [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).

    Si configuró la autenticación de certificado como método de autenticación adicional, el texto del mensaje predeterminado es **seleccionar un certificado que desea usar para la autenticación. Si cancela la operación, cierre el explorador y vuelva a intentarlo.**

    Si configura la autenticación multifactor de Windows Azure como método de autenticación adicional, el texto del mensaje predeterminado será **Se realizará una llamada a su teléfono para completar la autenticación.** Para obtener más información acerca del inicio de sesión con Autenticación multifactor de Windows Azure y con distintas opciones para el método de comprobación preferido, consulte [Introducción a la autenticación multifactor de Windows Azure](https://technet.microsoft.com/library/dn249479.aspx).

## <a name="see-also"></a>Vea también
[Administración de riesgos con autenticación multifactor adicional para aplicaciones confidenciales](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
[configurar el entorno de laboratorio para AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



