---
ms.assetid: acc9101b-841c-4540-8b3c-62a53869ef7a
title: PREGUNTAS MÁS FRECUENTES SOBRE AD FS
description: Preguntas más frecuentes sobre AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/17/2019
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 48d93f515a5f3e5f8ce2c3ff9a1b40f300ca57ed
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265747"
---
# <a name="ad-fs-frequently-asked-questions-faq"></a>AD FS preguntas más frecuentes (p + f)


La siguiente documentación es un hogar de las preguntas más frecuentes con respecto a Servicios de federación de Active Directory (AD FS).  El documento se ha dividido en grupos según el tipo de pregunta.

## <a name="deployment"></a>Implementación

### <a name="how-can-i-upgrademigrate-from-previous-versions-of-ad-fs"></a>¿Cómo puedo actualizar o migrar desde versiones anteriores de AD FS
Puede actualizar AD FS mediante una de las siguientes acciones:


- Windows Server 2012 R2 AD FS a Windows Server 2016 AD FS o posterior. Tenga en cuenta que la metodología es la misma si va a actualizar desde Windows Server 2016 AD FS a Windows Server 2019 AD FS. 
    - [Actualización a AD FS en Windows Server 2016 mediante una base de datos WID](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)
    - [Actualizar a AD FS en Windows Server 2016 mediante una base de datos SQL](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL.md)
