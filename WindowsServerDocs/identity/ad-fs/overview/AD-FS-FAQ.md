---
ms.assetid: acc9101b-841c-4540-8b3c-62a53869ef7a
title: Preguntas más frecuentes sobre AD FS
description: Preguntas más frecuentes sobre AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/29/2020
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2fce4c5669ff78a6d97cd65580db1a68bfe3a390
ms.sourcegitcommit: f305bc5f1c5a44dac62f4288450af19f351f9576
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "87118592"
---
# <a name="ad-fs-frequently-asked-questions-faq"></a>Preguntas más frecuentes (P+F) sobre AD FS


En la siguiente documentación se incluyen las preguntas más frecuentes en relación con los Servicios de federación de Active Directory (AD FS).  El documento se ha dividido en grupos según el tipo de pregunta.

## <a name="deployment"></a>Implementación

### <a name="how-can-i-upgrademigrate-from-previous-versions-of-ad-fs"></a>¿Cómo puedo realizar una actualización o migración desde versiones anteriores a AD FS?
Puedes actualizar AD FS mediante una de las siguientes acciones:


- De AD FS en Windows Server 2012 R2 a AD FS en Windows Server 2016 o posterior. Ten en cuenta que la metodología es la misma si vas a actualizar desde AD FS en Windows Server 2016 a AD FS en Windows Server 2019. 
    - [Actualización a AD FS en Windows Server 2016 mediante una base de datos WID](../deployment/upgrading-to-ad-fs-in-windows-server.md)
    - [Actualizar a AD FS en Windows Server 2016 mediante una base de datos SQL](../deployment/upgrading-to-ad-fs-in-windows-server-sql.md)
- De AD FS en Windows Server 2012 a AD FS en Windows Server 2012 R2.
    - [Migración a AD FS en Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486815(v=ws.11))
- De AD FS 2.0 a AD FS en Windows Server 2012
    - [Migración a AD FS en Windows Server 2012](../deployment/migrate-ad-fs-role-services-to-windows-server-2012.md)
- De AD FS 1.x a AD FS 2.0
    - [Actualización de AD.FS 1.x a AD FS 2.0](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff678035(v=ws.10))

Si necesitas realizar una actualización de AD FS 2.0 o 2.1 (Windows Server 2008 R2 o Windows Server 2012), debes usar los scripts integrados (que se encuentran en C:\Windows\ADFS).

### <a name="why-does-ad-fs-installation-require-a-reboot-of-the-server"></a>¿Por qué la instalación de AD FS requiere un reinicio del servidor?

La compatibilidad con HTTP/2 se agregó en Windows Server 2016, pero HTTP/2 no se puede usar para la autenticación de certificado de cliente.  Dado que muchos escenarios de AD FS hacen uso de la autenticación de certificado de cliente y un número significativo de clientes no admiten el reintento de solicitudes con HTTP/1.1, la configuración de la granja de servidores de AD FS vuelve a establecer la configuración HTTP del servidor local en HTTP/1.1.  Este proceso requiere un reinicio del servidor.  

### <a name="is-using-windows-2016-wap-servers-to-publish-the-ad-fs-farm-to-the-internet-without-upgrading-the-back-end-ad-fs-farm-supported"></a>¿Se admite el uso de servidores WAP de Windows 2016 para publicar la granja de servidores de AD FS en Internet sin necesidad de actualizar la granja de servidores de AD FS de back-end?
Sí, esta configuración se admite; sin embargo, no se admitirán nuevas características de AD FS 2016 en esta configuración.  Esta configuración está pensada para ser temporal durante la fase de migración de AD FS 2012 R2 a AD FS 2016 y no debe implementarse durante largos períodos de tiempo.

### <a name="is-it-possible-to-deploy-ad-fs-for-office-365-without-publishing-a-proxy-to-office-365"></a>¿Es posible implementar AD FS para Office 365 sin publicar ningún proxy en Office 365?
Sí, se admite, sin embargo, hay los problemas siguientes:

1. Deberás administrar manualmente la actualización de los certificados de firma de tokens, ya que Azure AD no podrá acceder a los metadatos de federación. Para obtener más información sobre cómo actualizar manualmente el certificado de firma de tokens, lee [Renovación de certificados de federación para Office 365 y Azure Active Directory](/azure/active-directory/connect/active-directory-aadconnect-o365-certs).
2. No podrás aprovechar los flujos de autenticación heredados (por ejemplo, el flujo de autenticación de proxy ExO).

### <a name="what-are-load-balancing-requirements-for-ad-fs-and-wap-servers"></a>¿Cuáles son los requisitos de equilibrio de carga para AD FS y los servidores WAP?

AD FS es un sistema sin estado. Por lo tanto, el equilibrio de carga es bastante sencillo para los inicios de sesión. A continuación, te proporcionamos algunas recomendaciones clave para los sistemas de equilibrio de carga.


