---
ms.assetid: aa892a85-f95a-4bf1-acbb-e3c36ef02b0d
title: Novedades en Servicios de federación de Active Directory (AD FS) para Windows Server 2016
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/22/2020
ms.topic: article
ms.openlocfilehash: a0407639f6c3efcbdc8f0353b0313101272351ac
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87938147"
---
# <a name="whats-new-in-active-directory-federation-services"></a>Novedades de Servicios de federación de Active Directory (AD FS)


## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2019"></a>Novedades en los Servicios de federación de Active Directory (AD FS) para Windows Server 2019

### <a name="protected-logins"></a>Inicios de sesión protegidos

A continuación se ofrece un breve resumen de las actualizaciones para los inicios de sesión protegidos disponibles en AD FS 2019:
- **Proveedores de autenticación externos como principales**: los clientes ahora pueden usar productos de autenticación de terceros como primera fase sin exponer contraseñas como primera fase. En los casos en los que un proveedor de autenticación externo puede comprobar dos factores, puede recibir la notificación de MFA.
- **Autenticación de contraseña como autenticación adicional**: los clientes tienen una opción de bandeja de entrada totalmente compatible para usar solo una contraseña como factor adicional después de usar la opción sin contraseña como primera fase. Esta opción mejora la experiencia del cliente en comparación con AD FS 2016, donde los clientes tenían que descargar un adaptador de GitHub, que se admite tal cual.
- **Módulo de evaluación de riesgos acoplable**: los clientes ahora pueden crear sus propios módulos acoplables para bloquear determinados tipos de solicitudes durante la fase de autenticación previa. Esto facilita a los clientes el uso de la inteligencia en la nube (como Identity Protection) para bloquear los inicios de sesión para usuarios o transacciones de riesgo.  Para obtener más información, consulta [Creación de complementos con el modelo de evaluación de riesgos de AD FS 2019](../../ad-fs/development/ad-fs-risk-assessment-model.md)
- **Mejoras de ESL**: mejora ESL QFE en 2016 al agregar las siguientes funcionalidades
    - Permite que los clientes se encuentren en modo auditoría mientras conservan la funcionalidad de bloqueo de extranet "clásica" disponible desde AD FS 2012 R2. Actualmente, los clientes de la versión 2016 no tienen protección mientras están en modo auditoría.
    - Habilita el umbral de bloqueo independiente para las ubicaciones conocidas. Esto permite que varias instancias de aplicaciones que se ejecutan con una cuenta de servicio común sustituyan las contraseñas con el menor impacto posible.


### <a name="additional-security-improvements"></a>Mejoras de seguridad adicionales

Las siguientes mejoras de seguridad adicionales están disponibles en AD FS 2019:
- **PowerShell (PSH) remoto mediante el inicio de sesión de tarjeta inteligente**: los clientes ahora pueden usar tarjetas inteligentes para conectarse de forma remota a AD FS a través de PSH, y usarlas para administrar todas las funciones de PSH, incluidos los cmdlets de PSH de varios nodos.
- **Personalización del encabezado HTTP**: ahora los clientes pueden personalizar los encabezados HTTP emitidos durante las respuestas de AD FS. Esto incluye los siguientes encabezados
     - HSTS: implica que los puntos de conexión de AD FS solo se pueden usar en puntos de conexión HTTPS para que los aplique un explorador compatible
     - x-frame-options: permite que los administradores de AD FS permitan a usuarios de confianza específicos insertar iFrames para páginas de inicio de sesión interactivas de AD FS. Esta opción debe usarse con cuidado y únicamente en hosts HTTPS.
     - Encabezado futuro: También se pueden configurar encabezados futuros adicionales.

Para obtener más información, consulta [Personalización de encabezados de respuesta de seguridad HTTP con AD FS 2019](../../ad-fs/operations/customize-http-security-headers-ad-fs.md)


### <a name="authenticationpolicy-capabilities"></a>Funcionalidades de autenticación o directiva