- Windows Server 2012 AD FS a Windows Server 2012 R2 AD FS
    - [Migrar a AD FS en Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- AD FS 2,0 a Windows Server 2012 AD FS
    - [Migrar a AD FS en Windows Server 2012](https://technet.microsoft.com/library/jj647765.aspx)
- AD FS 1. x a AD FS 2,0
    - [Actualización de AD FS 1. x a AD FS 2,0](https://technet.microsoft.com/library/ff678035.aspx)

Si necesita actualizar desde AD FS 2,0 o 2,1 (Windows Server 2008 R2 o Windows Server 2012), debe usar los scripts integrados (que se encuentran en C:\Windows\ADFS).

### <a name="why-does-ad-fs-installation-require-a-reboot-of-the-server"></a>¿Por qué AD FS instalación requiere un reinicio del servidor?

La compatibilidad con HTTP/2 se agregó en Windows Server 2016, pero HTTP/2 no se puede usar para la autenticación de certificados de cliente.  Dado que muchos AD FS escenarios hacen uso de la autenticación de certificados de cliente y un número significativo de clientes no admiten el reintento de solicitudes con HTTP/1.1, la configuración de la granja de servidores de AD FS vuelve a configurar la configuración HTTP del servidor local en HTTP/1.1.  Esto requiere un reinicio del servidor.  

### <a name="is-using-windows-2016-wap-servers-to-publish-the-ad-fs-farm-to-the-internet-without-upgrading-the-back-end-ad-fs-farm-supported"></a>¿Usa servidores WAP de Windows 2016 para publicar la granja de AD FS en Internet sin necesidad de actualizar la granja de servidores de AD FS de back-end compatible?
Sí, se admite esta configuración; sin embargo, no se admitirán nuevas características AD FS 2016 en esta configuración.  Esta configuración está pensada para ser temporal durante la fase de migración desde AD FS 2012 R2 a AD FS 2016 y no debe implementarse durante largos períodos de tiempo.

### <a name="is-it-possible-to-deploy-ad-fs-for-office-365-without-publishing-a-proxy-to-office-365"></a>¿Es posible implementar AD FS para Office 365 sin publicar un proxy en Office 365?
Sí, este procedimiento se admite. Sin embargo, como efecto secundario

1. Deberá administrar manualmente los certificados de firma de tokens de actualización, ya que Azure AD no podrán tener acceso a los metadatos de Federación. Para obtener más información sobre la actualización manual del certificado de firma de tokens [, lea renovación de certificados de Federación para Office 365 y Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs)
2. No podrá aprovechar los flujos de autenticación heredados (por ejemplo, el flujo de autenticación de proxy de ExO)

### <a name="what-are-load-balancing-requirements-for-ad-fs-and-wap-servers"></a>¿Cuáles son los requisitos de equilibrio de carga para AD FS y los servidores WAP?

AD FS es un sistema sin estado. Por lo tanto, el equilibrio de carga es bastante sencillo para los inicios de sesión. Las siguientes son recomendaciones clave para los sistemas de equilibrio de carga.


- Los equilibradores de carga no se deben configurar con afinidad de IP. Esto puede suponer una carga innecesaria en un subconjunto de los servidores en ciertos escenarios de Exchange Online.
- Los equilibradores de carga no deben terminar las conexiones HTTPS y volver a iniciar una nueva conexión con el servidor de ADFS.
- Los equilibradores de carga deben asegurarse de que la dirección IP de conexión debe traducirse como la IP de origen en el paquete HTTP cuando se envía a ADFS. En caso de que un equilibrador de carga no pueda enviar la IP de origen en el paquete HTTP, el equilibrador de carga debe agregar (o anexar en caso de que exista) la dirección IP al encabezado x-forwarded-for. Esto es necesario para el control correcto de ciertas características relacionadas con IP (IP prohibida,,... de bloqueo inteligente de extranet) y puede dar lugar a una menor seguridad si se ha configurado incorrectamente.
- Los equilibradores de carga deben admitir SNI. En caso de que no sea así, asegúrese de que AD FS esté configurado para crear enlaces HTTPS para administrar clientes que no admiten SNI.
- Los equilibradores de carga deben usar el punto de conexión de sondeo de mantenimiento de HTTP AD FS para detectar si los servidores de AD FS o WAP están en funcionamiento y excluirlos si no se devuelve 200 aceptar.

### <a name="what-multi-forest-configurations-are-supported-by-ad-fs"></a>¿Qué configuraciones de varios bosques admite AD FS?

AD FS admite varias configuraciones de varios bosques y se basa en la red de confianza de AD DS subyacente para autenticar a los usuarios en varios dominios de confianza. Nos recomendamos encarecidamente que confíe en los bosques bidireccionales, ya que es una configuración más sencilla para garantizar que el subsistema de confianza funcione correctamente sin problemas. Además:

- En el caso de una confianza de bosque unidireccional, como un bosque DMZ que contiene identidades de asociados, se recomienda implementar ADFS en el bosque Corp y tratar el bosque DMZ como otra relación de confianza para proveedor de notificaciones local conectada a través de LDAP. En este caso, la autenticación integrada de Windows no funcionará para los usuarios del bosque DMZ y se les pedirá que realice la autenticación con contraseña, ya que es el único mecanismo admitido para LDAP. En caso de que no pueda seguir esta opción, deberá configurar otro ADFS en el bosque DMZ y agregarlo como confianza del proveedor de notificaciones en ADFS en el bosque Corp. Los usuarios deberán realizar la detección del dominio de inicio, pero la autenticación integrada de Windows y la autenticación de contraseña funcionarán. Realice los cambios adecuados en las reglas de emisión en ADFS en el bosque de la red perimetral, ya que ADFS en el bosque Corp no podrá obtener información adicional sobre el usuario del bosque DMZ.
- Aunque se admiten las confianzas de nivel de dominio y pueden funcionar, se recomienda encarecidamente pasar a un modelo de confianza de nivel de bosque. Además, debe asegurarse de que el enrutamiento UPN y la resolución de nombres NETBIOS de los nombres deben funcionar con precisión.

>[!NOTE]  
>Si se usa la autenticación optativa con una configuración de confianza bidireccional, asegúrese de que el usuario que llama tenga el permiso "permitir la autenticación" en la cuenta de servicio de destino. 


## <a name="design"></a>Diseñar

### <a name="what-third-party-multi-factor-authentication-providers-are-available-for-ad-fs"></a>¿Qué proveedores de autenticación multifactor de terceros están disponibles para AD FS?
AD FS proporciona un mecanismo extensible para que los proveedores de MFA de terceros se integren. No hay ningún programa de certificación establecido para este. Se supone que el proveedor ha realizado las validaciones necesarias antes del lanzamiento. 

La lista de proveedores que han notificado a Microsoft está publicada en [proveedores de MFA para AD FS](../operations/Configure-Additional-Authentication-Methods-for-AD-FS.md).  Siempre puede haber proveedores disponibles que no conocemos y actualizaremos la lista a medida que nos conozcamos.

### <a name="are-third-party-proxies-supported-with-ad-fs"></a>¿Se admiten los proxies de terceros con AD FS?
Sí, los proxies de terceros se pueden colocar delante del proxy de aplicación Web, pero cualquier proxy de terceros debe admitir el [Protocolo MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx) para usarse en lugar del proxy de aplicación Web.

A continuación se muestra una lista de proveedores de terceros que somos conscientes de ello.  Siempre puede haber proveedores disponibles que no conocemos y actualizaremos la lista a medida que nos conozcamos.

- [Administrador de directivas de acceso de F5](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-third-party-integration-13-1-0/12.html#guid-1ee8fbb3-1b33-4982-8bb3-05ae6868d9ee)


### <a name="where-is-the-capacity-planning-sizing-spreadsheet-for-ad-fs-2016"></a>¿Dónde está la hoja de cálculo de tamaño de planeación de capacidad para AD FS 2016?
La versión AD FS 2016 de la hoja de cálculo puede descargarse [aquí](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
También se puede usar para AD FS en Windows Server 2012 R2.

### <a name="how-can-i-ensure-my-ad-fs-and-wap-servers-support-apples-atp-requirements"></a>¿Cómo puedo asegurarme de que los servidores WAP y AD FS admiten los requisitos de ATP de Apple?

Apple ha publicado un conjunto de requisitos denominado seguridad de transporte de aplicaciones (ATS) que pueden afectar a las llamadas de las aplicaciones de iOS que se autentican en AD FS.  Puede asegurarse de que los servidores de AD FS y WAP se ajustan asegurándose de que admiten los [requisitos para la conexión mediante ATS](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  
En particular, debe comprobar que los servidores WAP y AD FS admiten TLS 1,2 y que el conjunto de cifrado negociado de la conexión TLS admitirá la confidencialidad directa perfecta.

Puede habilitar y deshabilitar SSL 2,0 y las versiones 3,0 y TLS 1,0, 1,1 y 1,2 mediante los [protocolos de administración SSL en AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md).

Para asegurarse de que los servidores de AD FS y WAP negocian solo los conjuntos de cifrado TLS que admiten ATP, puede deshabilitar todos los conjuntos de cifrado que no estén en la [lista de conjuntos de cifrado compatibles con ATP](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  Para ello, use los [cmdlets de PowerShell de Windows TLS](https://technet.microsoft.com/itpro/powershell/windows/tls/index).

## <a name="developer"></a>Desarrollador

### <a name="when-generating-an-id_token-with-adfs-for-a-user-authenticated-against-ad-how-is-the-sub-claim-generated-in-the-id_token"></a>Al generar una id_token con ADFS para un usuario autenticado en AD, ¿cómo se genera la declaración "sub" en el id_token?
El valor de la demanda "sub" es el hash del identificador de cliente y el valor de la petición de delimitador.

### <a name="what-is-the-lifetime-of-the-refresh-tokenaccess-token-when-the-user-logs-in-via-a-remote-claims-provider-trust-over-ws-fedsaml-p"></a>¿Cuál es la duración del token de actualización y el token de acceso cuando el usuario inicia sesión a través de una confianza de proveedor de notificaciones remotas sobre WS-FED/SAML-P?
La duración del token de actualización será la duración del token que ADFS recibió de la confianza del proveedor de notificaciones remotas. La duración del token de acceso será la duración del token del usuario de confianza para el que se emite el token de acceso.

### <a name="i-need-to-return-profile-and-email-scopes-as-well-in-addition-to-the-openid-scope-can-i-obtain-additional-information-using-scopes-how-to-do-it-in-ad-fs"></a>También necesito devolver los ámbitos de perfil y correo electrónico, además del ámbito de OpenId. ¿Puedo obtener información adicional mediante el uso de ámbitos? ¿Cómo hacerlo en AD FS?
Puede usar id_token personalizadas para agregar información relevante en el id_token mismo. Para obtener más información, consulte el artículo [Personalización de notificaciones que se van a emitir en ID_token](../development/Custom-Id-Tokens-in-AD-FS.md).

### <a name="how-to-issue-json-blobs-inside-jwt-tokens"></a>¿Cómo se emiten blobs JSON dentro de tokens JWT?
Se agregó un ValueType especial ("<http://www.w3.org/2001/XMLSchema#json>") y un carácter de escape (\x22) en AD FS 2016. En el ejemplo siguiente se muestra la regla de emisión y también la salida final del token de acceso.

Ejemplo de regla de emisión:

    => issue(Type = "array_in_json", ValueType = "http://www.w3.org/2001/XMLSchema#json", Value = "{\x22Items\x22:[{\x22Name\x22:\x22Apple\x22,\x22Price\x22:12.3},{\x22Name\x22:\x22Grape\x22,\x22Price\x22:3.21}],\x22Date\x22:\x2221/11/2010\x22}");

Notificaciones emitidas en el token de acceso:

    "array_in_json":{"Items":[{"Name":"Apple","Price":12.3},{"Name":"Grape","Price":3.21}],"Date":"21/11/2010"}

### <a name="can-i-pass-resource-value-as-part-of-the-scope-value-like-how-requests-are-done-against-azure-ad"></a>¿Puedo pasar el valor del recurso como parte del valor del ámbito, como la forma en que se realizan las solicitudes en Azure AD?
Con AD FS en el servidor 2019, ahora puede pasar el valor de recurso incrustado en el parámetro de ámbito. Ahora, el parámetro de ámbito se puede organizar como una lista separada por espacios donde cada entrada es una estructura como recurso/ámbito. Por ejemplo,  
**< crear una solicitud de ejemplo válida >**

### <a name="does-ad-fs-support-pkce-extension"></a>¿AD FS admite la extensión PKCE?
AD FS en el servidor 2019 admite la clave de prueba para el intercambio de código (PKCE) para el flujo de concesión de código de autorización OAuth

### <a name="what-permitted-scopes-are-supported-by-ad-fs"></a>¿Qué ámbitos permitidos admite AD FS?
- AZA: si se usan [las extensiones de protocolo de OAuth 2,0 para los clientes de Broker](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapxbc/2f7d8875-0383-4058-956d-2fb216b44706) y si el parámetro de ámbito contiene el ámbito "AZA", el servidor emite un nuevo token de actualización principal y lo establece en el refresh_token campo de la respuesta, además de establecer el campo refresh_token_expires_in en la duración del nuevo token de actualización principal si se aplica uno.
- OpenID: permite que la aplicación solicite el uso del Protocolo de autorización OpenID Connect.
- logon_cert: el ámbito de logon_cert permite a una aplicación solicitar certificados de inicio de sesión, que se pueden usar para iniciar sesión de forma interactiva en usuarios autenticados. El servidor de AD FS omite el parámetro access_token de la respuesta y, en su lugar, proporciona una cadena de certificados CMS codificada en base64 o una respuesta de PKI completa de CMC. Puede encontrar más información [aquí](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapx/32ce8878-7d33-4c02-818b-6c9164cc731e). 
- user_impersonation: el ámbito de user_impersonation es necesario para solicitar correctamente un token de acceso en nombre de AD FS. Para obtener más información sobre cómo usar este ámbito, consulte [compilar una aplicación de varios niveles con on-behalf-of (OBO) mediante OAuth con AD FS 2016](../../ad-fs/development/ad-fs-on-behalf-of-authentication-in-windows-server.md).
- vpn_cert: el ámbito de vpn_cert permite a una aplicación solicitar certificados VPN, que se pueden usar para establecer conexiones VPN mediante la autenticación EAP-TLS. Esto ya no se admite.
- correo electrónico: permite que la aplicación solicite una solicitud de correo electrónico para el usuario que ha iniciado sesión. Esto ya no se admite. 
- Perfil: permite que la aplicación solicite notificaciones relacionadas con el perfil para el usuario de inicio de sesión. Esto ya no se admite. 


## <a name="operations"></a>Operaciones

### <a name="how-do-i-replace-the-ssl-certificate-for-ad-fs"></a>Cómo reemplazar el certificado SSL para AD FS?
El certificado SSL AD FS no es el mismo que el certificado de comunicaciones AD FS servicio que se encuentra en el complemento Administración de AD FS.  Para cambiar el certificado SSL AD FS, debe usar PowerShell. Siga las instrucciones del artículo siguiente:

[Administración de certificados SSL en AD FS y WAP 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

### <a name="how-can-i-enable-or-disable-tlsssl-settings-for-ad-fs"></a>¿Cómo puedo habilitar o deshabilitar la configuración de TLS/SSL para AD FS
Para deshabilitar o habilitar los protocolos SSL y los conjuntos de cifrado, use lo siguiente:

[Administrar los protocolos SSL en AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md)

### <a name="does-the-proxy-ssl-certificate-have-to-be-the-same-as-the-ad-fs-ssl-certificate"></a>¿El certificado SSL de proxy debe ser el mismo que el certificado SSL AD FS?  
Use la siguiente orientación con respecto al certificado SSL de proxy y el AD FS certificado SSL:


- Si el proxy se usa para AD FS solicitudes de proxy que usan la autenticación integrada de Windows, el certificado SSL de proxy debe ser el mismo (usar la misma clave) que el certificado SSL del servidor de Federación.
- Si está habilitada la propiedad AD FS "ExtendedProtectionTokenCheck" (la configuración predeterminada en AD FS), el certificado SSL del proxy debe ser el mismo (usar la misma clave) que el certificado SSL del servidor de Federación.
- De lo contrario, el certificado SSL de proxy puede tener una clave diferente de la AD FS certificado SSL, pero debe cumplir los mismos [requisitos](../overview/AD-FS-2016-Requirements.md)

### <a name="why-do-i-only-see-a-password-login-on-ad-fs-and-not-my-other-authentication-methods-that-i-have-configured"></a>¿Por qué solo veo un inicio de sesión de contraseña en AD FS y no en otros métodos de autenticación que he configurado? 
AD FS solo muestra un método de autenticación único en la pantalla de inicio de sesión cuando la aplicación requiere explícitamente un URI de autenticación específico que se asigna a un método de autenticación configurado y habilitado. Esto se transmite en el parámetro "wauth" para las solicitudes de WS-Federation y el parámetro "RequestedAuthnCtxRef" en una solicitud de protocolo SAML. Como resultado, solo se muestra el método de autenticación solicitado (por ejemplo, Inicio de sesión de contraseña).

Cuando AD FS se utiliza con Azure AD, es habitual que las aplicaciones envíen el parámetro prompt = login a Azure AD. De forma predeterminada Azure AD traduce esto a solicitar un inicio de sesión basado en contraseña nueva para AD FS. Esta es la razón más común para ver un inicio de sesión de contraseña en AD FS dentro de la red o no ver una opción para iniciar sesión con el certificado. Esto se puede solucionar fácilmente realizando un cambio en la configuración del dominio federado en Azure AD. 

Para obtener información sobre cómo configurarlo, consulte [servicios de Federación de Active Directory (AD FS) prompt = compatibilidad con parámetros de inicio de sesión](../operations/AD-FS-Prompt-Login.md).

### <a name="how-can-i-change-the-ad-fs-service-account"></a>¿Cómo se puede cambiar la cuenta de servicio de AD FS?
Para cambiar la cuenta de servicio de AD FS, siga las instrucciones que se indican en el módulo de PowerShell de la [cuenta de servicio](https://github.com/Microsoft/adfsToolbox/tree/master/serviceAccountModule)del cuadro de herramientas AD FS.

### <a name="how-can-i-configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>¿Cómo se pueden configurar los exploradores para usar la autenticación integrada de Windows (WIA) con AD FS?

Para obtener información sobre cómo configurar los exploradores [, consulte Configuración de exploradores para usar la autenticación integrada de Windows (WIA) con AD FS](../operations/Configure-AD-FS-Browser-WIA.md).

### <a name="can-i-turn-off-browserssoenabled"></a>¿Puedo desactivar BrowserSsoEnabled?
Si no tiene directivas de control de acceso basadas en el dispositivo en ADFS o en la inscripción de certificados de Windows Hello para empresas mediante ADFS; puede desactivar BrowserSsoEnabled. BrowserSsoEnabled permite que ADFS recopile un PRT (token de actualización principal) del cliente que contiene información del dispositivo. Sin esa autenticación de dispositivos, ADFS no funcionará en dispositivos Windows 10.

### <a name="how-long-are-ad-fs-tokens-valid"></a>¿Cuánto tiempo son válidos los tokens AD FS?

A menudo, esta pregunta significa "¿cuánto tiempo obtienen los usuarios el inicio de sesión único (SSO) sin tener que escribir nuevas credenciales y cómo puedo como control de administración?".  Este comportamiento y los valores de configuración que lo controlan se describen en el artículo [AD FS la configuración de inicio de sesión único](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-2016-single-sign-on-settings).

A continuación se enumeran las duraciones predeterminadas de las distintas cookies y tokens (así como los parámetros que rigen las duraciones):

**Dispositivos registrados**

- Cookies de PRT y SSO: 90 días como máximo, regido por PSSOLifeTimeMins. (El dispositivo proporcionado se usa al menos cada 14 días, que está controlado por DeviceUsageWindow)

- Token de actualización: se calcula en función de lo anterior para proporcionar un comportamiento coherente

- access_token: 1 hora de forma predeterminada, según el usuario de confianza

- id_token: igual que el token de acceso

**Dispositivos no registrados**

- Cookies de SSO: 8 horas de forma predeterminada, regido por SSOLifetimeMins.  Cuando la opción mantener la sesión iniciada (KMSI) está habilitada, el valor predeterminado es 24 horas y se puede configurar a través de KMSILifetimeMins.


- Token de actualización: 8 horas de forma predeterminada. 24 horas con KMSI habilitado

- access_token: 1 hora de forma predeterminada, según el usuario de confianza

- id_token: igual que el token de acceso

### <a name="does-ad-fs-support-http-strict-transport-security-hsts"></a>¿AD FS admite la seguridad de transporte estricta HTTP (HSTS)?  

La seguridad de transporte estricto HTTP (HSTS) es un mecanismo de directiva de seguridad Web que ayuda a mitigar los ataques de degradación del protocolo y la apropiación de cookies para los servicios que tienen puntos de conexión HTTP y HTTPS. Permite que los servidores web declaren que los exploradores Web (u otros agentes de usuario de cumplimiento) solo deberían interactuar con él mediante HTTPS y nunca a través del protocolo HTTP.

Todos los puntos de conexión de AD FS para el tráfico de autenticación Web se abren exclusivamente a través de HTTPS.  Como resultado, AD FS mitiga eficazmente las amenazas que proporciona el mecanismo de la Directiva de seguridad de transporte HTTP STRICT (por diseño, no hay ninguna degradación de HTTP, ya que no hay agentes de escucha en HTTP). Además, AD FS impide que las cookies se envíen a otro servidor con extremos del protocolo HTTP marcando todas las cookies con la marca segura.

Por lo tanto, no es necesario implementar HSTS en un servidor de AD FS porque nunca se puede degradar.  Por motivos de cumplimiento, AD FS servidores cumplen estos requisitos porque nunca pueden usar HTTP y todas las cookies se marcan como seguras.

Además, AD FS 2016 (con las revisiones más recientes) y AD FS compatibilidad con 2019 que emite el encabezado HSTS. Para configurarlo, consulte [Personalización de encabezados de respuesta de seguridad http con AD FS](../operations/customize-http-security-headers-ad-fs.md)

### <a name="x-ms-forwarded-client-ip-does-not-contain-the-ip-of-the-client-but-contains-ip-of-the-firewall-in-front-of-the-proxy-where-can-i-get-the-right-ip-of-the-client"></a>X-MS-forwarded-Client-IP no contiene la dirección IP del cliente, pero contiene la dirección IP del firewall delante del proxy. ¿Dónde puedo obtener la dirección IP correcta del cliente?
No se recomienda la terminación SSL antes de WAP. En caso de que se termine la terminación SSL delante de la WAP, X-MS-forwarded-Client-IP contendrá la dirección IP del dispositivo de red delante de WAP. A continuación se muestra una breve descripción de las distintas notificaciones relacionadas con IP admitidas por AD FS:
 - x-MS-Client-IP: IP de red del dispositivo que se ha conectado al STS.  En el caso de una solicitud de extranet, este siempre contiene la dirección IP de la WAP.
 - x-MS-forwarded-Client-IP: notificaciones multivalor que contendrán los valores reenviados a ADFS por Exchange Online y la dirección IP del dispositivo que se conectó a WAP.
 - Userip: para las solicitudes de extranet, esta demanda contendrá el valor de x-MS-forwarded-Client-IP.  En el caso de las solicitudes de la intranet, esta demanda contendrá el mismo valor que x-MS-Client-IP.

 Además, en AD FS 2016 (con las revisiones más recientes) y las versiones posteriores también admiten la captura del encabezado x-forwarded-for. Cualquier equilibrador de carga o dispositivo de red que no avance en el nivel 3 (IP se conserva) debe agregar la dirección IP de cliente entrante al encabezado x-forwarded-for estándar del sector. 

### <a name="i-am-trying-to-get-additional-claims-on-the-user-info-endpoint-but-its-only-returning-subject-how-can-i-get-additional-claims"></a>Estoy intentando obtener notificaciones adicionales en el punto de conexión de información de usuario, pero su único asunto devuelto. ¿Cómo puedo obtener notificaciones adicionales?
El punto de conexión de la UserInfo AD FS siempre devuelve la demanda de asunto como se especifica en los estándares de OpenID. AD FS no proporciona notificaciones adicionales solicitadas a través del punto de conexión de la UserInfo. Si necesita notificaciones adicionales en el token de identificador, consulte [tokens de identificador personalizado en AD FS](../development/custom-id-tokens-in-ad-fs.md).

### <a name="why-do-i-see-a-lot-of-1021-errors-on-my-ad-fs-servers"></a>¿Por qué veo muchos errores 1021 en los servidores AD FS?
Este evento se registra normalmente para un acceso a recursos no válido en AD FS para el recurso 00000003-0000-0000-C000-000000000000. Este error se debe a un comportamiento erróneo del cliente en el que intenta obtener un token de acceso para el servicio Azure AD Graph. Puesto que el recurso no está presente en AD FS, esto da como resultado el ID. de evento 1021 en los servidores de AD FS. Es seguro omitir cualquier advertencia o error para el recurso 00000003-0000-0000-C000-000000000000 en AD FS.

### <a name="why-am-i-seeing-a-warning-for-failure-to-add-the-ad-fs-service-account-to-the-enterprise-key-admins-group"></a>¿Por qué veo una advertencia de error al agregar la cuenta de servicio de AD FS al grupo administradores de claves de empresa?
Este grupo solo se crea cuando existe un controlador de dominio de Windows 2016 con el rol PDC de FSMO en el dominio. Para resolver el error, puede crear el grupo manualmente y seguir el siguiente para conceder el permiso necesario después de agregar la cuenta de servicio como miembro del grupo.
1.  Abre **Usuarios y equipos de Active Directory**.
2.  **Haga clic con el botón secundario en** el nombre de dominio en el panel de navegación y **haga clic en** propiedades.
3.  **Haga clic en** Seguridad (si falta la pestaña seguridad, Active características avanzadas en el menú Ver).
4.  **Haga clic en** Financieros. **Haga clic en** Agréguela. **Haga clic en** Seleccione una entidad de seguridad.
5.  Aparece el cuadro de diálogo Seleccionar usuario, equipo, cuenta de servicio o grupo.  En el cuadro de texto escriba el nombre de objeto que desea seleccionar, escriba grupo de administradores de claves.  Haga clic en Aceptar.
6.  En el cuadro de lista se aplica a, seleccione **objetos de usuario descendiente**.
7.  Con la barra de desplazamiento, desplácese hasta la parte inferior de la página y **haga clic en** borrar todo.
8.  En la sección **propiedades** , seleccione **Leer MSDS-KeyCredentialLink** y **Escriba MSDS-KeyCredentialLink**.

### <a name="why-does-modern-authentication-from-android-devices-fail-if-the-server-does-not-send-all-the-intermediate-certificates-in-the-chain-with-the-ssl-cert"></a>¿Por qué se produce un error en la autenticación moderna de dispositivos Android si el servidor no envía todos los certificados intermedios de la cadena con el certificado SSL?

Los usuarios federados pueden experimentar la autenticación en Azure AD para las aplicaciones que usan la biblioteca de ADAL de Android. La aplicación obtendrá un **AuthenticationException** cuando intente Mostrar la página de inicio de sesión. En Chrome, es posible que la página de inicio de sesión de AD FS se invoque como no segura.

Android: en todas las versiones y todos los dispositivos: no admite la descarga de certificados adicionales desde el campo **authorityInformationAccess** del certificado. Esto también se aplica al explorador Chrome. Cualquier certificado de autenticación de servidor que no tenga certificados intermedios producirá este error si la cadena de certificados completa no se pasa desde AD FS.

Una solución adecuada para este problema es configurar los servidores AD FS y WAP para enviar los certificados intermedios necesarios junto con el certificado SSL.

Al exportar el certificado SSL, de un equipo, que se va a importar en el almacén personal del equipo, del AD FS y de los servidores WAP, asegúrese de exportar la clave privada y seleccione **intercambio de información personal: PKCS #12**.

Es importante que se active la casilla para **incluir todos los certificados en la ruta de acceso del certificado, si es posible** , así como **exportar todas las propiedades extendidas**.  

Ejecute certlm. msc en los servidores de Windows e importe el *. PFX en el almacén de certificados personal del equipo. Esto hará que el servidor pase toda la cadena de certificados a la biblioteca ADAL.

>[!NOTE]
> El almacén de certificados de los equilibradores de carga de red también debe actualizarse para incluir toda la cadena de certificados si está presente

### <a name="does-ad-fs-support-head-requests"></a>¿AD FS admite las solicitudes HEAD?
AD FS no admite solicitudes HEAD.  Las aplicaciones no deben usar solicitudes HEAD en puntos de conexión de AD FS.  Esto puede producir respuestas de error HTTP inesperadas o diferidas.  Además, es posible que vea eventos de error inesperados en el registro de eventos de AD FS.

### <a name="why-am-i-not-seeing-a-refresh-token-when-i-am-logging-in-with-a-remote-idp"></a>¿Por qué no veo un token de actualización cuando inicio sesión con un IdP remoto?
No se emite un token de actualización si el token emitido por IdP tiene un validty de menos de 1 hora. Para asegurarse de que se emite un token de actualización, aumente la validez del token emitido por el IdP a más de 1 hora.

### <a name="do-we-have-any-way-to-change-rp-token-encryption-algorithm"></a>¿Hay alguna manera de cambiar el algoritmo de cifrado de tokens RP?
De forma predeterminada, el cifrado de tokens RP se establece en AES256 y no se puede cambiar a ningún otro valor.

### <a name="on-a-mixed-mode-farm-i-get-error-when-trying-to-set-the-new-ssl-certificate-using-set-adfssslcertificate--thumbprint-how-can-i-update-the-ssl-certificate-in-a-mixed-mode-ad-fs-farm"></a>En una granja de servidores de modo mixto, obtengo un error al intentar establecer el nuevo certificado SSL mediante Set-AdfsSslCertificate-Thumbprint. ¿Cómo puedo actualizar el certificado SSL en un modo mixto AD FS granja de servidores?
Las granjas de AD FS de modo mixto están pensadas para ser un estado de transición. Se recomienda durante la planeación para sustituir el certificado SSL antes del proceso de actualización o completar el proceso y aumentar el nivel de comportamiento de la granja antes de actualizar el certificado SSL. En caso de que no se haya hecho esto, las siguientes instrucciones proporcionan la capacidad de actualizar el certificado SSL. 

En los servidores WAP, puede seguir usando Set-WebApplicationProxySslCertificate. En los servidores ADFS, debe utilizar netsh. Siga los pasos que se indican a continuación:

1. Seleccionar subconjunto de servidores de ADFS 2016 para su mantenimiento (por ejemplo, quitar del equilibrador de carga)
2. En los servidores seleccionados en #1, importe el nuevo certificado a través de MMC.
3. Eliminar los certificados existentes

    a. netsh http Delete sslcert hostnameport = FS. contoso. com: 443 b. netsh http Delete sslcert hostnameport = localhost: 443 c. netsh http Delete sslcert hostnameport = FS. contoso. com: 49443

4.  Agregar el nuevo certificado

    a. netsh http Add sslcert hostnameport = FS. contoso. com: 443 certhash = CERTTHUMBPRINT AppID = {5d89a20c-BEAB-4389-9447-324788eb944a} certstorename = MY SslCtlStoreName = AdfsTrustedDevices

    b. netsh http Add sslcert hostnameport = localhost: 443 certhash = CERTTHUMBPRINT AppID = {5d89a20c-BEAB-4389-9447-324788eb944a} certstorename = MY SslCtlStoreName = AdfsTrustedDevices

    c. netsh http Add sslcert hostnameport = FS. contoso. com: 49443 certhash = CERTTHUMBPRINT AppID = {5d89a20c-BEAB-4389-9447-324788eb944a} certstorename = MY SslCtlStoreName = AdfsTrustedDevices

5. Reiniciar el servicio ADFS en el servidor seleccionado
6. Quitar subconjunto de servidores WAP para mantenimiento
7. En los servidores WAP seleccionados, importe el nuevo certificado a través de MMC.
8. Establecer el nuevo certificado en WAP mediante el cmdlet

    a. Set-WebApplicationProxySslCertificate-Thumbprint "CERTTHUMBPRINT"

9. Reiniciar el servicio en los servidores WAP seleccionados
10. Vuelva a colocar los servidores WAP y AD FS seleccionados en el entorno de producción.

Realice la actualización en el resto de AD FS y servidores WAP de manera similar.

### <a name="is-adfs-supported-when-web-application-proxy-wap-servers-are-behind-azure-web-application-firewallwaf"></a>¿Se admite ADFS cuando los servidores del proxy de aplicación web (WAP) están detrás del firewall de aplicaciones web (WAF) de Azure?
ADFS y los servidores de aplicaciones web admiten cualquier firewall que no realice la terminación SSL en el extremo. Además, los servidores ADFS/WAP tienen mecanismos integrados para evitar ataques web comunes, como el scripting entre sitios, el proxy de ADFS y cumplir todos los requisitos definidos por el [Protocolo MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx).

### <a name="i-am-seeing-an-event-441-a-token-with-a-bad-token-binding-key-was-found-what-should-i-do-to-resolve-this"></a>Veo un "evento 441: se encontró un token con una clave de enlace de tokens no válida". ¿Qué debo hacer para solucionarlo?
En AD FS 2016, el enlace de tokens se habilita automáticamente y provoca varios problemas conocidos con los escenarios de proxy y de Federación que producen este error. Para resolverlo, ejecute el siguiente comando de PowerShell y quite la compatibilidad con el enlace de tokens.

`Set-AdfsProperties -IgnoreTokenBinding $true`

### <a name="i-have-upgraded-my-farm-from-ad-fs-in-windows-server-2016-to-ad-fs-in-windows-server-2019-the-farm-behavior-level-for-the-ad-fs-farm-has-been-successfully-raised-to-2019-but-the-web-application-proxy-configuration-is-still-displayed-as-windows-server-2016"></a>He actualizado mi granja de servidores de AD FS en Windows Server 2016 a AD FS en Windows Server 2019. El nivel de comportamiento de la granja de servidores de AD FS se ha generado correctamente en 2019 pero la configuración del proxy de aplicación web todavía se muestra como Windows Server 2016?
Después de una actualización a Windows Server 2019, la versión de configuración del proxy de aplicación web se seguirá mostrando como Windows Server 2016. El proxy de aplicación web no tiene nuevas características específicas de la versión para Windows Server 2019 y, si el nivel de comportamiento de la granja se ha generado correctamente en AD FS, el proxy de aplicación web seguirá mostrando Windows Server 2016 por diseño.

### <a name="can-i-estimate-the-size-of-the-adfsartifactstore-before-enabling-esl"></a>¿Puedo calcular el tamaño de ADFSArtifactStore antes de habilitar ESL?
Con ESL habilitado, AD FS realiza un seguimiento de la actividad de la cuenta y de las ubicaciones conocidas de los usuarios en la base de datos ADFSArtifactStore. Esta base de datos se escala en función del número de usuarios y de las ubicaciones conocidas a las que se realiza el seguimiento. Al planear la habilitación de ESL, puede calcular el tamaño de la base de datos de ADFSArtifactStore para que crezca a una velocidad de hasta 1 GB por 100.000 usuarios. Si la granja de AD FS usa Windows Internal Database (WID), la ubicación predeterminada de los archivos de base de datos es C:\Windows\WID\Data. Para evitar que se llene esta unidad, asegúrese de tener un mínimo de 5 GB de almacenamiento libre antes de habilitar ESL. Además de almacenamiento en disco, planee el aumento de la memoria de proceso total después de habilitar ESL hasta un 1 GB de RAM adicional para los rellenados de usuarios de 500.000 o menos.