- Los equilibradores de carga no DEBEN configurar con afinidad de IP. Esto puede suponer una carga innecesaria en un subconjunto de los servidores en ciertos escenarios de Exchange Online.
- Los equilibradores de carga no DEBEN terminar las conexiones HTTPS y volver a iniciar una nueva conexión con el servidor de ADFS.
- En su lugar, DEBEN garantizar que la dirección IP de conexión debe traducirse como la IP de origen en el paquete HTTP cuando se envía a AD FS. En el caso de que un equilibrador de carga no pueda enviar la IP de origen en el paquete HTTP, el equilibrador de carga DEBE agregar (o anexar en caso de que exista) la dirección IP al encabezado x-forwarded-for. Esto es necesario para poder controlar correctamente ciertas características relacionadas con la IP (IP prohibida, bloqueo inteligente de extranet, etc.) y puede dar lugar a una menor seguridad si no se configura de forma adecuada.
- Los equilibradores de carga DEBEN admitir SNI. En el caso de que no sea así, asegúrate de que AD FS esté configurado para crear enlaces HTTPS para administrar clientes que no admitan SNI.
- Los equilibradores de carga DEBEN usar el punto de conexión de sondeo de estado HTTP de AD FS para detectar si los servidores WAP o AD FS se encuentran en funcionamiento, y excluirlos en caso de que no se devuelva el error 200 (correcto).

### <a name="what-multi-forest-configurations-are-supported-by-ad-fs"></a>¿Qué configuraciones de varios bosques admite AD FS?

AD FS admite distintas configuraciones de varios bosques y se basa en la red de confianza de AD DS subyacente para autenticar a los usuarios en varios dominios de confianza. Te recomendamos encarecidamente las relaciones de confianza de bosques bidireccionales, ya que requieren una configuración más sencilla para garantizar que el subsistema de confianza funciona correctamente sin problemas. Además:

- En el caso de una confianza de bosque unidireccional, como un bosque de red perimetral que contiene identidades de asociados, te recomendamos que implementes AD FS en el bosque CORP y trates el bosque de red perimetral como otra relación de confianza del proveedor de notificaciones local conectada a través de LDAP. En este caso, la autenticación integrada de Windows no funcionará para los usuarios del bosque de red perimetral y se les pedirá que realicen la autenticación con contraseña, ya que es el único mecanismo admitido para LDAP. En el caso de que no puedas optar por esta opción, deberás configurar otro AD FS en el bosque de red perimetral y agregarlo como relación de confianza del proveedor de notificaciones en la instancia de AD FS del bosque CORP. Los usuarios deberán realizar la detección del dominio de inicio, pero tanto la autenticación integrada de Windows como la autenticación de contraseña funcionarán. Realiza los cambios adecuados en las reglas de emisión de AD FS del bosque de red perimetral, ya que AD FS en el bosque CORP no podrá obtener información adicional sobre el usuario del bosque de red perimetral.
- Aunque las relaciones de confianza a nivel de dominio se admiten y pueden funcionar, te recomendamos encarecidamente pasar a un modelo de confianza a nivel de bosque. Además, debes asegurarte de que el enrutamiento UPN y la resolución de nombres NETBIOS de los nombres funcionen con precisión.

>[!NOTE]  
>Si usas la autenticación optativa con una configuración de confianza bidireccional, asegúrate de que el usuario que es el autor de la llamada tenga el permiso "permitir la autenticación" en la cuenta de servicio de destino. 

### <a name="does-ad-fs-extranet-smart-lockout-support-ipv6"></a>¿La protección de bloqueo inteligente de extranet de AD FS admite IPv6?
Sí, se aceptan direcciones IPv6 de ubicaciones conocidas o desconocidas.


## <a name="design"></a>Diseño

### <a name="what-third-party-multi-factor-authentication-providers-are-available-for-ad-fs"></a>¿Qué proveedores de autenticación multifactor de terceros están disponibles para AD FS?
AD FS proporciona un mecanismo extensible para que los proveedores de MFA de terceros se integren. No hay ningún programa de certificación establecido para ello. Se supone que el proveedor ha realizado las validaciones necesarias antes del lanzamiento. 

La lista de proveedores que han enviado una notificación a Microsoft se publica en [proveedores de MFA para AD FS](../operations/Configure-Additional-Authentication-Methods-for-AD-FS.md).  Puede que siempre haya proveedores disponibles que no conozcamos y actualizaremos la lista a medida que los descubramos.

### <a name="are-third-party-proxies-supported-with-ad-fs"></a>¿Los proxies de terceros son compatibles con AD FS?
Sí, los proxies de terceros se pueden colocar delante de AD FS, pero cualquier proxy de terceros debe admitir el uso del [protocolo MS-ADFSPIP](/openspecs/windows_protocols/ms-adfspip/76deccb1-1429-4c80-8349-d38e61da5cbb) en lugar del Proxy de aplicación web.

A continuación, se muestra una lista de los proveedores de terceros que conocemos.  Puede que siempre haya proveedores disponibles que no conozcamos y actualizaremos la lista a medida que los descubramos.

