---
ms.assetid: a0f7bb11-47a5-47ff-a70c-9e6353382b39
title: Administración de riesgos con control de acceso condicional
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2c399467a8bb70e723a86618aa37fc54425f4e7d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189051"
---
# <a name="manage-risk-with-conditional-access-control"></a>Administración de riesgos con control de acceso condicional




-   [Control de acceso de condicional de los conceptos clave de AD FS](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Administración de riesgos con Control de acceso condicional](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md#BKMK_2)

## <a name="BKMK_1"></a>Conceptos clave: control de acceso condicional en AD FS
La función general de AD FS es emitir un token de acceso que contiene un conjunto de notificaciones. La decisión con respecto a qué notificaciones de AD FS acepta y luego emite se rige por reglas de notificación.

Control de acceso en AD FS se implementa con reglas de notificación de autorización de emisión que se usan para emitir un permiso o denegar las notificaciones que determinarán si un usuario o un grupo de usuarios se podrá tener acceso a recursos de AD FS protegido o no. Las reglas de autorización solo se pueden establecer en relaciones de confianza para usuario autenticado.

|Opción de regla|Lógica de regla|
|---------------|--------------|
|Permitir a todos los usuarios|Si el tipo de notificación entrante es igual a *cualquier tipo de notificación* y el valor es igual a *cualquier valor*, emitir una notificación con un valor igual a *Permitir*|
|Permitir acceso a los usuarios con esta notificación entrante|Si el tipo de notificación entrante es igual a *tipo de notificación especificado* y el valor es igual a *valor de notificación especificado*, emitir una notificación con un valor igual a *Permitir*|
|Denegar acceso a los usuarios con esta notificación entrante|Si el tipo de notificación entrante es igual a *tipo de notificación especificado* y el valor es igual a *valor de notificación especificado*, emitir una notificación con un valor igual a *Denegar*|

Para obtener más información sobre estas opciones y la lógica de regla, consulte [When to Use an Authorization Claim Rule](https://technet.microsoft.com/library/ee913560.aspx).

En AD FS en Windows Server 2012 R2, control de acceso mejora con varios factores, incluido el usuario, dispositivo, ubicación y datos de autenticación. Esto es posible porque hay una gran variedad de tipos de notificación disponibles para las reglas de notificación de autorización.  En otras palabras, en AD FS en Windows Server 2012 R2, puede aplicar control de acceso condicional basado en miembros del grupo o la identidad de usuario, ubicación de red, el dispositivo (ya sea un espacio de trabajo Unido, para obtener más información, consulte [unirse a un área de trabajo desde cualquier Dispositivo para SSO y sin problemas segundo Factor Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)) y el estado de autenticación (si se realizó la autenticación multifactor (MFA)).

Control de acceso condicional en AD FS en Windows Server 2012 R2, ofrece las siguientes ventajas:

-   Directivas de autorización por aplicación flexibles y expresivas mediante las que puede permitir o denegar el acceso en función del usuario, el dispositivo, la ubicación de red y el estado de autenticación

-   Creación de reglas de autorización de emisión para aplicaciones de confianza para usuario autenticado

-   Experiencia de IU completa para los escenarios habituales de control de acceso condicional

-   Lenguaje de notificaciones completo y compatibilidad con Windows PowerShell para los escenarios avanzados de control de acceso condicional

-   Personalizado (por usuarios de confianza aplicación) mensajes de "Acceso denegado". Para obtener más información, consulte [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx). Al poder personalizar estos mensajes, puede explicar por qué se deniega el acceso a un usuario y, además, facilitar una corrección de autoservicio cuando sea posible, por ejemplo, solicitar a los usuarios que unan sus dispositivos al área de trabajo. Para obtener más información, consulte [Join to Workplace from Any Device for SSO and Seamless Second Factor Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

En la tabla siguiente incluye todos los tipos de notificación disponibles en AD FS en Windows Server 2012 R2 que se usará para implementar el control de acceso condicional.

|Tipo de notificación|Descripción|
|--------------|---------------|
|Dirección de correo electrónico|Dirección de correo electrónico del usuario|
|Nombre propio|Nombre propio del usuario|
|Name|Nombre del usuario|
|UPN|Nombre principal de usuario (UPN) del usuario|
|Nombre común|Nombre común del usuario|
|Dirección de correo electrónico para AD FS 1|Dirección de correo electrónico del usuario al interoperar con AD FS 1.1 o AD FS 1.0|
|Agrupar|Grupo al que pertenece el usuario|
|UPN para AD FS 1|UPN del usuario al interoperar con AD FS 1.1 o AD FS 1.0|
|Rol|Rol que tiene el usuario|
|Apellido|Apellido del usuario|
|PPID|Identificador privado del usuario|
|Id. de nombre|Identificador de nombre SAML del usuario|
|Marca de tiempo de autenticación|Se usa para mostrar la hora y la fecha en que se autenticó al usuario.|
|Método de autenticación|Método usado para autenticar al usuario|
|SID de grupo de solo denegación|SID de grupo usado solo para denegar del usuario|
|SID principal de solo denegación|SID principal usado solo para denegar del usuario|
|SID de grupo principal de solo denegación|SID de grupo principal usado solo para denegar del usuario|
|SID de grupo|SID de grupo del usuario|
|SID de grupo principal|SID de grupo principal del usuario|
|SID principal|SID principal del usuario|
|Nombre de cuenta de Windows|Nombre de cuenta de dominio del usuario con el formato dominio\usuario|
|Usuario registrado|El usuario está registrado para usar este dispositivo.|
|Identificador de dispositivo|Identificador del dispositivo|
|Identificador de registro de dispositivo|Identificador para el registro del dispositivo|
|Nombre para mostrar de registro de dispositivo|Nombre para mostrar del registro de dispositivo|
|Tipo de SO del dispositivo|Tipo de sistema operativo del dispositivo|
|Versión de SO del dispositivo|Versión de sistema operativo del dispositivo|
|Dispositivo administrado|El dispositivo se administra mediante un servicio de administración.|
|IP de cliente reenviada|Dirección IP del usuario|
|Aplicación cliente|Tipo de aplicación cliente|
|Agente de usuario del cliente|Tipo de dispositivo usado por el cliente para tener acceso a la aplicación|
|IP del cliente|Dirección IP del cliente|
|Ruta de acceso de extremo|Ruta de acceso absoluta al extremo, que se puede usar para determinar si un cliente es activo o pasivo|
|Proxy|Nombre DNS del servidor proxy de federación que pasó la solicitud|
|Identificador de la aplicación|Identificador de la aplicación de confianza para usuario autenticado|
|Directivas de aplicación|Directivas de aplicación del certificado|
|Identificador de clave de entidad emisora|Extensión de identificador de clave de entidad emisora del certificado que firmó un certificado emitido|
|Restricción básica|Una de las restricciones básicas del certificado|
|Uso mejorado de clave|Describe uno de los usos mejorados de clave del certificado.|
|Emisor|Nombre de la entidad de certificación que emitió el certificado X.509|
|Nombre del emisor|Nombre distintivo del emisor del certificado|
|Uso de la clave|Uno de los usos de clave del certificado|
|Hasta el|Fecha en hora local después de la cual un certificado ya no es válido|
|A partir del|Fecha en hora local en la cual un certificado empieza a ser válido|
|Directivas de certificado|Directivas en virtud de las cuales se emitió el certificado|
|Clave pública|Clave pública del certificado|
|Datos sin procesar del certificado|Datos sin procesar del certificado|
|Nombre alternativo del firmante|Uno de los nombres alternativos del certificado|
|Número de serie|Numero de serie del certificado|
|Algoritmo de firma|Algoritmo usado para crear la firma de un certificado|
|Subject|Firmante del certificado|
|Identificador de clave del firmante|Identificador de clave del firmante del certificado|
|Nombre del firmante|Nombre distintivo del firmante de un certificado|
|Nombre de plantilla V2|Es el nombre de la plantilla de certificado de la versión 2 que se usa al emitir o renovar un certificado. Se trata de un valor específico de Microsoft.|
|Nombre de plantilla V1|Es el nombre de la plantilla de certificado de la versión 1 que se usa al emitir o renovar un certificado. Se trata de un valor específico de Microsoft.|
|Huella digital|Huella digital del certificado|
|Versión X.509|Versión en formato X.509 del certificado|
|Dentro de la red corporativa|Se utiliza para indicar si una solicitud tiene su origen dentro de la red corporativa.|
|Hora de expiración de la contraseña|Se usa para mostrar la hora en la que expirará la contraseña.|
|Días hasta la expiración de la contraseña|Se usa para mostrar el número de días que quedan hasta la expiración de la contraseña.|
|Dirección URL de actualización de contraseña|Se usa para mostrar la dirección web del servicio de actualización de contraseña.|
|Referencias de métodos de autenticación|Se usa para indicar los métodos de autenticación que se usan para autenticar al usuario.|

## <a name="BKMK_2"></a>Administración de riesgos con Control de acceso condicional
Con la configuración disponible hay muchas maneras de administrar los riesgos mediante la implementación del control de acceso condicional.

### <a name="common-scenarios"></a>Escenarios habituales
Por ejemplo, imagine un escenario sencillo de implementación del control de acceso condicional basado en datos de pertenencia a grupos del usuario para una aplicación concreta (de confianza). En otras palabras, puede configurar una regla de autorización de emisión en el servidor de federación para permitir a los usuarios que pertenecen a un determinado grupo de AD acceso al dominio a una aplicación concreta que está protegida por AD FS.  Las instrucciones de paso a paso detalladas (mediante la interfaz de usuario y Windows PowerShell) para implementar este escenario se tratan en [guía paso a paso: Administración de riesgos con Control de acceso condicional](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md). Para completar los pasos descritos en este tutorial, debe configurar un entorno de laboratorio y seguir los pasos descritos en [configurar el entorno de laboratorio para AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

### <a name="advanced-scenarios"></a>Escenarios avanzados
Otros ejemplos de implementación del control de acceso condicional en AD FS en Windows Server 2012 R2 incluyen lo siguiente:

-   Permitir el acceso a una aplicación protegida por AD FS solo si la identidad del usuario se ha validado con MFA

    Puede usar el código siguiente:

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "PermitAccessWithMFA"
    c:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Permitir el acceso a una aplicación protegida por AD FS solo si la solicitud de acceso procede de un dispositivo unido al área de trabajo que se registra al usuario

    Puede usar el código siguiente:

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "PermitAccessFromRegisteredWorkplaceJoinedDevice"
    c:[Type == "https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", Value =~ "^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Permitir el acceso a una aplicación protegida por AD FS solo si la solicitud de acceso procede de un dispositivo unido al área de trabajo que se ha registrado para un usuario cuya identidad se ha validado con MFA

    Puede usar el código siguiente:

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "RequireMFAOnRegisteredWorkplaceJoinedDevice"
    c1:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$"] &&
    c2:[Type == "https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", Value =~ "^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Permitir acceso de extranet a una aplicación protegida por AD FS solo si la solicitud de acceso procede de un usuario cuya identidad se ha validado con MFA.

    Puede usar el código siguiente:

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "RequireMFAForExtranetAccess"
    c1:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$"] &&
    c2:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value =~ "^(?i)false$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

## <a name="see-also"></a>Vea también
[Guía de tutorial: Administración de riesgos con Control de acceso condicional](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)
[configurar el entorno de laboratorio para AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



