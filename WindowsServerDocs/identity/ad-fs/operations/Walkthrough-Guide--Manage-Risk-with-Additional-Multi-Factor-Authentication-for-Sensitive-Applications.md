---
ms.assetid: 5fd4063d-34dc-4b15-9a88-cc6c1fff455a
title: "Guía paso a paso: administrar el riesgo con la autenticación multifactor adicional para aplicaciones con información confidencial"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 414f37e86f0072863e5fa2f107c39e5518e560ec
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="walkthrough-guide-manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>Guía paso a paso: Administrar el riesgo con la autenticación multifactor adicional para aplicaciones con información confidencial

>Se aplica a: Windows Server 2012 R2


## <a name="about-this-guide"></a>Acerca de esta guía
Este tutorial proporciona instrucciones para configurar la autenticación multifactor (AMF) en servicios de federación de Active Directory (AD FS) en Windows Server 2012 R2 en función de los datos de pertenencia a grupo del usuario.

Para obtener más información acerca de los mecanismos de autenticación y MFA en AD FS, consulta [administrar riesgo con la autenticación multifactor adicional para aplicaciones con información confidencial](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).

Este tutorial consta de las siguientes secciones:

-   [Paso 1: Configurar el entorno de laboratorio](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Paso 2: Comprobar el mecanismo de autenticación de AD FS predeterminado](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2)

-   [Paso 3: Configurar MFA en el servidor de federación](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_3)

-   [Paso 4: Comprobar mecanismo AMF](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_4)

## <a name="BKMK_1"></a>Paso 1: Configurar el entorno de laboratorio
Para completar este tutorial, necesitas un entorno que consta de los siguientes componentes:

-   Un dominio de Active Directory con un usuario de prueba y cuentas de grupo que se ejecutan en Windows Server 2012 R2 o en un dominio de Active Directory que se ejecutan en Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 con su esquema actualizado a Windows Server 2012 R2

-   Un servidor de federación que se ejecuta en Windows Server 2012 R2

-   Un servidor web que hospeda la aplicación de muestra

-   Un equipo cliente desde el que puede obtener acceso a la aplicación de ejemplo

> [!WARNING]
> (Tanto en los entornos de prueba y producción) se recomienda encarecidamente que no uses el mismo equipo que el servidor de federación y el servidor web.

En este entorno, el servidor de federación problemas de las notificaciones que son necesarios para que los usuarios pueden acceder a la aplicación de ejemplo. El servidor Web hospede una aplicación de muestra que confíe en los usuarios que presentan las notificaciones que los problemas de servidor de federación.