Las siguientes funcionalidades de autenticación y directivas forman parte de AD FS 2019:
- **Especificar el método de autenticación para la autenticación adicional por RP**: los clientes ahora pueden usar reglas de notificaciones para decidir qué proveedor de autenticación adicional debe invocarse como proveedor de autenticación adicional. Esto resulta útil en dos casos de uso
    - Los clientes están pasando de un proveedor de autenticación adicional a otro. De esta manera, cuando incorporan usuarios a un proveedor de autenticación más nuevo, pueden usar grupos para controlar a qué proveedor de autenticación adicional se llama.
    - Los clientes necesitan un proveedor de autenticación adicional específico (por ejemplo, un certificado) para ciertas aplicaciones.
- **Restringir la autenticación de dispositivos basada en TLS solo a las aplicaciones que lo requieran**: los clientes ahora pueden restringir las autenticaciones de dispositivo basadas en TLS de cliente solo a las aplicaciones que realizan el acceso condicional basado en dispositivos. Esto impide que se generen mensajes no deseados para la autenticación de dispositivos (o errores si la aplicación cliente no puede controlarla) para las aplicaciones que no requieren de la autenticación de dispositivo basada en TLS.
- **Compatibilidad con la actualización de MFA**: AD FS ahora es compatible con volver a realizar la credencial de segunda fase en función de la actualización de la credencial de segunda fase. Esto permite a los clientes realizar una transacción inicial con dos fases y solicitar la segunda solo de forma periódica. Esta situación solo está disponible para las aplicaciones que pueden proporcionar un parámetro adicional en la solicitud que no es un valor configurable en AD FS. Este parámetro es compatible con Azure AD cuando está configurada la opción "Remember my MFA for X days" (Recordar mi MFA durante X días) y la marca "supportsMFA" está establecida en true en la configuración de confianza de dominio federado de Azure AD.


### <a name="sign-in-sso-improvements"></a>Mejoras del SSO de inicio de sesión

Se han realizado las siguientes mejoras en el SSO de inicio de sesión de AD FS 2019:

- [Experiencia de usuario paginada con tema centrado](../operations/AD-FS-paginated-sign-in.md): AD FS ahora se ha cambiado a un flujo de experiencia de usuario paginada que permite a AD FS validar y proporcionar una experiencia de inicio de sesión más fluida. AD FS ahora usa una interfaz de usuario centrada (en lugar de una alineada del lado derecho de la pantalla). Es posible que necesites un logotipo e imágenes de fondo más recientes para alinearte con esta experiencia. Esto también refleja la funcionalidad ofrecida en Azure AD.
- **Corrección de errores: estado de SSO persistente para dispositivos con Windows 10 cuando se realiza la autenticación de PRT**: esto soluciona un problema en el que el estado de MFA no se conservaba al usar la autenticación de PRT para dispositivos Windows 10. Como resultado, a los usuarios finales se les pedían las credenciales de segunda fase (MFA) con frecuencia. La corrección también hace que la experiencia sea coherente cuando la autenticación de dispositivo se realiza correctamente a través de TLS cliente y a través del mecanismo de PRT.


### <a name="suppport-for-building-modern-line-of-business-apps"></a>Compatibilidad para crear aplicaciones de línea de negocio modernas

Se ha agregado la siguiente compatibilidad para crear aplicaciones de LOB modernas en AD FS 2019:

