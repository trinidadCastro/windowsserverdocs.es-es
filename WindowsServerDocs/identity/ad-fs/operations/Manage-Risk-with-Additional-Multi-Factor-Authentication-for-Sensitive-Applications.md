---
ms.assetid: 934ac796-e2ee-490d-8265-6a818be5ee79
title: "Administrar el riesgo con la autenticación multifactor adicional para aplicaciones con información confidencial"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e9ee275e6fe38005394cd071e9cfe9a7999350e8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>Administrar el riesgo con la autenticación multifactor adicional para aplicaciones con información confidencial

>Se aplica a: Windows Server 2012 R2


-   [Configurar el entorno de laboratorio de AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)

-   [Guía paso a paso: Administrar el riesgo con la autenticación multifactor adicional para aplicaciones con información confidencial](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

-   [Configurar los métodos de autenticación adicional de AD FS](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)

## <a name="in-this-guide"></a>En esta guía
Esta guía proporciona la siguiente información:

-   [Mecanismos de autenticación de AD FS](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_1) -descripción de los mecanismos de autenticación disponibles en los servicios de federación de Active Directory (AD FS) en Windows Server 2012 R2

-   [Información general del escenario](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2) -una descripción de un escenario donde usan los servicios de federación de Active Directory (AD FS) para habilitar la autenticación multifactor (AMF) en función de la pertenencia a grupos del usuario.

    > [!NOTE]
    > Puedes habilitar MFA basándose en la ubicación de red, la identidad del dispositivo y el usuario identidad o pertenencia a grupo de AD FS en Windows Server 2012 R2.

    Para obtener instrucciones detalladas tutorial paso a paso para configurar y comprobar este escenario, consulta [guía paso a paso: administrar el riesgo con la autenticación multifactor adicional para aplicaciones con información confidencial](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).

## <a name="BKMK_1"></a>Conceptos clave - mecanismos de autenticación de AD FS

### <a name="benefits-of-authentication-mechanisms-in-ad---fs"></a>Ventajas de los mecanismos de autenticación de AD FS
Servicios de federación de Active Directory (AD FS) en Windows Server 2012 R2 proporciona a los administradores de TI con un conjunto de herramientas más enriquecido y más flexible para autenticar a los usuarios que quieran tener acceso a recursos corporativos. Permite a los administradores con un control flexible sobre el principal y los métodos de autenticación adicional, proporciona una experiencia de administración enriquecida para configurar la autenticación directivas (tanto a través de la interfaz de usuario y de Windows PowerShell) y mejora la experiencia para los usuarios finales que tienen acceso a aplicaciones y servicios que están protegidos por AD FS. Éstas son algunas de las ventajas de seguridad de aplicaciones y servicios con AD FS en Windows Server 2012 R2:

-   Directiva de autenticación global: una funcionalidad de administración central, desde el que un administrador de TI puede elegir métodos de autenticación que se usan para autenticar usuarios en función de la ubicación de red desde el que acceden a los recursos protegidos. Esto permite a los administradores hacer lo siguiente:

    -   Obliga al uso de los métodos de autenticación más seguros para las solicitudes de acceso desde la extranet.

    -   Habilitar la autenticación de dispositivo para la autenticación de factor de segundo sin problemas. Esto vincula la identidad del usuario para el dispositivo registrado que se usa para tener acceso al recurso, por lo que ofrece la verificación de identidad compuestos más segura antes de que se tiene acceso a recursos protegidos.

        > [!NOTE]
        > Para obtener más información sobre el objeto de dispositivo, el servicio de registro de dispositivos, al área de trabajo y el dispositivo como la autenticación de factor de segundo sin problemas y SSO, consulta [unir al área de trabajo desde cualquier dispositivo para SSO y transparente segundo Factor de autenticación a través de aplicaciones de la compañía](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

    -   Establece el requisito de MFA todo el acceso extranet o condicionalmente en función de la identidad del usuario, la ubicación de red o un dispositivo que se usa para acceder a recursos protegidos.

-   Mayor flexibilidad en la configuración de directivas de autenticación: puedes configurar las directivas de autenticación personalizado para los recursos de AD FS protegidas con distintos valores de empresas. Por ejemplo, pueden requerir AMF para la aplicación con un impacto empresarial alto.

-   Facilidad de uso: herramientas de administración sencillo e intuitivo, como el complemento basado en la interfaz gráfica de usuario de AD FS Management MMC y los cmdlets de Windows PowerShell permiten los administradores de TI configurar las directivas de autenticación con relativa facilidad. Con Windows PowerShell, puedes crear un script tus soluciones para su uso a escala y para automatizar tareas administrativas corrientes.

-   Mayor control sobre los activos de la empresa: dado que un administrador puede usar AD FS para configurar una directiva de autenticación que se aplica a un recurso específico, dispones de mayor control sobre los recursos corporativos cómo se protegen. Las aplicaciones no pueden reemplazar las directivas de autenticación especificadas por los administradores de TI. Para aplicaciones con información confidencial y servicios, puedes habilitar MFA requisito, dispositivo autenticación y, opcionalmente, actualizado cada vez que se accede a los recursos.

-   Compatibilidad con los proveedores MFA personalizados: para las organizaciones que sacan partido de métodos MFA de terceros, AD FS ofrece la posibilidad de incorporar y usar estos métodos de autenticación sin problemas.

### <a name="authentication-scope"></a>Ámbito de autenticación
Puede especificar una directiva de autenticación en un ámbito global que se aplica a todas las aplicaciones y servicios que están protegidos por AD FS de AD FS en Windows Server 2012 R2.  También puedes establecer directivas de autenticación para determinadas aplicaciones y servicios (confiar confianzas de terceros) que están protegidos por AD FS. Especificar una directiva de autenticación para una aplicación concreta (por confiar confianza de terceros) no reemplaza la directiva de autenticación global. Si globales o por usar la directiva de autenticación requiere MFA, MFA de confianza de terceros se activará cuando el usuario intente autenticarse en esta confianza de terceros de confianza.  La directiva de autenticación global es una reserva para usuarios de confianza confianzas de terceros (aplicaciones y servicios) que no tienen una directiva de autenticación específico configurada.

Una directiva de autenticación global se aplica a todos los usuarios de confianza que están protegidos por AD FS. Puedes configurar la siguiente configuración como parte de la directiva de autenticación global:

-   Métodos de autenticación que se usará para la autenticación principal

-   Configuración y los métodos de MFA

-   Si está habilitada la autenticación de dispositivo. Para obtener más información, consulta [unir al área de trabajo desde cualquier dispositivo para SSO y transparente segundo Factor de autenticación a través de aplicaciones de la compañía](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

Confiar por las directivas de autenticación de confianza de terceros se aplican específicamente a los intentos de acceso a esa confianza de terceros de confianza (aplicación o servicio). Puedes configurar la siguiente configuración como parte de la directiva de autenticación de confianza de terceros confiar por:

-   Si es necesario para proporcionar sus credenciales al iniciar sesión cada vez que los usuarios

-   Configuración de MFA en función del usuario o grupo, registro de dispositivos y solicitud de acceso a datos de ubicación

### <a name="primary-and-additional-authentication-methods"></a>Métodos de autenticación principal y adicionales
Con AD FS en Windows Server 2012 R2, además del mecanismo de autenticación principal, los administradores pueden configurar los métodos de autenticación adicional. Métodos de autenticación principal están integradas y están pensadas para validar las identidades de usuarios. Puedes configurar factores de autenticación adicional para solicitar que se proporciona más información sobre la identidad del usuario y, por consiguiente, asegúrate de autenticación más segura.

Con la autenticación principal de AD FS en Windows Server 2012 R2, tienes las siguientes opciones:

-   Recursos que se han publicado para tener acceso desde fuera de la red corporativa, formularios de autenticación se selecciona de manera predeterminada. Además, también puedes habilitar la autenticación de certificado (en otras palabras, la autenticación con tarjeta inteligente o autenticación de certificado de cliente de usuario que funciona con AD DS).

-   Recursos de la intranet, autenticación de Windows se selecciona de manera predeterminada. Además también puede habilitar formularios o la autenticación de certificado.

Al seleccionar más de un método de autenticación, permiten a los usuarios pueden decidir qué método autenticarse con a la página de inicio de sesión de la aplicación o servicio.

También puede habilitar la autenticación de dispositivo para la autenticación de factor de segundo sin problemas. Esto vincula la identidad del usuario para el dispositivo registrado que se usa para tener acceso al recurso, por lo que ofrece la verificación de identidad compuestos más segura antes de que se tiene acceso a recursos protegidos.

> [!NOTE]
> Para obtener más información sobre el objeto de dispositivo, el servicio de registro de dispositivos, al área de trabajo y el dispositivo como la autenticación de factor de segundo sin problemas y SSO, consulta [unir al área de trabajo desde cualquier dispositivo para SSO y transparente segundo Factor de autenticación a través de aplicaciones de la compañía](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

Si especificas el método de autenticación de Windows (opción predeterminada) para los recursos de la intranet, las solicitudes de autenticación someterse a este método sin problemas en los exploradores que admiten la autenticación de Windows.

> [!NOTE]
> Autenticación de Windows no se admite en todos los exploradores. El mecanismo de autenticación de AD FS en Windows Server 2012 R2 detecta el agente de usuario del explorador del usuario y usa un valor configurable para determinar si ese agente de usuario admite la autenticación de Windows. Los administradores pueden agregar a la lista de agentes de usuario (a través de Windows PowerShell `Set-AdfsProperties -WIASupportedUserAgents`comando, para especificar cadenas de agente de usuario alternativo para los exploradores que admiten la autenticación de Windows. Si el agente de usuario del cliente no admite la autenticación de Windows, el método de reserva predeterminado es la autenticación de formularios.

### <a name="configuring-mfa"></a>Configuración de MFA
Hay dos partes para configurar MFA de AD FS en Windows Server 2012 R2: especificar las condiciones en las que se requiere MFA y seleccionar un método de autenticación adicional. Para obtener más información acerca de los métodos de autenticación adicional, consulta [configurar otros métodos de autenticación de AD FS](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md).

**Configuración de MFA**

Las siguientes opciones están disponibles para la configuración de MFA (condiciones en la que se requieren AMF):

-   Puedes requerir AMF para determinados usuarios y grupos en el que el servidor de federación está unido a dominio de AD.

-   Puedes requerir AMF para bien registra (área de trabajo Unido) o no (no unidos a un trabajo) dispositivos.

    Windows Server 2012 R2 toma un enfoque centrado en el usuario a donde representan objetos de dispositivo de la relación entre los dispositivos modernos user@devicey una compañía. Objetos de dispositivo son una nueva clase en AD en Windows Server 2012 R2 que puede usarse para ofrecer compuesto identidad al proporcionar acceso a aplicaciones y servicios. Un nuevo componente de AD FS - el servicio de registro dispositivo (DRS) - aprovisiona una identidad del dispositivo en Active Directory y establece un certificado en el dispositivo de consumidor que se usará para representar la identidad del dispositivo. Puedes usar este dispositivo identidad al área de trabajo unir el dispositivo, en otras palabras, para conectar su dispositivo personal en Active Directory de tu empresa. Cuando te unes a su dispositivo personal para tu empresa, se convierte en un dispositivo conocido y proporcionará la autenticación de factor de segundo transparente a recursos protegidos y aplicaciones. En otras palabras, después de un dispositivo está unido a un área de trabajo, la identidad del usuario está vinculada a este dispositivo y puede usarse para una comprobación de identidad compuesta sin problemas antes de que se accede a un recurso protegido.

    Para obtener más información sobre la unión a un área de trabajo y de salida, consulta [unirse a un área de trabajo desde cualquier dispositivo para SSO y transparente segundo Factor de autenticación a través de aplicaciones de la compañía](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

-   Puedes requerir MFA cuando la solicitud de acceso de los recursos protegidos procede de la extranet o la intranet.

## <a name="BKMK_2"></a>Información general del escenario
En este escenario, se habilita MFA en función de los datos del usuario grupo pertenencia para una aplicación específica. En otras palabras, se establece una directiva de autenticación en el servidor de federación para requerir MFA cuando los usuarios que pertenecen a un determinado grupo solicitan acceso a una aplicación específica que está hospedado en un servidor web.

Más concretamente, en este escenario, habilita una directiva de autenticación para una aplicación de prueba basada en notificaciones denominada **claimapp**, mediante el cual un usuario AD **Robert Hatley** deben someterse a MFA dado que pertenece a un grupo AD **finanzas**.

Se proporcionan instrucciones paso a paso para configurar y comprobar este escenario en [guía paso a paso: administrar el riesgo con la autenticación multifactor adicional para aplicaciones con información confidencial](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md). Para completar los pasos de este tutorial, debes configurar un entorno de laboratorio y sigue los pasos de [configurar el entorno de laboratorio de AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

Otros escenarios de habilitar MFA en AD FS incluyen lo siguiente:

-   Habilitar MFA, si procede de la solicitud de acceso de la extranet. Puedes modificar el código que figura en la sección "Establecer una directiva de MFA" de [guía paso a paso: administrar el riesgo con la autenticación multifactor adicional para aplicaciones con información confidencial](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) con las siguientes acciones:

    ```
    'c:[type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn" );'
    ```

-   Habilitar MFA, si procede de la solicitud de acceso de un dispositivo combinado de área de trabajo no.  Puedes modificar el código que figura en la sección "Establecer una directiva de MFA" de [guía paso a paso: administrar el riesgo con la autenticación multifactor adicional para aplicaciones con información confidencial](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) con las siguientes acciones:

    ```
    'NOT EXISTS([type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/registrationid"]) => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

-   Habilitar MFA, si el acceso proviene de un usuario con un dispositivo que es el área de trabajo se unió pero no registrado para este usuario. Puedes modificar el código que figura en la sección "Establecer una directiva de MFA" de [guía paso a paso: administrar el riesgo con la autenticación multifactor adicional para aplicaciones con información confidencial](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) con las siguientes acciones:

    ```
    'c:[type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", value == "false"] => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

## <a name="see-also"></a>Consulta también
[Guía paso a paso: Administrar el riesgo con la autenticación multifactor adicional para aplicaciones con información confidencial](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)<ph x="2">
</ph>[configurar el entorno de laboratorio de AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



