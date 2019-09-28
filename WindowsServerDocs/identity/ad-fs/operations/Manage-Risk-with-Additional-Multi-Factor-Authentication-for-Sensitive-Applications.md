---
ms.assetid: 934ac796-e2ee-490d-8265-6a818be5ee79
title: administración de riesgos con la autenticación multifactor adicional para aplicaciones confidenciales
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 79319f54ceb14195dffd56b5a4dfe1b17f048df9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407525"
---
# <a name="manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>administración de riesgos con la autenticación multifactor adicional para aplicaciones confidenciales




-   [Configuración del entorno de laboratorio para AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)

-   [Guía de tutorial: Administración de riesgos con Multi-Factor Authentication adicionales para aplicaciones confidenciales](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

-   [Configurar métodos de autenticación adicionales para AD FS](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)

## <a name="in-this-guide"></a>En esta guía
En esta guía se proporciona la siguiente información:

-   [Mecanismos de autenticación en AD FS](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_1) : Descripción de los mecanismos de autenticación disponibles en Servicios de federación de Active Directory (AD FS) (AD FS) en Windows Server 2012 R2

-   [Información general del escenario](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2) : Descripción de un escenario en el que se usa Servicios de federación de Active Directory (AD FS) (AD FS) para habilitar la autenticación multifactor (MFA) en función de la pertenencia a grupos del usuario.

    > [!NOTE]
    > En AD FS en Windows Server 2012 R2, puede habilitar MFA basándose en la ubicación de red, la identidad del dispositivo y la identidad del usuario o la pertenencia a grupos.

    Para obtener instrucciones detalladas del Tutorial paso a paso para configurar y comprobar este escenario, consulte la guía de @no__t 0Walkthrough: Administrar los riesgos con Multi-Factor Authentication adicionales para las](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)aplicaciones confidenciales.

## <a name="BKMK_1"></a>Conceptos clave: mecanismos de autenticación en AD FS

### <a name="benefits-of-authentication-mechanisms-in-ad---fs"></a>Ventajas de los mecanismos de autenticación en AD FS
Servicios de federación de Active Directory (AD FS) (AD FS) en Windows Server 2012 R2 proporciona a los administradores de ti un conjunto de herramientas más completo y flexible para autenticar a los usuarios que quieren tener acceso a los recursos corporativos. Permite a los administradores tener un control flexible de los métodos de autenticación principal y adicionales, proporciona una experiencia de administración enriquecida para configurar las directivas de autenticación (a través de la interfaz de usuario y Windows PowerShell) y mejora la experiencia de los usuarios finales que tienen acceso a las aplicaciones y los servicios protegidos por AD FS. A continuación se muestran algunas de las ventajas de proteger la aplicación y los servicios con AD FS en Windows Server 2012 R2:

-   Directiva de autenticación global: una capacidad de administración central desde la que un administrador de ti puede elegir los métodos de autenticación que se usan para autenticar a los usuarios en función de la ubicación de red desde la que tienen acceso a los recursos protegidos. Esto permite a los administradores hacer lo siguiente:

    -   Ordenar el uso de métodos de autenticación más seguros para las solicitudes de acceso procedentes de la extranet.

    -   Habilitar la autenticación de dispositivos para la autenticación de segundo factor optimizada. Esto vincula la identidad del usuario con el dispositivo registrado que se usa para tener acceso al recurso, con lo que se ofrece una verificación de identidad compuesta más segura antes de tener acceso a los recursos protegidos.

        > [!NOTE]
        > Para obtener más información sobre el objeto de dispositivo, el servicio de registro de dispositivos, Workplace Join y el dispositivo como autenticación de segundo factor sin problemas y SSO, consulte [unirse al área de trabajo desde cualquier dispositivo para SSO y autenticación de segundo factor sin problemas en toda la compañía. Aplicaciones](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)de.

    -   Establezca el requisito de MFA para todo el acceso a la extranet o condicionalmente según la identidad del usuario, la ubicación de red o un dispositivo que se usa para tener acceso a los recursos protegidos.

-   Mayor flexibilidad en la configuración de directivas de autenticación: puede configurar directivas de autenticación personalizadas para los recursos protegidos mediante AD FS con distintos valores empresariales. Por ejemplo, puede requerir MFA para una aplicación con un fuerte impacto empresarial.

-   Facilidad de uso: herramientas de administración sencillas e intuitivas como el complemento MMC de administración de AD FS basado en GUI y los cmdlets de Windows PowerShell permiten a los administradores de ti configurar directivas de autenticación con relativa facilidad. Con Windows PowerShell, puede crear scripts de sus soluciones para usarlos a escala y automatizar tareas administrativas rutinarias.

-   Mayor control sobre los activos corporativos: como administrador puede usar AD FS para configurar una directiva de autenticación que se aplique a un recurso específico, tiene un mayor control sobre cómo se protegen los recursos corporativos. Las aplicaciones no pueden reemplazar las directivas de autenticación especificadas por los administradores de TI. Para aplicaciones y servicios confidenciales, puede habilitar la obligatoriedad de MFA, la autenticación de dispositivos y, opcionalmente, la repetición de la autenticación cada vez que se tenga acceso al recurso.