- **Flujo o perfil de dispositivo de OAuth**: AD FS ahora admite el perfil de flujo de dispositivo de OAuth para realizar inicios de sesión en dispositivos que no tienen un área expuesta de interfaz de usuario para admitir experiencias de inicio de sesión enriquecidas. Esto permite al usuario completar la experiencia de inicio de sesión en un dispositivo diferente. Esta funcionalidad es necesaria para la experiencia de la CLI de Azure en Azure Stack y se puede usar en otros casos.
- **Eliminación del parámetro "Resource"** : AD FS ahora ha retirado el requisito de especificar un parámetro de recurso, que coincide con las especificaciones actuales de OAuth. Los clientes ahora pueden proporcionar el identificador de la relación de confianza para usuario autenticado como parámetro de ámbito, junto con los permisos solicitados.
- **Encabezados CORS en respuestas de AD FS**: los clientes ahora pueden crear aplicaciones de una sola página que permitan a las bibliotecas JS del lado cliente validar la firma del id_token al consultar las claves de firma del documento de detección de OIDC en AD FS.
- **Compatibilidad con PKCE**: AD FS agrega compatibilidad con PKCE para proporcionar un flujo de código de autenticación seguro en OAuth. Esto agrega una capa adicional de seguridad a este flujo para evitar que se secuestre el código y se reproduzca desde un cliente diferente.
- **Corrección de errores: envío de notificación x5t y kid**: se trata de una corrección de errores secundaria. AD FS ahora envía también la notificación "kid" para indicar la sugerencia de identificador de clave para comprobar la firma. Anteriormente, AD FS solo enviaba esta información como una notificación "x5t".


### <a name="supportability-improvements"></a>Mejoras de compatibilidad

Las siguientes mejoras de compatibilidad no forman parte de AD FS 2019:
- **Envío de los detalles del error a los administradores de AD FS**: los administradores pueden configurar a los usuarios finales para que envíen registros de depuración relacionados con un error en la autenticación del usuario final que se almacenará como archivo comprimido para facilitar su consumo. Los administradores también pueden configurar una conexión SMTP para enviar automáticamente el archivo comprimido a una cuenta de correo electrónico de evaluación de errores o para crear automáticamente un vale basado en el correo electrónico.


### <a name="deployment-updates"></a>Actualizaciones de implementación

Las siguientes actualizaciones de implementación ahora están incluidas en AD FS 2019:
- **Nivel de comportamiento de la granja 2019**: al igual que con AD FS 2016, se requiere una nueva versión de nivel de comportamiento de granja para habilitar la nueva funcionalidad descrita anteriormente. Esto permite realizar los siguientes cambios:
    - 2012 R2 -> 2019
    - 2016 -> 2019


### <a name="saml-updates"></a>Actualizaciones de SAML

La siguiente actualización de SAML está incluida en AD FS 2019:
- **Corrección de errores: corrección de errores en la federación agregada**: se han producido numerosas correcciones de errores en torno a la compatibilidad con la federación agregada (por ejemplo, InCommon). Las correcciones se han realizado en torno a lo siguiente:
  - Mejora de la escala para un número elevado de entidades en el documento de metadatos de la federación agregada. Anteriormente, esto generaba un error "ADMIN0017".
  - Consulte mediante el parámetro "ScopeGroupID" a través del cmdlet Get-AdfsRelyingPartyTrustsGroup de PSH (PowerShell).
  - Control de condiciones de error en torno a entityID duplicados.


### <a name="azure-ad-style-resource-specification-in-scope-parameter"></a>Especificación de recursos de estilo Azure AD en el parámetro de ámbito

Anteriormente, AD FS necesitaba que el recurso y el ámbito deseados se incluyeran en parámetros independientes en cualquier solicitud de autenticación. Por ejemplo, una solicitud de OAuth típica tenía el siguiente aspecto: 7 **https:&#47;&#47;fs.contoso.com/adfs/oauth2/authorize?</br>response_type=code&client_id=claimsxrayclient&resource=urn:microsoft:</br>adfs:claimsxray&scope=oauth&redirect_uri=https:&#47;&#47;adfshelp.microsoft.com/</br> ClaimsXray/TokenResponse&prompt=login**

Con AD FS en Server 2019, ahora puedes pasar el valor de recurso insertado en el parámetro de ámbito. Esto es coherente con el modo en que también se puede realizar la autenticación en Azure AD.

Ahora, el parámetro de ámbito se puede organizar como una lista separada por espacios donde cada entrada está estructurada como recurso/ámbito.

> [!NOTE]
> Solo se puede especificar un recurso en la solicitud de autenticación. Si se incluye más de un recurso en la solicitud, AD FS devolverá un error y la autenticación no se realizará correctamente.


