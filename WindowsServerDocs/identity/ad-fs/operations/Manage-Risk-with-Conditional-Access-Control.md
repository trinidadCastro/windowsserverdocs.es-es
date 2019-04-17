---
ms.assetid: a0f7bb11-47a5-47ff-a70c-9e6353382b39
title: Administrar el riesgo con Control de acceso condicional
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e2ad7d1467abd6d69077b515b8c69a65f7e70f19
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="manage-risk-with-conditional-access-control"></a>Administrar el riesgo con Control de acceso condicional

>Se aplica a: Windows Server 2012 R2


-   [Control de acceso a la clave conceptos condicional en AD FS](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [Administración de riesgos con Control de acceso condicional](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md#BKMK_2)

## <a name="BKMK_1"></a>Conceptos clave: el control de acceso condicional en ADFS
La función global de AD FS es emitir un token de acceso que contiene un conjunto de notificaciones. La decisión sobre qué notificaciones AD FS acepta y, a continuación, emite se rige por las reglas de notificación.

Control de acceso en AD FS se implementa con reglas de notificación de autorización de emisión que se usan para emitir una permitir o denegar notificaciones que determinan si un usuario o un grupo de usuarios podrán tener acceso a recursos de AD FS protegido o no. Las reglas de autorización solo pueden establecerse en confianzas de terceros de confianza.

|Opción de regla|Lógica de la regla|
|---------------|--------------|
|Permitir que todos los usuarios|Si el tipo de notificación entrante es igual a *cualquier tipo de notificación* y el valor es igual a *cualquier valor*, a continuación, reclamar el problema con el valor es igual a *permitir*|
|Permitir el acceso a los usuarios con esta notificación entrante|Si el tipo de notificación entrante es igual a *especifica el tipo de notificación* y el valor es igual a *especifica el valor de notificación*, a continuación, reclamar el problema con el valor es igual a *permitir*|
|Denegar el acceso a los usuarios con esta notificación entrante|Si el tipo de notificación entrante es igual a *especifica el tipo de notificación* y el valor es igual a *especifica el valor de notificación*, a continuación, reclamar el problema con el valor es igual a *denegar*|

Para obtener más información acerca de estas opciones de reglas y la lógica, consulta [cuándo usar una regla de solicitud de autorización](https://technet.microsoft.com/library/ee913560.aspx).

En AD FS en Windows Server 2012 R2, se mejora el control de acceso con varios factores, incluidos los datos de autenticación, ubicación, dispositivo y usuario. Esto se realiza mediante una mayor variedad de tipos de notificaciones disponible para las reglas de notificación de autorización.  En otras palabras, de AD FS en Windows Server 2012 R2, puedes aplicar el control de acceso condicional en función de los miembros del grupo o la identidad de usuario, una ubicación de red, un dispositivo (ya sea una empresa Unido, para obtener más información, consulta [unir al área de trabajo desde cualquier dispositivo para SSO y transparente segundo Factor de autenticación a través de aplicaciones de la compañía](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)) y el estado de autenticación (si se realizó la autenticación multifactor (AMF)).

Control de acceso condicional en ADFS en Windows Server 2012 R2, ofrece las siguientes ventajas:

-   Directivas de autorización por aplicación expresivos y flexible, mediante el cual puede permitir o denegar el acceso en función de usuario, dispositivo, ubicación de red y el estado de autenticación

-   Creación de reglas de autorización para aplicaciones de terceros de confianza de emisión

-   Experiencia de UI Rich para los escenarios comunes de control de acceso condicional

-   Admiten lenguaje enriquecido reclamaciones & Windows PowerShell para escenarios de control de acceso condicional avanzado

-   Personalizado (por usa la aplicación de terceros) mensajes de error "acceso denegado". Para obtener más información, consulta [personalización de las páginas de signo en AD FS](https://technet.microsoft.com/library/dn280950.aspx). Al poder personalizar estos mensajes, puedes explicar por qué un usuario se acceso denegado y también facilitan la corrección de autoservicio que es posible, por ejemplo, pedir a los usuarios empresa unirse a sus dispositivos. Para obtener más información, consulta [unir al área de trabajo desde cualquier dispositivo para SSO y transparente segundo Factor de autenticación a través de aplicaciones de la compañía](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md).

La siguiente tabla incluye todos los tipos de notificación disponibles en AD FS en Windows Server 2012 R2 para usarse para implementar el control de acceso condicional.

|Tipo de notificación|Descripción|
|--------------|---------------|
|Dirección de correo electrónico|La dirección de correo electrónico del usuario.|
|Nombre|El nombre de usuario.|
|Nombre|El nombre único del usuario,|
|UPN|El nombre principal de usuario (UPN) del usuario.|
|Nombre común|El nombre común del usuario.|
|AD FS 1 x dirección de correo electrónico|La dirección de correo electrónico del usuario al interoperar con AD FS 1.1 o AD FS 1.0.|
|Grupo|El usuario es miembro de un grupo.|
|AD FS 1 x UPN|El UPN de usuario al interoperar con AD FS 1.1 o AD FS 1.0.|
|Función|Una función que el usuario tiene.|
|Apellidos|El apellido del usuario.|
|P PID|El identificador privado del usuario.|
|Id. de nombre|El identificador de nombre SAML del usuario.|
|Marca de tiempo de autenticación|Usado para mostrar la hora y fecha en que se ha autenticado al usuario.|
|Método de autenticación|El método que se usa para autenticar al usuario.|
|Denegar el SID del grupo solo|Como solo denegar grupo SID del usuario.|
|Denegar a solo SID principal|Como solo denegar principal SID del usuario.|
|Denegar solo principal SID del grupo|Como solo denegar grupo primario SID del usuario.|
|SID del grupo|El SID del grupo del usuario.|
|SID del grupo principal|El grupo principal SID del usuario.|
|SID principal|El SID principal del usuario.|
|Nombre de la cuenta de Windows|El nombre de cuenta de dominio del usuario en forma de dominio\nombre de usuario.|
|Está registrado el usuario|Usuario registrado para usar este dispositivo.|
|Identificador de dispositivo|Identificador del dispositivo.|
|Identificador de registro del dispositivo|Identificador de registro del dispositivo.|
|Nombre de pantalla de registro de dispositivo|Nombre para mostrar de registro del dispositivo.|
|Tipo de sistema operativo de dispositivo|Tipo de sistema operativo del dispositivo.|
|Versión del sistema operativo de dispositivo|Versión del sistema operativo del dispositivo.|
|Es dispositivo administrado|Un servicio de administración administra el dispositivo.|
|Reenvía cliente IP|Dirección IP del usuario.|
|Aplicación de cliente|Tipo de la aplicación cliente.|
|Agente de usuario de cliente|Tipo de dispositivo del cliente está usando para acceder a la aplicación.|
|Cliente IP|Dirección IP del cliente.|
|Ruta de acceso de extremo|Ruta de extremo absoluta que puede usarse para determinar activa frente a los clientes pasivos.|
|Proxy|Nombre DNS del proxy del servidor de federación que pasa la solicitud.|
|Identificador de la aplicación|Identificador de usuario de confianza.|
|Directivas de aplicación|Directivas de aplicación del certificado.|
|Identificador de clave de entidad emisora|La extensión de identificador de clave de entidad emisora del certificado que se inició un certificado emitido.|
|Restricción básica|Una de las restricciones básicas del certificado.|
|Uso mejorado de claves|Describe uno de los usos de clave mejorados del certificado.|
|Emisor|El nombre de la entidad de certificación que emitió el certificado X.509.|
|Nombre del emisor|El nombre completo del emisor del certificado.|
|Uso de claves|Uno de los usos de la clave del certificado.|
|No después|Fecha en la hora local tras el cual un certificado ya no es válido.|
|Nunca antes|La fecha en la hora local en el que un certificado es válido.|
|Directivas del certificado|Las directivas en las que se ha emitido el certificado.|
|Clave pública|Clave pública del certificado.|
|Datos sin procesar del certificado|Los datos sin procesar del certificado.|
|Nombre alternativo del firmante|Uno de los nombres alternativos del certificado.|
|Número de serie|El número de serie del certificado.|
|Algoritmo de firma|El algoritmo usado para crear la firma de un certificado.|
|Asunto|El firmante del certificado.|
|Identificador de clave del firmante|El identificador de clave de firmante del certificado.|
|Nombre del firmante|Nombre del sujeto completo de un certificado.|
|V2 Nombre de la plantilla|El nombre de la plantilla de certificado 2 versión utiliza wen emitir o renovar un certificado. Este es un valor específico de Microsoft.|
|V1 Nombre de la plantilla|El nombre de la plantilla de certificado de la versión 1 utilizado cuando emitir o renovar un certificado. Este es un valor específico de Microsoft.|
|Huella digital|Huella digital del certificado.|
|Versión 509 x|La versión de formato X.509 del certificado.|
|Inside red corporativa|Se usa para indicar si una solicitud se origina desde dentro de la red corporativa.|
|Hora de expiración de contraseña|Se usa para mostrar la hora en que expira la contraseña.|
|Días de expiración de contraseña|Usado para mostrar el número de días para la expiración de contraseña.|
|Actualizar la dirección URL de la contraseña|Usado para mostrar la dirección web del servicio de actualización de contraseña.|
|Referencias de métodos de autenticación|Se usa para indicar al métodos de autenticación usados para autenticar al usuario.|

## <a name="BKMK_2"></a>Administración de riesgos con Control de acceso condicional
Con la configuración disponible, existen muchas formas en que puede administrar riesgo mediante la implementación de control de acceso condicional.

### <a name="common-scenarios"></a>Escenarios comunes
Por ejemplo, imagina un escenario simple de implementar el control de acceso condicional en función de los datos del usuario grupo pertenencia para una aplicación concreta (confianza confianza de terceros). En otras palabras, configurar una regla de autorización de emisión en el servidor de federación para permitir que los usuarios que pertenecen a un determinado grupo en el anuncio acceso de dominio a una aplicación concreta que está protegida por AD FS.  Paso a paso por instrucciones detalladas (con la interfaz de usuario y de Windows PowerShell) para la implementación de este escenario se tratan en [guía paso a paso: administrar el riesgo con el Control de acceso condicional](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md). Para completar los pasos de este tutorial, debes configurar un entorno de laboratorio y sigue los pasos de [configurar el entorno de laboratorio de AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md).

### <a name="advanced-scenarios"></a>Escenarios avanzados
Otros ejemplos de implementación de control de acceso condicional en ADFS en Windows Server 2012 R2 incluyen lo siguiente:

-   Permitir el acceso a una aplicación protegida por AD FS solo si la identidad del usuario se ha validado con MFA

    Puedes usar el siguiente código:

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "PermitAccessWithMFA"
    c:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Permitir el acceso a una aplicación protegida por AD FS solo si la solicitud de acceso proviene de un dispositivo estén unidos a un área de trabajo que esté registrado para el usuario

    Puedes usar el siguiente código:

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "PermitAccessFromRegisteredWorkplaceJoinedDevice"
    c:[Type == "https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", Value =~ "^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Permitir el acceso a una aplicación protegida por AD FS solo si la solicitud de acceso proviene de un dispositivo estén unidos a un área de trabajo que esté registrado a un usuario cuya identidad se ha validado con MFA

    Puedes usar el siguiente código

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "RequireMFAOnRegisteredWorkplaceJoinedDevice"
    c1:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$"] &&
    c2:[Type == "https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", Value =~ "^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   Permitir el acceso extranet a una aplicación protegida por AD FS solo si la solicitud de acceso proviene de un usuario cuya identidad se ha validado con MFA.

    Puedes usar el siguiente código:

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "RequireMFAForExtranetAccess"
    c1:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$"] &&
    c2:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value =~ "^(?i)false$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

## <a name="see-also"></a>Consulta también
[Guía paso a paso: Administrar el riesgo con el Control de acceso condicional](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)<ph x="2">
</ph>[configurar el entorno de laboratorio de AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