-   Compatibilidad con proveedores de MFA personalizados: para las organizaciones que usan métodos de MFA de terceros, AD FS ofrece la posibilidad de incorporar y usar estos métodos de autenticación sin problemas.

### <a name="authentication-scope"></a>Ámbito de autenticación
En AD FS en Windows Server 2012 R2, puede especificar una directiva de autenticación en un ámbito global que se aplique a todas las aplicaciones y servicios que están protegidos por AD FS.  También puede establecer directivas de autenticación para aplicaciones y servicios específicos (relaciones de confianza para usuario autenticado) protegidos por AD FS. Si se especifica una directiva de autenticación para una aplicación concreta (por relación de confianza para usuario autenticado) no se reemplaza la directiva de autenticación global. Si la directiva de autenticación global o la directiva de autenticación por relación de confianza para usuario autenticado requieren MFA, MFA se desencadenará cuando el usuario intente autenticarse a esta relación de confianza para usuario autenticado.  La directiva de autenticación global es una reserva para las relaciones de confianza para usuario autenticado (aplicaciones y servicios) que no tengan configurada una directiva de autenticación específica.

Una directiva de autenticación global se aplica a todos los usuarios de confianza que están protegidos por AD FS. Puede configurar las siguientes opciones como parte de la directiva de autenticación global:

-   Métodos globales que se usarán para la autenticación principal

-   Configuración y métodos para MFA