### <a name="proof-key-for-code-exchange-pkce-support-for-oauth"></a>Compatibilidad con la clave de prueba para intercambio de código (PKCE) de OAuth

Los clientes públicos de OAuth que usan la concesión de código de autorización son susceptibles de sufrir ataques de interceptación de código de autorización.  El ataque se describe correctamente en RFC 7636. Para mitigar este ataque, AD FS en Server 2019 admite la clave de prueba para intercambio de código (PKCE) para el flujo de concesión de código de autorización de OAuth.

Para aprovechar la compatibilidad con PKCE, esta especificación agrega parámetros adicionales a las solicitudes de autorización y token de acceso de OAuth 2.0.

![Clave de prueba](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/adfs2019.png)

A. El cliente crea y registra un secreto denominado "code_verifier" y deriva una versión transformada "t(code_verifier)" (a la que se hace referencia como "code_challenge"), que se envía en la solicitud de autorización de OAuth 2.0 junto con el método de transformación "t_m".

B. El punto de conexión de autorización responde como de costumbre pero registra "t(code_verifier)" y el método de transformación.

C. Después, el cliente envía el código de autorización en la solicitud de token de acceso como de costumbre, pero incluye el secreto "code_verifier" generado en (A).

D. AD FS transforma el secreto "code_verifier" y lo compara con "t(code_verifier)" de (B).  Se deniega el acceso si no son iguales.


#### <a name="faq"></a>Preguntas más frecuentes

> [!NOTE]
> Puedes encontrar este error en los registros de eventos del administrador de ADFS: Se ha recibido una solicitud de OAuth no válida. El cliente "NOMBRE" no tiene permiso para acceder al recurso con el ámbito "ugs".
> Para corregir este error:
> 1. Inicia la consola de administración de AD FS. Vaya a "Servicios > Descripciones de ámbito".
> 2. Haz clic con el botón derecho en "Descripciones de ámbito" y selecciona "Agregar descripción de ámbito"
> 3. Para el nombre, escribe "ugs" y haz clic en Aplicar > Aceptar
> 4. Inicie PowerShell como administrador.
> 5. Ejecuta el comando "Get-AdfsApplicationPermission". Busca el elemento ScopeNames: {openid, aza} que contenga ClientRoleIdentifier. Toma nota del elemento ObjectIdentifier.
> 6. Ejecuta el comando "Set-AdfsApplicationPermission -TargetIdentifier <ObjectIdentifier del paso 5> -AddScope "ugs"
> 7. Reinicia el servicio ADFS.
> 8. En el cliente: Reinicia el cliente. Se pedirá al usuario que aprovisione WHPB.
> 9. Si la ventana de aprovisionamiento no aparece, tienes que recopilar los registros de seguimiento de NGC y solucionar el problema.

**P.** ¿Puedo pasar el valor del recurso como parte del valor de ámbito, de la misma forma en que se realizan las solicitudes en Azure AD?</br>
**R.** Con AD FS en Server 2019, ahora puedes pasar el valor de recurso insertado en el parámetro de ámbito. Ahora, el parámetro de ámbito se puede organizar como una lista separada por espacios en la que cada entrada está estructurada como recurso/ámbito. Por ejemplo: **<crear una solicitud de ejemplo válida>**

**P.** ¿AD FS admite la extensión PKCE?</br>
**R.** AD FS en Server 2019 admite la clave de prueba para intercambio de código (PKCE) para el flujo de concesión de código de autorización de OAuth


## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>Novedades en Servicios de federación de Active Directory (AD FS) para Windows Server 2016

Si buscas información sobre versiones anteriores de AD FS, consulta los siguientes artículos: [AD FS en Windows Server 2012 o 2012 R2](../../active-directory-federation-services.md) y [AD FS 2.0](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd727958(v=ws.10))

 Servicios de federación de Active Directory (AD FS) proporciona inicio de sesión único y control de acceso en una amplia variedad de aplicaciones, como Office 365, aplicaciones SaaS basadas en la nube, y aplicaciones en la red corporativa.