Para obtener instrucciones sobre cómo configurar este entorno, consulta [configurar el entorno de laboratorio de AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

## <a name="BKMK_2"></a>Paso 2: Comprobar el mecanismo de autenticación de AD FS predeterminado
En este paso comprobará el mecanismo de control de acceso predeterminada AD FS (**autenticación de formularios** para extranet y **autenticación de Windows** de intranet), donde el usuario se redirige a la página de inicio de sesión de AD FS, proporciona las credenciales válidas y se concede acceso a la aplicación. Puedes usar la **Robert Hatley** cuenta de AD y la **claimapp** muestra la aplicación que has configurado en [configurar el entorno de laboratorio de AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

1.  En el equipo cliente, abre una ventana del explorador y navegar a la aplicación de muestra: **https://webserv1.contoso.com/claimapp**.

    Esta acción redirige automáticamente la solicitud al servidor de federación y se le pide que inicies sesión con un nombre de usuario y una contraseña.

2.  Escribe las credenciales de la **Robert Hatley** cuenta de AD que creaste en [configurar el entorno de laboratorio de AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

    Tendrán acceso a la aplicación.

## <a name="BKMK_3"></a>Paso 3: Configurar MFA en el servidor de federación
Hay dos partes a la configuración de MFA de AD FS en Windows Server 2012 R2:

-   [Selecciona un método de autenticación adicional](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_5)

-   [Configurar la directiva de MFA](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_6)

### <a name="BKMK_5"></a>Selecciona un método de autenticación adicional
Para poder configurar MFA, debes seleccionar un método de autenticación adicional. En este tutorial, para el método de autenticación adicionales, puedes elegir entre las siguientes opciones:

-   Selecciona [autenticación de certificado](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_7) método que está disponible en AD FS en Windows Server 2012 R2, de manera predeterminada

-   Configurar y seleccionar [Windows Azure la autenticación multifactor](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_8)

#### <a name="BKMK_7"></a>Autenticación de certificado
Realiza cualquiera de los siguientes procedimientos para seleccionar la autenticación de certificado como método de autenticación adicionales:

###### <a name="to-configure-certificate-authentication-as-an-additional-authentication-method-via-the-ad-fs-management-console"></a>Para configurar la autenticación de certificado como un método de autenticación adicional a través de la consola de administración de AD FS

1.  En el servidor de federación, en la consola de administración de AD FS, ve a la **directivas de autenticación** nodo y, en **la autenticación multifactor** sección, haz clic en el **editar** vincular junto a la **configuración Global** subsección.

2.  En la **Editar directiva de autenticación Global** , selecciona **autenticación de certificado** como método de autenticación adicional y, a continuación, haz clic en **Aceptar**.

###### <a name="to-configure-certificate-authentication-as-an-additional-authentication-method-via-windows-powershell"></a>Para configurar la autenticación de certificado como un método de autenticación adicional a través de Windows PowerShell

1.  En el servidor de federación, abre la ventana de comandos de Windows PowerShell y ejecuta el siguiente comando:

    ```
    Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider CertificateAuthentication

    ```

    > [!WARNING]
    > Para comprobar que este comando se ejecutó correctamente, puedes ejecutar el `Get-AdfsGlobalAuthenticationPolicy` comando.

#### <a name="BKMK_8"></a>Windows Azure la autenticación multifactor
Completar los siguientes procedimientos para descargar y configurar y selecciona **Windows Azure la autenticación multifactor** como autenticación adicional en el servidor de federación:

1.  [Crear un proveedor de autenticación multifactor a través de las ventanas Portal de Azure](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_a)

2.  [Descargar el servidor de Windows Azure la autenticación multifactor](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_b)

3.  [Instalar al servidor de Windows Azure la autenticación multifactor en el servidor de federación](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_c)

4.  [Configurar la autenticación multifactor de Azure de Windows como método de autenticación adicionales](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_d)

##### <a name="BKMK_a"></a>Crear un proveedor de autenticación multifactor a través de las ventanas Portal de Azure

1.  Iniciar sesión en Windows Azure Portal como administrador.

2.  En el lado izquierdo, selecciona Active Directory.

3.  En la página de Active Directory, en la parte superior, selecciona **proveedores de autenticación multifactor**.  A continuación, haz clic en la parte inferior, **nueva**.

4.  En **servicios de la aplicación -> Active Directory**, selecciona **proveedor de autenticación multifactor**y selecciona **crear rápido**.

5.  En **servicios de la aplicación**, selecciona **activa los proveedores de autenticación**y selecciona **crear rápido**.

6.  Relleno en los siguientes campos y selecciona **crear**.

    1.  **Nombre** -el nombre del proveedor de autenticación multifactor.

    2.  **Modelo de uso** -el modelo de uso del proveedor de autenticación multifactor.

        -   **Por autenticación** -modelo de compra que cobra por la autenticación. Suele usarse para escenarios que usan la autenticación multifactor de Azure de Windows en una aplicación de muestra al consumidor.

        -   **Cada usuario habilitado** -modelo de compra que cobra por usuario habilitado.  Suele usarse para escenarios de orientación de los empleados, como Office 365.

        Para obtener más información en los modelos de uso, consulta [detalles sobre los precios de Windows Azure](http://www.windowsazure.com/pricing/details/active-directory/).

    3.  **Directorio** -inquilino de The Windows Azure Active Directory que está asociado el proveedor de autenticación multifactor. Esto es opcional, como el proveedor no tiene que esté vinculada a Windows Azure Active Directory cuando la protección de aplicaciones locales.

7.  Una vez que pulses crear, se creará el proveedor de autenticación multifactor y deberías ver un mensaje que indique: proveedor de autenticación multifactor se creó correctamente.  Haz clic en **Aceptar**.

A continuación, debes descargar el servidor de autenticación multifactor de Windows Azure. Para ello, iniciando el Portal de autenticación multifactor de Windows Azure a través del portal de Windows Azure.

##### <a name="BKMK_b"></a>Descargar el servidor de Windows Azure la autenticación multifactor

1.  Iniciar sesión en el Portal de Windows Azure como administrador y haz clic en el proveedor de autenticación multifactor que creaste en el procedimiento anterior. A continuación, haz clic en el **administrar** botón.

    Se inicia el **Windows Azure la autenticación multifactor** portal.

2.  En el **Windows Azure la autenticación multifactor** portal, haz clic en **descargas**y, a continuación, haz clic en **descargar** para descargar una copia del servidor de autenticación multifactor de Azure de Windows.

Una vez que hayas descargado el archivo ejecutable del servidor de autenticación multifactor de Windows Azure, se debe instalar en el servidor de federación.

##### <a name="BKMK_c"></a>Instalar al servidor de Windows Azure la autenticación multifactor en el servidor de federación

1.  Descargar y haz doble clic en el archivo ejecutable del servidor de autenticación multifactor de Windows Azure.  Esto iniciará la instalación.

2.  En la pantalla del contrato de licencia, lea el contrato, selecciona **acepto** y haz clic en **siguiente**.

3.  Asegúrate de que la carpeta de destino es correcta y haz clic en **siguiente**.

4.  Una vez que se complete la instalación, haz clic en **finalizar**.

Ya estás listo para iniciar el servidor de autenticación multifactor de Azure de Windows que instalaste en el servidor de federación y configurarla como un método de autenticación adicional.

##### <a name="BKMK_d"></a>Configurar la autenticación multifactor de Azure de Windows como método de autenticación adicionales

1.  Iniciar **Windows Azure la autenticación multifactor** desde que se instaló en el servidor de federación y en la página de bienvenida, comprobar la **omitir mediante el Asistente para configuración de autenticación** casilla de verificación y haz clic en **siguiente**.

2.  Para activar el servidor de autenticación multifactor, ve a la página en el portal de administración de la autenticación multifactor donde se descargará el servidor de autenticación multifactor y haz clic en el **generar credenciales de activación** botón. En la interfaz de usuario de servidor de autenticación multifactor, escribe las credenciales que se generaron y haz clic en **activar**.

3.  A continuación, el **servidor de autenticación multifactor** interfaz de usuario se le pide que ejecute el **Asistente para configuración del servidor de múltiples**.  Selecciona **No**.

    > [!IMPORTANT]
    > Puedes omitir completar la **Asistente para configuración del servidor de múltiples** dado el entorno de laboratorio con el servidor de federación de un único que se usa para completar este tutorial. Sin embargo, si el entorno contiene varios servidores de federación, debes instalar el servidor de autenticación multifactor y completar la **Asistente para configuración del servidor de múltiples** en cada servidor de federación con el fin de habilitar la replicación entre los servidores de varios factores que se ejecutan en los servidores de federación.

4.  En la **servidor de autenticación multifactor** interfaz de usuario, selecciona el **usuarios** icono, haz clic en **importar de Active Directory**, selecciona el **Robert Hatley** cuenta para aprovisionar en Windows Azure la autenticación multifactor y, a continuación, haz clic en **importar**.

5.  En la **a los usuarios** lista, selecciona el **Robert Hatley** cuenta y, a continuación, haz clic en **editar**y en la **usuario editar** ventana, proporcionar un número de teléfono móvil de esta cuenta, asegúrate de que la **habilitado** casilla está activada y, a continuación, haz clic en **aplicar**.

6.  En la **usuarios** lista, selecciona el **Robert Hatley** cuenta y haz clic en **prueba**. En la **usuario prueba** ventana, proporcionar las credenciales para la **Robert Hatley** cuenta. Cuando se presionan los anillos del teléfono móvil, '#' para completar la verificación de cuenta.

7.  En la **servidor de autenticación multifactor** interfaz de usuario, selecciona el **AD FS** icono, asegúrate de que **inscripción de usuario de permitir**, **permitir al usuario seleccionar el método** (incluidos **llamada telefónica** y **mensaje de texto**), **usa preguntas de seguridad para la reserva** y **habilitar el registro** se comprueban las casillas de verificación, haz clic en **instalar AD FS adaptador**y completar la **la autenticación multifactor AD FS adaptador** Asistente para la instalación.

    > [!NOTE]
    > La **la autenticación multifactor AD FS adaptador** Asistente para la instalación crea un grupo de seguridad denominado **PhoneFactor Admins** en Active Directory y, a continuación, agrega la cuenta de servicio de AD FS de los servicios de federación a este grupo.
    > 
    > Se recomienda que compruebes en el controlador de dominio que la **PhoneFactor Admins** efectivamente se creó el grupo y que la de AD FS cuenta de servicio es un miembro de este grupo.
    > 
    > Si es necesario, agrega la cuenta de servicio de AD FS a la **PhoneFactor Admins** agrupar manualmente en el controlador de dominio.

    Para obtener más información sobre cómo instalar al adaptador de AD FS, haz clic en el vínculo de ayuda en la esquina superior derecha del servidor de autenticación multifactor.

8.  Para registrar el adaptador en el servicio de federación, en el servidor de federación, iniciar la ventana de comandos de Windows PowerShell y ejecuta el siguiente comando: `\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`. El adaptador está ahora registrado como **WindowsAzureMultiFactorAuthentication**.  Debes reiniciar el servicio de AD FS de registro surta efecto.

9. Para configurar la autenticación multifactor de Azure de Windows como método de autenticación adicional, en la consola de administración de AD FS, navega hasta la **directivas de autenticación** nodo y, en **la autenticación multifactor** sección, haz clic en el **editar** vincular junto a la **configuración Global** subsección. En la **Editar directiva de autenticación Global** , selecciona **la autenticación multifactor** como método de autenticación adicional y, a continuación, haz clic en **Aceptar**.

    > [!NOTE]
    > Puedes personalizar el nombre y descripción del método de autenticación multifactor de Azure de Windows, así como cualquier configurado el método de autenticación de terceros, tal y como aparece en la interfaz de usuario de AD FS, mediante la ejecución de la **conjunto AdfsAuthenticationProviderWebContent** cmdlet. Para obtener más información, consulta [https://technet.microsoft.com/library/dn479401.aspx](https://technet.microsoft.com/library/dn479401.aspx)

### <a name="BKMK_6"></a>Configurar la directiva de MFA
Para poder habilitar MFA, debes configurar la directiva MFA en el servidor de federación. En este tutorial, según nuestra directiva MFA, **Robert Hatley** cuenta es necesaria para someterse a MFA porque pertenece a la **finanzas** grupo que configuraste en [configurar el entorno de laboratorio de AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

Puedes configurar la directiva MFA, ya sea mediante la consola de administración de AD FS o mediante Windows PowerShell.

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-the-ad-fs-management-console"></a>Para configurar la directiva MFA en función de los datos de pertenencia a grupo del usuario para 'claimapp' a través de la consola de administración de AD FS

1.  En el servidor de federación, en la consola de administración de AD FS, vaya a **directivas de autenticación**\\**por terceros confianza de confiar** nodo y selecciona la confianza de terceros confianza que representa la aplicación de muestra (**claimapp**).

2.  En la **acciones** página o haciendo clic con el botón por **claimapp**, selecciona **editar personalizado la autenticación multifactor**.

3.  En la **editar confiar terceros de confianza para claimapp** ventana, haz clic en el **agregar** situado junto a la **usuarios o grupos** lista. Escribe en **finanzas** el nombre de tu grupo de AD que creaste en [configurar el entorno de laboratorio de AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)y haz clic en **comprobar nombres**y cuando se resuelve el nombre, haz clic en **Aceptar**.

4.  Haz clic en **Aceptar** en la **editar confiar terceros de confianza para claimapp** ventana.

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-windows-powershell"></a>Para configurar la directiva MFA en función de los datos de pertenencia a grupo del usuario para 'claimapp' a través de Windows PowerShell

1.  En el servidor de federación, abre la ventana de comandos de Windows PowerShell y ejecuta el siguiente comando:

    ```
    $rp = Get-AdfsRelyingPartyTrust -Name claimapp
    ```

2.  En la misma ventana de comandos de Windows PowerShell, ejecuta el siguiente comando:

    ```
    $GroupMfaClaimTriggerRule = 'c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i) <group_SID>$"] => issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
    Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -AdditionalAuthenticationRules $GroupMfaClaimTriggerRule

    ```

    > [!NOTE]
    > Asegúrate de que reemplazar < group_SID > con el valor del SID de tu grupo AD **finanzas**.

## <a name="BKMK_4"></a>Paso 4: Comprobar mecanismo AMF
En este paso se comprueba la funcionalidad MFA que configuraste en el paso anterior. Puedes usar el siguiente procedimiento para comprobar que **Robert Hatley** usuario AD puede tener acceso a la aplicación de muestra y esta vez es necesaria para someterse a MFA porque pertenece a la **finanzas** grupo.

1.  En el equipo cliente, abre una ventana del explorador y navegar a la aplicación de muestra: **https://webserv1.contoso.com/claimapp**.

    Esta acción redirige automáticamente la solicitud al servidor de federación y se le pide que inicies sesión con un nombre de usuario y una contraseña.

2.  Escribe las credenciales de la **Robert Hatley** cuenta de AD.

    En este punto, debido a la directiva MFA que hayas establecido, el usuario, se le pedirá que sufren autenticación adicional. El texto del mensaje predeterminado es **por motivos de seguridad, se requiere información adicional que verifiques tu cuenta.** Sin embargo, este texto es totalmente personalizable. Para obtener más información sobre cómo personalizar la experiencia de inicio de sesión, consulta [personalizar las páginas AD FS Sign-in](https://technet.microsoft.com/library/dn280950.aspx).

    Si has configurado la autenticación de certificado como método de autenticación adicional, el texto del mensaje predeterminado es **seleccionar un certificado que quieras usar para la autenticación. Si se cancela la operación, cierre el explorador y vuelve a intentarlo.**

    Si has configurado la autenticación multifactor de Azure de Windows como método de autenticación adicional, el texto del mensaje predeterminado es **se colocará una llamada a tu teléfono para completar la autenticación.** Para obtener más información sobre cómo iniciar sesión con la autenticación multifactor de Azure de Windows y usar varias opciones para el método preferido de la comprobación de, consulta [Introducción a la autenticación multifactor Windows Azure](https://technet.microsoft.com/library/dn249479.aspx).

## <a name="see-also"></a>Consulta también
[Administrar el riesgo con la autenticación multifactor adicional para aplicaciones con información confidencial](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
[configurar el entorno de laboratorio de AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