-   Si se habilita la autenticación de dispositivos. Para obtener más información, consulte [unirse a un área de trabajo desde cualquier dispositivo para SSO y autenticación de segundo factor sin problemas en todas las aplicaciones de la compañía](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

Las directivas de autenticación por relación de confianza para usuario autenticado se aplican de forma concreta a los intentos de acceso a esa relación de confianza para usuario autenticado (aplicación o servicio). Puede configurar las siguientes opciones como parte de la directiva de autenticación por relación de confianza para usuario autenticado:

-   Si se requiere a los usuarios que proporcionen sus credenciales cada vez que inician sesión

-   Configuración de MFA basada en el usuario/grupo, registro de dispositivo y datos de ubicación de solicitud de acceso

### <a name="primary-and-additional-authentication-methods"></a>Métodos de autenticación principal y adicionales
Con AD FS en Windows Server 2012 R2, además del mecanismo de autenticación principal, los administradores pueden configurar métodos de autenticación adicionales. Los métodos de autenticación principales están integrados y están diseñados para validar las identidades de los usuarios. Puede configurar factores de autenticación adicionales para solicitar que se proporcione más información sobre la identidad del usuario y, por consiguiente, garantizar una autenticación más segura.

Con la autenticación principal en AD FS en Windows Server 2012 R2, tiene las siguientes opciones:

-   Para los recursos publicados a los que se va a tener acceso desde fuera de la red corporativa, la autenticación mediante formularios está seleccionada de manera predeterminada. Además, también puede habilitar la autenticación de certificado (es decir, autenticación basada en tarjetas inteligentes o autenticación de certificado de cliente de usuario que funcione con AD DS).

-   Para los recursos de intranet, la autenticación de Windows está seleccionada de manera predeterminada. Además, también puede habilitar la autenticación mediante formularios o de certificado.

Si selecciona más de un método de autenticación, permitirá a sus usuarios disponer de varias opciones a la hora de elegir el método de autenticación en la página de inicio de sesión de la aplicación o el servicio.

También puede habilitar la autenticación de dispositivos para la autenticación de segundo factor optimizada. Esto vincula la identidad del usuario con el dispositivo registrado que se usa para tener acceso al recurso, con lo que se ofrece una verificación de identidad compuesta más segura antes de tener acceso a los recursos protegidos.

> [!NOTE]
> Para obtener más información sobre el objeto de dispositivo, el servicio de registro de dispositivos, Workplace Join y el dispositivo como autenticación de segundo factor sin problemas y SSO, consulte [unirse al área de trabajo desde cualquier dispositivo para SSO y autenticación de segundo factor sin problemas en toda la compañía. Aplicaciones](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)de.

Si especifica el método de autenticación de Windows (opción predeterminada) para los recursos de la intranet, las solicitudes de autenticación utilizan este método sin problemas en los exploradores compatibles con la autenticación de Windows.

> [!NOTE]
> La autenticación de Windows no es compatible con todos los exploradores. El mecanismo de autenticación de AD FS en Windows Server 2012 R2 detecta el agente de usuario del explorador del usuario y utiliza una opción configurable para determinar si ese agente de usuario es compatible con la autenticación de Windows. Los administradores pueden agregar entradas a esta lista de agentes de usuario (a través del comando `Set-AdfsProperties -WIASupportedUserAgents` de Windows PowerShell) para especificar cadenas de agente de usuario alternativos para los exploradores compatibles con la autenticación de Windows. Si el agente de usuario del cliente no admite la autenticación de Windows, el método de reserva predeterminado es la autenticación mediante formularios.

### <a name="configuring-mfa"></a>Configuración de MFA
Hay dos partes para configurar MFA en AD FS en Windows Server 2012 R2: especificar las condiciones en las que se requiere MFA y seleccionar un método de autenticación adicional. Para obtener más información sobre métodos de autenticación adicionales, vea [configurar métodos de autenticación adicionales para AD FS](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md).

**Configuración de MFA**

Las opciones siguientes están disponibles para la configuración de MFA (condiciones bajo las cuales el uso de MFA es obligatorio):

-   Ahora puede requerir MFA para usuarios y grupos concretos del dominio de AD al que se ha unido su servidor de federación.

-   Puede requerir MFA para dispositivos registrados (unidos al área de trabajo) o no registrados (no unidos al área de trabajo).

    Windows Server 2012 R2 adopta un enfoque centrado en el usuario a los dispositivos modernos, donde los objetos de dispositivo representan una relación entre user@device y una empresa. Los objetos de dispositivo son una nueva clase de AD en Windows Server 2012 R2 que se puede usar para ofrecer identidad compuesta al proporcionar acceso a aplicaciones y servicios. Un nuevo componente de AD FS, el servicio de registro de dispositivos (DRS), aprovisiona una identidad de dispositivo en Active Directory y establece un certificado en el dispositivo de consumo que se utilizará para representar la identidad del dispositivo. A continuación, puede usar esta identidad de dispositivo para unir su dispositivo al área de trabajo, es decir, para conectar su dispositivo personal a Active Directory en su área de trabajo. Cuando una su dispositivo personal al área de trabajo, se convertirá en un dispositivo conocido y proporcionará autenticación de segundo factor sin problemas para las aplicaciones y los recursos protegidos. En otras palabras, una vez que un dispositivo está unido al área de trabajo, la identidad del usuario está asociada a este dispositivo y se puede usar para una verificación de identidad compuesta sin problemas antes de tener acceso a un recurso protegido.

    Para obtener más información acerca de la Unión al área de trabajo y el abandono, consulte [unirse al área de trabajo desde cualquier dispositivo para SSO y autenticación de segundo factor sin problemas en las aplicaciones de la empresa](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

-   Puede requerir MFA cuando la solicitud de acceso a los recursos protegidos provenga de la extranet o la intranet.

## <a name="BKMK_2"></a>Información general del escenario
En este escenario, habilitará MFA basándose en los datos de pertenencia a grupos del usuario para una aplicación específica. Es decir, configurará una directiva de autenticación en el servidor de federación que requiera MFA cuando los usuarios que pertenezcan a un grupo concreto soliciten acceso a una aplicación específica hospedada en un servidor web.

Más concretamente, en este escenario habilitará una directiva de autenticación para una aplicación de prueba basada en notificaciones denominada **claimapp**, en la que el usuario de AD **Robert Hatley** estará obligado a someterse a MFA, ya que pertenece al grupo de AD **Finance**.

Las instrucciones paso a paso para configurar y comprobar este escenario se proporcionan en @no__t guía de 0Walkthrough: Administrar los riesgos con Multi-Factor Authentication adicionales para las](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)aplicaciones confidenciales. Para completar los pasos de este tutorial, debe configurar un entorno de laboratorio y seguir los pasos descritos en [configurar el entorno de laboratorio para AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

Otros escenarios para habilitar MFA en AD FS incluyen los siguientes:

-   Habilitar MFA si la solicitud de acceso proviene de la intranet. Puede modificar el código presentado en la sección "configurar la Directiva de MFA" de @no__t guía de 0Walkthrough: Administre el riesgo con Multi-Factor Authentication adicionales para las aplicaciones confidenciales @ no__t-0 con lo siguiente:

    ```
    'c:[type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn" );'
    ```

-   Habilitar MFA si la solicitud de acceso proviene de un dispositivo que no se ha unido al área de trabajo.  Puede modificar el código presentado en la sección "configurar la Directiva de MFA" de @no__t guía de 0Walkthrough: Administre el riesgo con Multi-Factor Authentication adicionales para las aplicaciones confidenciales @ no__t-0 con lo siguiente:

    ```
    'NOT EXISTS([type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/registrationid"]) => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

-   Habilitar MFA si el acceso proviene de un usuario con un dispositivo unido al área de trabajo, pero no registrado a nombre de este usuario. Puede modificar el código presentado en la sección "configurar la Directiva de MFA" de @no__t guía de 0Walkthrough: Administre el riesgo con Multi-Factor Authentication adicionales para las aplicaciones confidenciales @ no__t-0 con lo siguiente:

    ```
    'c:[type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", value == "false"] => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

## <a name="see-also"></a>Vea también
[Guía de tutorial: Administración de riesgos con Multi-Factor Authentication adicionales para las aplicaciones confidenciales @ no__t-0 @ no__t-1[configurar el entorno de laboratorio para AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