* Para la organización de TI, permite proporcionar inicio de sesión y control de acceso a aplicaciones modernas y antiguas, en el entorno local y en la nube, basados en el mismo conjunto de credenciales y directivas.
* Para el usuario, proporciona un inicio de sesión sin problemas con las mismas credenciales de cuenta conocidas.
* Para el desarrollador, proporciona una manera sencilla de autenticar a los usuarios cuyas identidades se encuentran en el directorio de la organización, de forma que puedas centrar tus esfuerzos en la aplicación, no en la autenticación o la identidad.

En este artículo se describen las novedades de AD FS en Windows Server 2016 (AD FS 2016).


## <a name="eliminate-passwords-from-the-extranet"></a>Eliminación de contraseñas de la extranet

AD FS 2016 habilita tres nuevas opciones para el inicio de sesión sin contraseñas, lo que permite a las organizaciones evitar el riesgo de poner en peligro a causa de contraseñas suplantadas, filtradas o robadas.


### <a name="sign-in-with-azure-multi-factor-authentication"></a>Inicio de sesión con Azure Multi-Factor Authentication

AD FS 2016 se basa en las funcionalidades de la autenticación multifactor (MFA) de AD FS en Windows Server 2012 R2 al permitir el inicio de sesión con solo un código de Azure MFA, sin escribir primero un nombre de usuario y una contraseña.

* Con Azure MFA como método de autenticación principal, se solicita al usuario su nombre de usuario y el código de OTP de la aplicación Azure Authenticator.
* Con Azure MFA como método de autenticación secundario o adicional, el usuario proporciona las credenciales de autenticación principal (mediante la autenticación integrada de Windows, un nombre de usuario y contraseña, una tarjeta inteligente, o un certificado de usuario o dispositivo) y, a continuación, se le solicita un texto, voz o inicio de sesión de Azure MFA basado en OTP.
* Con el nuevo adaptador de Azure MFA integrado, el programa de instalación y configuración de Azure MFA con AD FS nunca ha sido más sencillo.
* Las organizaciones pueden aprovechar Azure MFA sin necesidad de un servidor de Azure MFA local.
* Azure MFA puede configurarse para la intranet o extranet, o como parte de cualquier directiva de control de acceso.

Para obtener más información acerca de Azure MFA con AD FS
*  [Configuración de AD FS 2016 y Azure MFA](../operations/configure-ad-fs-and-azure-mfa.md)


### <a name="password-less-access-from-compliant-devices"></a>Acceso sin contraseña desde dispositivos compatibles

AD FS 2016 se basa en las funcionalidades de registro de dispositivos anteriores para habilitar el inicio de sesión y el control de acceso en función del estado de cumplimiento del dispositivo. Los usuarios pueden iniciar sesión con las credenciales del dispositivo y el cumplimiento se vuelve a evaluar cuando cambian los atributos del dispositivo, de modo que siempre se garantiza la aplicación de las directivas.  Esto habilita directivas como las siguientes

* Habilitar el acceso solo desde dispositivos administrados y/o conformes
* Habilitar el acceso a extranet solo desde dispositivos administrados y/o conformes
* Solicitar la autenticación multifactor para equipos que no están administrados o conformes

AD FS proporciona el componente local de las directivas de acceso condicional en un escenario híbrido. Al registrar los dispositivos con Azure AD para el acceso condicional a los recursos en la nube, la identidad del dispositivo puede usarse también para las directivas de AD FS.

![Novedades](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/ADFS_ITPRO4.png)

 Para obtener más información sobre el uso del acceso condicional basado en dispositivos en la nube
 * [Acceso condicional de Azure Active Directory](/azure/active-directory/conditional-access/overview)

Para obtener más información sobre el uso del acceso condicional basado en dispositivos con AD FS
* [Planeación del acceso condicional basado en dispositivos con AD FS](../../ad-fs/deployment/Plan-Device-based-Conditional-Access-on-Premises.md)
* [Directivas de control de acceso en AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)


### <a name="sign-in-with-windows-hello-for-business"></a>Inicio de sesión con Windows Hello para empresas