- [Administrador de directivas de acceso de F5](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-third-party-integration-13-1-0/12.html#guid-1ee8fbb3-1b33-4982-8bb3-05ae6868d9ee)


### <a name="where-is-the-capacity-planning-sizing-spreadsheet-for-ad-fs-2016"></a>¿Dónde está la hoja de cálculo de tamaño de planeamiento de capacidad para AD FS 2016?
Puedes descargar la versión de AD FS 2016 de la hoja de cálculo [aquí](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
También se puede usar para AD FS en Windows Server 2012 R2.

### <a name="how-can-i-ensure-my-ad-fs-and-wap-servers-support-apples-atp-requirements"></a>¿Cómo puedo asegurarme de que los servidores WAP y AD FS admiten los requisitos de ATP de Apple?

Apple ha publicado un conjunto de requisitos denominado Seguridad de transporte de aplicaciones (ATS) que pueden afectar a las llamadas de las aplicaciones de iOS que se autentican en AD FS.  Para asegurarte de que los servidores WAP y AD FS son válidos, verifica que admitan los [requisitos para la conexión mediante ATS](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  
En concreto, debes verificar que los servidores WAP y AD FS admiten TLS 1.2 y que el conjunto de cifrado negociado de la conexión TLS admitirá la confidencialidad directa perfecta.

Puedes habilitar y deshabilitar SSL 2.0 y 3.0 y las versiones de TLS 1.0, 1.1 y 1.2 mediante el artículo [Administración de los protocolos SSL en AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md).

Para asegurarte de que los servidores WAP y AD FS negocian solo los conjuntos de cifrado TLS que admiten ATP, puedes deshabilitar todos los conjuntos de cifrado que no estén en la [lista de conjuntos de cifrado compatibles con ATP](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  Para ello, usa los [cmdlets de PowerShell de TLS de Windows](/powershell/module/tls/?view=win10-ps).

## <a name="developer"></a>Desarrollador

### <a name="when-generating-an-id_token-with-adfs-for-a-user-authenticated-against-ad-how-is-the-sub-claim-generated-in-the-id_token"></a>Al generar el elemento id_token con ADFS para un usuario autenticado en AD, ¿cómo se genera la notificación "sub" en el elemento id_token?
El valor de la notificación "sub" es el hash del id. de cliente y el valor de la notificación de delimitador.

### <a name="what-is-the-lifetime-of-the-refresh-tokenaccess-token-when-the-user-logs-in-via-a-remote-claims-provider-trust-over-ws-fedsaml-p"></a>¿Cuál es la duración del token de acceso o de actualización cuando el usuario inicia sesión a través de una relación de confianza de proveedor de notificaciones remotas a través de WS-FED/SAML-P?
La duración del token de actualización será la del token que AD FS recibió de la relación de confianza del proveedor de notificaciones remotas. A su vez, la duración del token de acceso será la del token del usuario de confianza para el que se emite el token de acceso.

### <a name="i-need-to-return-profile-and-email-scopes-as-well-in-addition-to-the-openid-scope-can-i-obtain-additional-information-using-scopes-how-to-do-it-in-ad-fs"></a>También necesito devolver los ámbitos de perfil y correo electrónico, además del ámbito OpenId. ¿Puedo obtener información adicional con los ámbitos? ¿Cómo puedo hacerlo en AD FS?
Puedes usar el elemento id_token personalizado para agregar información pertinente en el propio elemento id_token. Para obtener más información, consulta el artículo [Personalización de notificaciones para que se emitan en id_token](../development/Custom-Id-Tokens-in-AD-FS.md).

### <a name="how-to-issue-json-blobs-inside-jwt-tokens"></a>¿Cómo se emiten los blobs JSON en los tokens JWT?
Se agregó un valor ValueType especial ("<http://www.w3.org/2001/XMLSchema#json>") y un carácter de escape (\x22) en AD FS 2016. En el ejemplo siguiente se muestra la regla de emisión y también la salida final del token de acceso.

Ejemplo de regla de emisión:

    => issue(Type = "array_in_json", ValueType = "http://www.w3.org/2001/XMLSchema#json", Value = "{\x22Items\x22:[{\x22Name\x22:\x22Apple\x22,\x22Price\x22:12.3},{\x22Name\x22:\x22Grape\x22,\x22Price\x22:3.21}],\x22Date\x22:\x2221/11/2010\x22}");

Notificaciones emitidas en el token de acceso:

    "array_in_json":{"Items":[{"Name":"Apple","Price":12.3},{"Name":"Grape","Price":3.21}],"Date":"21/11/2010"}

### <a name="can-i-pass-resource-value-as-part-of-the-scope-value-like-how-requests-are-done-against-azure-ad"></a>¿Puedo pasar el valor del recurso como parte del valor de ámbito de la misma forma en que se realizan las solicitudes en Azure AD?
Con AD FS en Server 2019, ahora puedes pasar el valor del recurso insertado en el parámetro de ámbito. Ahora, el parámetro de ámbito se puede organizar como una lista separada por espacios en la que cada entrada está estructurada como recurso/ámbito. Por ejemplo:  
**< crear una solicitud de ejemplo válida >**

### <a name="does-ad-fs-support-pkce-extension"></a>¿AD FS admite la extensión PKCE?
AD FS en Server 2019 admite la clave de prueba para intercambio de código (PKCE) para el flujo de concesión de código de autorización de OAuth.

### <a name="what-permitted-scopes-are-supported-by-ad-fs"></a>¿Qué ámbitos permitidos admite AD FS?
- aza: si se usan las [extensiones de protocolo de OAuth 2.0 para los clientes de agente](/openspecs/windows_protocols/ms-oapxbc/2f7d8875-0383-4058-956d-2fb216b44706) y el parámetro de ámbito contiene el ámbito "aza", el servidor emite un nuevo token de actualización principal y lo establece en el campo refresh_token de la respuesta, además de establecer el campo refresh_token_expires_in en la duración del nuevo token de actualización principal, si se aplica uno.
- openid: permite que la aplicación solicite el uso del protocolo de autorización OpenID Connect.
- logon_cert: el ámbito logon_cert permite a una aplicación solicitar certificados de inicio de sesión, que se pueden usar para iniciar sesión de forma interactiva de usuarios autenticados. El servidor de AD FS omite el parámetro access_token de la respuesta y, en su lugar, proporciona una cadena de certificados CMS codificada en Base 64 o una respuesta de PKI completa de CMC. Puedes encontrar más información disponible [aquí](/openspecs/windows_protocols/ms-oapx/32ce8878-7d33-4c02-818b-6c9164cc731e). 
- user_impersonation: el ámbito de user_impersonation es necesario para solicitar correctamente un token de acceso "en nombre de" de AD FS. Para obtener más información sobre cómo usar este ámbito, consulta [Compilar una aplicación de varios niveles usando "en nombre de" (OBO) mediante OAuth con AD FS 2016](../../ad-fs/development/ad-fs-on-behalf-of-authentication-in-windows-server.md).
- vpn_cert: el ámbito de vpn_cert permite a una aplicación solicitar certificados VPN, que se pueden usar para establecer conexiones VPN mediante la autenticación EAP-TLS. Esto ya no se admite.
- email: permite que la aplicación solicite una notificación por correo electrónico para el usuario que ha iniciado sesión. Esto ya no se admite. 
- profile: permite que la aplicación solicite notificaciones relacionadas con el perfil para el usuario que ha iniciado sesión. Esto ya no se admite. 


## <a name="operations"></a>Operaciones

### <a name="how-do-i-replace-the-ssl-certificate-for-ad-fs"></a>¿Cómo puedo reemplazar el certificado SSL de AD FS?
El certificado SSL de AD FS no es el mismo que el certificado de comunicaciones del servicio AD FS que se encuentra en el complemento Administración de AD FS.  Para cambiar el certificado SSL de AD FS, debes usar PowerShell. Sigue las instrucciones del artículo siguiente:

[Administración de certificados SSL en AD FS y WAP 2016](../operations/manage-ssl-certificates-ad-fs-wap.md)

### <a name="how-can-i-enable-or-disable-tlsssl-settings-for-ad-fs"></a>¿Cómo puedo habilitar o deshabilitar la configuración de TLS/SSL para AD FS?
Para deshabilitar o habilitar los protocolos SSL y los conjuntos de cifrado, usa lo siguiente:

[Administración de los protocolos SSL en AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md)

### <a name="does-the-proxy-ssl-certificate-have-to-be-the-same-as-the-ad-fs-ssl-certificate"></a>¿El certificado SSL de proxy debe ser el mismo que el certificado SSL de AD FS?  
Usa las instrucciones siguientes con respecto al certificado SSL de proxy y de AD FS:


- Si el proxy se usa para redirigir mediante proxy las solicitudes de AD FS que usan la autenticación integrada de Windows, el certificado SSL del proxy debe ser el mismo (usar la misma clave) que el certificado SSL del servidor de federación.
- Si la propiedad "ExtendedProtectionTokenCheck" de AD FS está habilitada (la configuración predeterminada en AD FS), el certificado SSL del proxy debe ser el mismo (usar la misma clave) que el certificado SSL del servidor de federación.
- De lo contrario, el certificado SSL de proxy puede tener una clave diferente de la del certificado SSL de AD FS, pero debe cumplir los mismos [requisitos](./ad-fs-requirements.md).

### <a name="why-do-i-only-see-a-password-login-on-ad-fs-and-not-my-other-authentication-methods-that-i-have-configured"></a>¿Por qué solo veo un inicio de sesión con contraseña en AD FS y no los otros métodos de autenticación que he configurado? 
AD FS solo muestra un método de autenticación en la pantalla de inicio de sesión cuando la aplicación requiere explícitamente un URI de autenticación específico que se asigna a un método de autenticación configurado y habilitado. Esto se transmite en el parámetro "wauth" para las solicitudes de WS-Federation, y el parámetro "RequestedAuthnCtxRef" en una solicitud de protocolo SAML. Como resultado, solo se muestra el método de autenticación solicitado (por ejemplo, el inicio de sesión con contraseña).

Cuando AD FS se utiliza con Azure AD, es habitual que las aplicaciones envíen el parámetro prompt=login a Azure AD. Por este motivo, de forma predeterminada Azure AD realiza la solicitud de un inicio de sesión basado en una contraseña nueva en AD FS. Este es el motivo más común por el que verás un inicio de sesión con contraseña en AD FS dentro de la red o no verás ninguna opción para iniciar sesión con el certificado. Si quieres, puedes solucionarlo fácilmente. Para ello, realiza un cambio en la configuración del dominio federado en Azure AD. 

Para obtener información sobre cómo configurarlo, consulta [Compatibilidad con el parámetro prompt=login de Servicios de federación de Active Directory (AD FS)](../operations/AD-FS-Prompt-Login.md).

### <a name="how-can-i-change-the-ad-fs-service-account"></a>¿Cómo puedo cambiar la cuenta de servicio de AD FS?
Para cambiar la cuenta de servicio de AD FS, sigue las instrucciones que se indican en el cuadro de herramientas de AD FS [Módulo de PowerShell de la cuenta de servicio](https://github.com/Microsoft/adfsToolbox/tree/master/serviceAccountModule).

### <a name="how-can-i-configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>¿Cómo puedo configurar los exploradores para usar la Autenticación integrada de Windows (WIA) con AD FS?

Para obtener información sobre cómo configurar los exploradores, consulta [Configuración de los exploradores para usar la Autenticación integrada de Windows (WIA) con AD FS](../operations/Configure-AD-FS-Browser-WIA.md).

### <a name="can-i-turn-off-browserssoenabled"></a>¿Puedo desactivar el valor BrowserSsoEnabled?
Si no tienes directivas de control de acceso basadas en el dispositivo en AD FS o en la inscripción de certificado de Windows Hello para empresas mediante AD FS; puedes desactivar el valor BrowserSsoEnabled. El valor BrowserSsoEnabled permite que AD FS recopile un token de actualización principal (PRT) del cliente que contiene información del dispositivo. Sin esa autenticación del dispositivo, AD FS no funcionará en dispositivos Windows 10.

### <a name="how-long-are-ad-fs-tokens-valid"></a>¿Durante cuánto tiempo son válidos los tokens de AD FS?

A menudo, esta pregunta significa "¿durante cuánto tiempo obtienen los usuarios el inicio de sesión único (SSO) sin tener que escribir nuevas credenciales y cómo puedo controlarlo como administrador?".  Este comportamiento y los valores de configuración que lo controlan se describen en el artículo [Configuración de inicio de sesión único de AD FS](../operations/ad-fs-single-sign-on-settings.md).

A continuación, se enumeran las duraciones predeterminadas de las distintas cookies y los tokens (así como los parámetros que rigen las duraciones):

**Dispositivos registrados**

- Cookies de SSO y PRT: 90 días como máximo, regidas por PSSOLifeTimeMins. (El dispositivo proporcionado se usa al menos cada 14 días; lo controla DeviceUsageWindow).

- Token de actualización: se calcula en función de lo anterior para proporcionar un comportamiento coherente.

- access_token: 1 hora de forma predeterminada, según el usuario de confianza.

- id_token: igual que el token de acceso.

**Dispositivos no registrados**

- Cookies de SSO: 8 horas de forma predeterminada, regidas por SSOLifetimeMins.  Cuando la opción Mantener la sesión iniciada (KMSI) está habilitada, el valor predeterminado es 24 horas y se puede configurar a través de KMSILifetimeMins.


- Token de actualización: 8 horas de forma predeterminada. 24 horas con KMSI habilitado.

- access_token: 1 hora de forma predeterminada, según el usuario de confianza.

- id_token: igual que el token de acceso.

### <a name="does-ad-fs-support-http-strict-transport-security-hsts"></a>¿AD FS admite la seguridad de transporte estricta HTTP (HSTS)?  

La seguridad de transporte estricta HTTP (HSTS) es un mecanismo de la directiva de seguridad web que ayuda a mitigar los ataques de degradación del protocolo y el secuestro de cookies de los servicios que tienen puntos de conexión HTTP y HTTPS. Permite que los servidores web declaren que los exploradores web (u otros agentes de usuario que cumplen los requisitos) solo deben interactuar con esta mediante HTTPS y nunca a través del protocolo HTTP.

Todos los puntos de conexión de AD FS para el tráfico de autenticación web se abren exclusivamente a través de HTTPS.  Como resultado, AD FS mitiga de manera eficaz las amenazas que proporciona el mecanismo de la directiva de seguridad de transporte estricta HTTP (por naturaleza no existe ninguna degradación de HTTP, ya que no hay clientes de escucha en HTTP). Además, AD FS impide que las cookies se envíen a otro servidor con puntos de conexión del protocolo HTTP al proporcionar la marca segura a todas las cookies.

Por lo tanto, no es necesario implementar HSTS en un servidor de AD FS porque nunca se puede degradar.  Por motivos de cumplimiento, los servidores de AD FS cumplen estos requisitos porque nunca pueden usar HTTP y todas las cookies se marcan como seguras.

Además, AD FS 2016 (con las revisiones más recientes) y AD FS 2019 admiten la emisión del encabezado HSTS. Para obtener más información, consulta [Personalización de encabezados de respuesta de seguridad HTTP con AD FS](../operations/customize-http-security-headers-ad-fs.md)

### <a name="x-ms-forwarded-client-ip-does-not-contain-the-ip-of-the-client-but-contains-ip-of-the-firewall-in-front-of-the-proxy-where-can-i-get-the-right-ip-of-the-client"></a>El encabezado X-ms-forwarded-client-ip no contiene la dirección IP del cliente, sino la dirección IP del firewall que va delante del proxy. ¿Dónde puedo obtener la dirección IP correcta del cliente?
No te recomendamos que realices la terminación SSL antes de WAP. En caso de hacerlo, el encabezado X-ms-forwarded-client-ip contendrá la dirección IP del dispositivo de red delante de WAP. A continuación, se incluye una breve descripción de las distintas notificaciones relacionadas con IP que admite AD FS:
 - x-ms-client-ip : IP de red del dispositivo que se conectó a STS.  En el caso de una solicitud de extranet, esta siempre contiene la dirección IP del protocolo WAP.
 - x-ms-forwarded-client-ip: notificación de varios valores que contendrá los valores que reenvíe Exchange Online a AD FS y la dirección IP del dispositivo que se conectó a WAP.
 - Userip: para las solicitudes de extranet, esta notificación contendrá el encabezado x-ms-forwarded-client-ip.  En el caso de las solicitudes de intranet, esta notificación contendrá el mismo valor que x-ms-client-ip.

 Además, en AD FS 2016 (con las revisiones más recientes) y las versiones posteriores, también se admite la captura del encabezado x-forwarded-for. Cualquier equilibrador de carga o dispositivo de red que no se reenvíe en el nivel 3 (la IP se conserva) debe agregar la dirección IP de cliente entrante al encabezado x-forwarded-for estándar del sector. 

### <a name="i-am-trying-to-get-additional-claims-on-the-user-info-endpoint-but-its-only-returning-subject-how-can-i-get-additional-claims"></a>Estoy intentando obtener notificaciones adicionales sobre el punto de conexión de información del usuario, pero solo se devuelve el firmante. ¿Cómo puedo obtener notificaciones adicionales?
El punto de conexión userinfo de AD FS siempre devuelve la notificación del firmante tal y como se especifica en los estándares de OpenID. AD FS no proporciona notificaciones adicionales solicitadas a través del punto de conexión UserInfo. Si necesitas notificaciones adicionales en el token de identificador, consulta [Tokens de identificador personalizados en AD FS](../development/custom-id-tokens-in-ad-fs.md).

### <a name="why-do-i-see-a-lot-of-1021-errors-on-my-ad-fs-servers"></a>¿Por qué veo muchos errores 1021 en los servidores AD FS?
Normalmente, este evento se registra para un acceso no válido en AD FS al recurso 00000003-0000-0000-C000-000000000000. Este error se debe a un comportamiento erróneo del cliente en el que intenta obtener un token de acceso para el servicio Azure AD Graph. Puesto que el recurso no existe en AD FS, esto da como resultado el identificador de evento 1021 en los servidores de AD FS. Es seguro omitir cualquier advertencia o error para el recurso 00000003-0000-0000-C000-000000000000 en AD FS.

### <a name="why-am-i-seeing-a-warning-for-failure-to-add-the-ad-fs-service-account-to-the-enterprise-key-admins-group"></a>¿Por qué veo una advertencia de error al agregar la cuenta de servicio de AD FS al grupo administradores de claves de empresa?
Este grupo solo se crea cuando existe un controlador de dominio de Windows 2016 con el rol PDC de FSMO en el dominio. Para resolver el error, puedes crear el grupo manualmente y seguir los pasos que se indican a continuación para conceder el permiso necesario después de agregar la cuenta de servicio como miembro del grupo.
1.    Abra **Usuarios y equipos de Active Directory**.
2.    **Haz clic con el botón derecho** en el nombre de dominio del panel de navegación y **haz clic** en Propiedades.
3.    **Haz clic** en Seguridad (si falta la pestaña Seguridad, activa las Características avanzadas en el menú Vista).
4.    **Haz clic** en Avanzado. **Haz clic** en Agregar. **Haz clic** en Seleccionar una entidad de seguridad.
5.    Se abrirá el cuadro de diálogo Select User, Computer, Service Account, or Group (Seleccionar usuarios, equipos, cuentas de servicio o grupos).  En el cuadro de texto Enter the object name to select (Escribe el nombre del objeto que quieras seleccionar), escribe Key Admin Group (Grupo de administradores de claves).  Haga clic en Aceptar.
6.    En el cuadro de lista Se aplica a, selecciona **Descendant User objects** (Objetos de usuario descendientes).
7.    Con la barra de desplazamiento, desplázate a la parte inferior de la página y **haz clic** en Borrar todo.
8.    En la sección **Propiedades**, selecciona **Read msDS-KeyCredentialLink** (Leer msDS-KeyCredentialLink) y **Write msDS-KeyCredentialLink** (Escribir msDS-KeyCredentialLink).

### <a name="why-does-modern-authentication-from-android-devices-fail-if-the-server-does-not-send-all-the-intermediate-certificates-in-the-chain-with-the-ssl-cert"></a>¿Por qué se produce un error en la autenticación moderna de dispositivos Android si el servidor no envía todos los certificados intermedios de la cadena con el certificado SSL?

Los usuarios federados pueden experimentar un error en la autenticación en Azure AD para las aplicaciones que usan la biblioteca ADAL de Android. La aplicación obtendrá el valor **AuthenticationException** cuando intente mostrar la página de inicio de sesión. En Chrome, es posible que la página de inicio de sesión de AD FS se invoque como no segura.

Android: en todas las versiones y todos los dispositivos; no admite la descarga de certificados adicionales del campo **authorityInformationAccess** del certificado. Esto también se aplica al explorador Chrome. Cualquier certificado de autenticación de servidor que no tenga certificados intermedios producirá este error si la cadena de certificados completa no se pasa desde AD FS.

Una solución adecuada para este problema es configurar los servidores AD FS y WAP para enviar los certificados intermedios necesarios junto con el certificado SSL.

Al exportar el certificado SSL, desde una máquina, para importarlo en el almacén personal del equipo, de los servidores WAP y AD FS, asegúrate de exportar la clave privada y selecciona **Personal Information Exchange - PKCS #12** (Intercambio de información personal: PKCS n.º 12).

Es importante seleccionar la casilla **Include all certificates in the certificate path if possible** (Incluir todos los certificados en la ruta de acceso del certificado si es posible), así como **Exportar todas las propiedades extendidas**.  

Ejecuta certlm.msc en los servidores de Windows e importa el archivo *.PFX en el almacén de certificados personal del equipo. De este modo, el servidor pasará toda la cadena de certificados a la biblioteca ADAL.

>[!NOTE]
> El almacén de certificados de los equilibradores de carga de red también debe actualizarse para incluir toda la cadena de certificados si existe.

### <a name="does-ad-fs-support-head-requests"></a>¿AD FS admite las solicitudes HEAD?
AD FS no admite las solicitudes HEAD.  Las aplicaciones no deben usar solicitudes HEAD en puntos de conexión de AD FS.  Es posible que se produzcan respuestas de error HTTP inesperadas o diferidas.  Además, es posible que veas eventos de error inesperados en el registro de eventos de AD FS.

### <a name="why-am-i-not-seeing-a-refresh-token-when-i-am-logging-in-with-a-remote-idp"></a>¿Por qué no veo un token de actualización cuando inicio sesión con un IdP remoto?
No se emite ningún token de actualización si el token que emite el IdP tiene una validez de menos de una hora. Para asegurarte de que se emite un token de actualización, aumenta la validez del token que emite el IdP a más de una hora.

### <a name="do-we-have-any-way-to-change-rp-token-encryption-algorithm"></a>¿Hay alguna manera de cambiar el algoritmo de cifrado de tokens RP?
De forma predeterminada, el cifrado de tokens RP se establece en AES256 y no se puede cambiar a ningún otro valor.

### <a name="on-a-mixed-mode-farm-i-get-error-when-trying-to-set-the-new-ssl-certificate-using-set-adfssslcertificate--thumbprint-how-can-i-update-the-ssl-certificate-in-a-mixed-mode-ad-fs-farm"></a>En una granja de servidores de modo mixto, obtengo un error al intentar establecer el nuevo certificado SSL mediante Set-AdfsSslCertificate -Thumbprint. ¿Cómo puedo actualizar el certificado SSL en una granja de servidores de AD FS de modo mixto?
Las granjas de servidores de AD FS de modo mixto están pensadas para ser un estado de transición. Te recomendamos que, durante la planificación, sustituyas el certificado SSL antes del proceso de actualización o completes el proceso y aumentes el nivel de comportamiento de la granja de servidores antes de actualizar el certificado SSL. En caso de que no hacerlo, en las siguientes instrucciones se describe cómo se puede actualizar el certificado SSL. 

En los servidores WAP, puedes seguir usando Set-WebApplicationProxySslCertificate. En los servidores AD FS, debes utilizar netsh. Sigue los pasos que se indican a continuación:

1. Selecciona el subconjunto de servidores de AD FS 2016 para su mantenimiento (por ejemplo, quítalo del equilibrador de carga).
2. En los servidores seleccionados en el paso 1, importa el nuevo certificado a través de MMC.
3. Elimina los certificados existentes.

    a. netsh http delete sslcert hostnameport=fs.contoso.com:443 b. netsh http delete sslcert hostnameport=localhost:443 c. netsh http delete sslcert hostnameport=fs.contoso.com:49443

4.  Agrega el nuevo certificado.

    a. netsh http add sslcert hostnameport=fs.contoso.com:443 certhash=CERTTHUMBPRINT appid={5d89a20c-beab-4389-9447-324788eb944a} certstorename=MY sslctlstorename=AdfsTrustedDevices

    b. netsh http add sslcert hostnameport=localhost:443 certhash=CERTTHUMBPRINT appid={5d89a20c-beab-4389-9447-324788eb944a} certstorename=MY sslctlstorename=AdfsTrustedDevices

    c. netsh http add sslcert hostnameport=fs.contoso.com:49443 certhash=CERTTHUMBPRINT appid={5d89a20c-beab-4389-9447-324788eb944a} certstorename=MY sslctlstorename=AdfsTrustedDevices

5. Reinicia el servicio de AD FS en el servidor seleccionado.
6. Quita subconjunto de servidores WAP para su mantenimiento.
7. En los servidores WAP seleccionados, importa el nuevo certificado a través de MMC.
8. Establece el nuevo certificado en WAP mediante el cmdlet.

    a. Set-WebApplicationProxySslCertificate -Thumbprint " CERTTHUMBPRINT"

9. Reinicia el servicio en los servidores WAP seleccionados.
10. Vuelve a colocar los servidores WAP y AD FS seleccionados en el entorno de producción.

Realiza la actualización en el resto de servidores WAP y AD FS de manera similar.

### <a name="is-adfs-supported-when-web-application-proxy-wap-servers-are-behind-azure-web-application-firewallwaf"></a>¿Se admite AD FS cuando los servidores del Proxy de aplicación web (WAP) están detrás del Firewall de aplicaciones web (WAF) de Azure?
Los servidores de aplicaciones web y AD FS admiten cualquier firewall que no realice la terminación SSL en el punto de conexión. Además, los servidores AD FS y WAP tienen mecanismos integrados para prevenir ataques web comunes, como el scripting entre sitios y el proxy de AD FS, y cumplir con todos los requisitos que define el [protocolo MS-ADFSPIP](/openspecs/windows_protocols/ms-adfspip/76deccb1-1429-4c80-8349-d38e61da5cbb).

### <a name="i-am-seeing-an-event-441-a-token-with-a-bad-token-binding-key-was-found-what-should-i-do-to-resolve-this"></a>Veo el mensaje "Evento 441: Se encontró un token con una clave de enlace de tokens incorrecta". ¿Qué debo hacer para solucionarlo?
En AD FS 2016, el enlace de tokens se habilita automáticamente y provoca varios problemas conocidos con los escenarios de proxy y de federación que producen este error. Para resolverlo, ejecuta el siguiente comando de PowerShell y quita la compatibilidad con el enlace de tokens.

`Set-AdfsProperties -IgnoreTokenBinding $true`

### <a name="i-have-upgraded-my-farm-from-ad-fs-in-windows-server-2016-to-ad-fs-in-windows-server-2019-the-farm-behavior-level-for-the-ad-fs-farm-has-been-successfully-raised-to-2019-but-the-web-application-proxy-configuration-is-still-displayed-as-windows-server-2016"></a>He actualizado mi granja de servidores de AD FS en Windows Server 2016 a AD FS en Windows Server 2019. El nivel de comportamiento de la granja de servidores de AD FS se ha generado correctamente en 2019, pero la configuración del Proxy de aplicación web todavía se muestra como Windows Server 2016?
Después de realizar una actualización a Windows Server 2019, la versión de configuración del Proxy de aplicación web se seguirá mostrando como Windows Server 2016. El Proxy de aplicación web no tiene nuevas características específicas de la versión para Windows Server 2019 y, si el nivel de comportamiento de la granja de servidores se ha generado correctamente en AD FS, el Proxy de aplicación web seguirá mostrando Windows Server 2016 por naturaleza.

### <a name="can-i-estimate-the-size-of-the-adfsartifactstore-before-enabling-esl"></a>¿Puedo calcular el tamaño de ADFSArtifactStore antes de habilitar ESL?
Con ESL habilitado, AD FS realiza un seguimiento de la actividad de la cuenta y de las ubicaciones conocidas de los usuarios en la base de datos ADFSArtifactStore. Esta base de datos se escala en función del número de usuarios y de las ubicaciones conocidas a las que se realiza un seguimiento. Al planificar la habilitación de ESL, puedes calcular el tamaño de la base de datos ADFSArtifactStore para que crezca a una velocidad de hasta 1 GB por 100 000 usuarios. Si la granja de servidores de AD FS usa Windows Internal Database (WID), la ubicación predeterminada de los archivos de la base de datos será C:\Windows\WID\Data. Para evitar que esta unidad se llene, asegúrate de tener un mínimo de 5 GB de almacenamiento disponible antes de habilitar ESL. Además del almacenamiento en disco, planifica un aumento de la memoria de proceso total después de habilitar ESL de hasta un 1 GB de RAM adicional para los rellenados de usuarios de 500 000 o menos.

### <a name="i-am-seeing-event-570-active-directory-trust-enumeration-was-unable-to-enumerate-one-of-more-domains-due-to-the-following-error-enumeration-will-continue-but-the-active-directory-identifier-list-may-not-be-correct-validate-that-all-expected-active-directory-identifiers-are-present-by-running-get-adfsdirectoryproperties-on-ad-fs-2019-what-is-the-mitigation-for-this-event"></a>Veo el evento 570 (La enumeración de confianza de Active Directory no pudo enumerar uno de varios dominios debido al siguiente error. La enumeración continuará, pero puede que la lista de identificadores de Active Directory no sea correcta. Ejecute Get-ADFSDirectoryProperties para comprobar que todos los identificadores de Active Directory previstos están presentes) en AD FS 2019. ¿Cuál es la mitigación de este evento?
Este evento se produce cuando AD FS intenta enumerar todos los bosques en una cadena de bosques de confianza y se conecta a través de todos los bosques, pero los bosques no son de confianza. Por ejemplo, si el bosque A y el bosque B son de confianza, y el bosque B y el bosque C son de confianza, AD FS enumerará los tres bosques e intentará encontrar una relación de confianza entre el bosque A y el C. Si AD FS tiene que autenticar a los usuarios del bosque con errores, configura una relación de confianza entre el bosque de AD FS y el bosque con errores. Si AD FS no tiene que autenticar a los usuarios del bosque con errores, omite este error.

### <a name="i-am-seeing-an-event-id-364-microsoftidentityserverauthenticationfailedexception-msis5015-authentication-of-the-presented-token-failed-token-binding-claim-in-token-must-match-the-binding-provided-by-the-channel-what-should-i-do-to-resolve-this"></a>Veo el mensaje "Evento 364: Microsoft.IdentityServer.AuthenticationFailedException: MSIS5015: no se pudo autenticar el token presentado. La notificación de enlace de token debe coincidir con el enlace proporcionado por el canal." ¿Qué debo hacer para solucionarlo?
En AD FS 2016, el enlace de tokens se habilita automáticamente y provoca varios problemas conocidos con los escenarios de proxy y de federación que producen este error. Para resolverlo, ejecuta el siguiente comando de PowerShell y quita la compatibilidad con el enlace de tokens.

`Set-AdfsProperties -IgnoreTokenBinding $true`