> [!NOTE]
> Actualmente, Google Chrome y los nuevos exploradores del proyecto de código libre de [Microsoft Edge basado en Chromium](https://www.microsoft.com/edge?form=MB110A&OCID=MB110A) no se admiten para el inicio de sesión único (SSO) basado en explorador con Microsoft Windows Hello para empresas. Usa Internet Explorer o una versión anterior de Microsoft Edge.

Los dispositivos Windows 10 presentan Windows Hello y Windows Hello para empresas, que reemplazan las contraseñas de usuario con credenciales de usuario seguras enlazadas a un dispositivo y protegidas por un gesto de usuario (un PIN, un gesto de biometría como una huella digital, o reconocimiento facial). AD FS 2016 admite estas nuevas funcionalidades de Windows 10 para que los usuarios puedan iniciar sesión en las aplicaciones de AD FS desde la intranet o la extranet sin necesidad de proporcionar una contraseña.

Para obtener más información acerca del uso de Microsoft Windows Hello para empresas en tu organización
* [Habilitar Windows Hello para empresas en la organización](/windows/security/identity-protection/hello-for-business/hello-identity-verification)


## <a name="secure-access-to-applications"></a>Acceso seguro a las aplicaciones

### <a name="modern-authentication"></a>Autenticación moderna

AD FS 2016 admite los protocolos modernos más recientes que proporcionan una mejor experiencia de usuario para Windows 10, así como para los dispositivos y aplicaciones iOS y Android más recientes.

Para obtener más información, consulta [Escenarios de AD FS para desarrolladores](../../ad-fs/overview/ad-fs-openid-connect-oauth-flows-scenarios.md).


### <a name="configure-access-control-policies-without-having-to-know-claim-rules-language"></a>Configuración de directivas de control de acceso sin tener que conocer el lenguaje de las reglas de notificaciones

Anteriormente, los administradores de AD FS tenían que configurar directivas mediante el lenguaje de reglas de notificaciones de AD FS, lo que dificultaba la configuración y mantenimiento de las directivas. Con las directivas de control de acceso, los administradores pueden usar las plantillas integradas para aplicar directivas comunes como las siguientes
* Permitir solo el acceso de intranet
* Permitir todos y requerir MFA de extranet
* Permitir todos y requerir MFA de extranet de un grupo específico

Las plantillas son fáciles de personalizar mediante un proceso controlado por asistente para agregar excepciones o reglas de directiva adicionales, y se pueden aplicar a una o varias aplicaciones para una aplicación coherente de directivas.

Para obtener más información, consulta [directivas de control de acceso en AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md).


### <a name="enable-sign-on-with-non-ad-ldap-directories"></a>Habilitación del inicio de sesión con directorios LDAP que no son de AD

Muchas organizaciones tienen una combinación de directorios de Active Directory y terceros. Con la adición de la compatibilidad de AD FS con la autenticación de usuarios almacenados en directorios compatibles con LDAP v3, AD FS ahora se puede usar para:
* Usuarios de directorios compatibles con LDAP v3 de terceros
* Usuarios de los bosques de Active Directory para los que no está configurada una confianza bidireccional de Active Directory
* Usuarios en Active Directory Lightweight Directory Services (AD LDS)

Para obtener más información, consulta [Configuración de AD FS para autenticar usuarios almacenados en directorios LDAP](../../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md).


## <a name="better-sign-in-experience"></a>Experiencia de inicio de sesión mejorada

### <a name="customize-sign-in-experience-for-ad-fs-applications"></a>Personalización de la experiencia de inicio de sesión para aplicaciones de AD FS

Nos comentaste que la capacidad de personalizar la experiencia de inicio de sesión para cada aplicación sería una mejora para la facilidad de uso, especialmente para las organizaciones que proporcionan inicio de sesión para aplicaciones que representan a varias empresas o marcas diferentes.

Anteriormente, AD FS en Windows Server 2012 R2 ofrecía una experiencia de inicio de sesión común para todas las aplicaciones de usuario de confianza, con la capacidad de personalizar un subconjunto de contenidos basados en texto por cada aplicación. Con Windows Server 2016, puedes personalizar no solo los mensajes, sino también las imágenes, el logotipo y el tema web por aplicación. Además, puedes crear nuevos temas web personalizados y aplicarlos por usuario de confianza.

Para obtener más información, consulta [Personalización del inicio de sesión del usuario de AD FS.](../../ad-fs/operations/AD-FS-user-sign-in-customization.md)


## <a name="manageability-and-operational-enhancements"></a>Mejoras operativas y de administración

En la siguiente sección se describen los escenarios operativos mejorados que se incluyen en Servicios de federación de Active Directory (AD FS) en Windows Server 2016.


### <a name="streamlined-auditing-for-easier-administrative-management"></a>Auditoría simplificada para facilitar la administración administrativa

En AD FS para Windows Server 2012 R2, se generaban muchos eventos de auditoría para una única solicitud y la información pertinente sobre una actividad de inicio de sesión o de emisión de tokens faltaba (en algunas versiones de AD FS) o se repartía en varios eventos de auditoría. De forma predeterminada, se desactivan los eventos de auditoría de AD FS debido a su naturaleza detallada.
Con el lanzamiento de AD FS 2016, la auditoría se ha vuelto más simplificada y menos detallada.

Para obtener más información, consulta [Mejoras de auditorías para AD FS en Windows Server 2016.](../../ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server.md)


### <a name="improved-interoperability-with-saml-20-for-participation-in-confederations"></a>Mejora de la interoperabilidad con SAML 2.0 para participar en confederaciones

AD FS 2016 incluye compatibilidad con el protocolo SAML adicional, incluida la compatibilidad para importar relaciones de confianza basadas en metadatos que contienen varias entidades. Esto permite configurar AD FS para participar en confederaciones como la Federación de InCommon y otras implementaciones que se ajustan al estándar eGov 2.0.

Para obtener más información, consulta [Interoperabilidad mejorada con SAML 2.0.](../../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)


### <a name="simplified-password-management-for-federated-o365-users"></a>Administración simplificada de contraseñas para usuarios federados de O365

Puedes configurar Servicios de federación de Active Directory (AD FS) para enviar notificaciones de expiración de contraseña a las relaciones de confianza para usuario autenticado (aplicaciones) que tienen la protección de AD FS. La forma en que se usan estas notificaciones depende de la aplicación. Por ejemplo, con Office 365 como usuario de confianza, se han implementado actualizaciones en Exchange y Outlook para notificar a los usuarios federados sobre las contraseñas que pronto van a expirar.

Para obtener más información, consulta [Configuración de AD FS para enviar notificaciones de expiración de contraseña.](../../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)


### <a name="moving-from-ad-fs-in-windows-server-2012-r2-to-ad-fs-in-windows-server-2016-is-easier"></a>La migración de AD FS en Windows Server 2012 R2 a AD FS en Windows Server 2016 es más fácil

Anteriormente, la migración a una nueva versión de AD FS requería que se exportara la configuración de la granja anterior y se importara a una nueva granja de servidores en paralelo.

Ahora, es mucho más fácil pasar de AD  FS en Windows Server 2012 R2 a AD FS en Windows Server 2016. Basta con agregar un nuevo servidor de Windows Server 2016 a una granja de servidores de Windows Server 2012 R2, y la granja funcionará en el nivel de comportamiento de la granja de servidores de Windows Server 2012 R2, por lo que parece y se comporta igual que una granja de Windows Server 2012 R2.

A continuación, agrega los nuevos servidores de Windows Server 2016 a la granja, comprueba que funcionen y quita los servidores más antiguos del equilibrador de carga. Una vez que todos los nodos de la granja estén ejecutando Windows Server 2016, estarás listo para actualizar el nivel de comportamiento de la granja a 2016 y comenzar a usar las nuevas características.

Para obtener más información, consulta [Actualización a AD FS en Windows Server 2016.](../deployment/upgrading-to-ad-fs-in-windows-server.md)
