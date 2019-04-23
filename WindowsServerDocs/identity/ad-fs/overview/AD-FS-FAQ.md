---
ms.assetid: acc9101b-841c-4540-8b3c-62a53869ef7a
title: Preguntas frecuentes de AD FS de 2016
description: Preguntas más frecuentes para AD FS 2016
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 12/07/2018
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d8014cd72e66642ea9200afd6cd4266cbccba30c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826816"
---
# <a name="ad-fs-frequently-asked-questions-faq"></a>Preguntas más frecuentes (P+F) de AD FS

>Se aplica a: Windows Server 2016

La siguiente documentación es una página principal a las preguntas más frecuentes con respecto a los servicios de federación de Active Directory.  El documento se ha dividido en grupos según el tipo de pregunta.

## <a name="deployment"></a>Implementación 

### <a name="how-can-i-upgrademigrate-from-previous-versions-of-ad-fs"></a>¿Cómo puedo actualizar o migrar desde versiones anteriores de AD FS
Puede actualizar AD FS mediante uno de los siguientes:


- Windows Server 2012 R2 AD FS para Windows Server 2016 AD FS
    - [Actualización a AD FS en Windows Server 2016 mediante una base de datos WID](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)
    - [Actualización a AD FS en Windows Server 2016 mediante una base de datos SQL](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL.md)
- Windows Server 2012 AD FS para Windows Server 2012 R2 AD FS
    - [Migrar a AD FS en Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- AD FS 2.0 para Windows Server 2012, AD FS
    - [Migrar a AD FS en Windows Server 2012](https://technet.microsoft.com/library/jj647765.aspx)
- AD FS 1.x a AD FS 2.0 
    - [Actualización de AD FS 1.x a AD FS 2.0](https://technet.microsoft.com/library/ff678035.aspx)

Si necesita actualizar desde AD FS 2.0 o 2.1 (Windows Server 2008 R2 o Windows Server 2012), debe usar las secuencias de comandos en el cuadro (ubicados en C:\Windows\ADFS).

### <a name="why-does-ad-fs-installation-require-a-reboot-of-the-server"></a>¿Por qué requiere un reinicio del servidor de instalación de AD FS?

Se agregó compatibilidad con HTTP/2 en Windows Server 2016, pero no se puede usar HTTP/2 para la autenticación de certificado de cliente.  Dado que muchos escenarios de AD FS hacen uso de la autenticación de certificado de cliente y un número significativo de los clientes no admiten no volver a intentar solicitudes con HTTP/1.1, AD FS granja configuración volverán a configurar configuración de HTTP del servidor local para HTTP/1.1.  Esto requiere un reinicio del servidor.  

### <a name="is-using-windows-2016-wap-servers-to-publish-the-ad-fs-farm-to-the-internet-without-upgrading-the-back-end-ad-fs-farm-supported"></a>¿Está usando los servidores de Windows 2016 WAP para publicar la granja de servidores de AD FS a internet sin actualizar la granja de AD FS de back-end compatibles?
Sí, se admite esta configuración, sin embargo, no hay características nuevas de AD FS 2016 podría ser compatible con esta configuración.  Esta configuración está pensada para ser temporal durante la fase de migración de AD FS 2012 R2 AD FS 2016 y no se debe implementar durante largos períodos de tiempo.

### <a name="is-it-possible-to-deploy-ad-fs-for-office-365-without-publishing-a-proxy-to-office-365"></a>¿Es posible implementar AD FS para Office 365 sin publicar a un servidor proxy en Office 365?
Sí, esto se admite. Sin embargo, como un efecto secundario

1. Deberá administrar manualmente la actualización de certificados de firmas de tokens porque Azure AD no podrán tener acceso a los metadatos de federación. Para obtener más información sobre cómo actualizar manualmente el certificado de firma de tokens leer [renovar certificados de federación para Office 365 y Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs)
2. No podrá aprovechar la autenticación heredada flujos (por ejemplo, flujo de autenticación de ExO proxy)

### <a name="what-are-load-balancing-requirements-for-ad-fs-and-wap-servers"></a>¿Cuáles son los requisitos de equilibrio de carga para los servidores de AD FS y WAP? 

AD FS es un sistema sin estado. Por lo tanto, el equilibrio de carga es bastante sencillo para los inicios de sesión. Los siguientes son recomendaciones importantes para los sistemas de equilibrio de carga. 


- Los equilibradores de carga no deben configurarse con afinidad de IP. Esto puede hacer que una carga excesiva en un subconjunto de los servidores en ciertos escenarios de Exchange Online.
- Los equilibradores de carga no deben terminar las conexiones HTTPS y vuelva a iniciar una nueva conexión con el servidor de ADFS. 
- Los equilibradores de carga deben asegurarse de que se debe traducir la dirección IP como dirección IP de origen en el paquete HTTP al que se envían a ADFS. En caso de que un equilibrador de carga no puede enviar la dirección IP de origen en el paquete HTTP, el equilibrador de carga debe agregar (o anexar en el caso de existentes) la dirección IP para el encabezado x-forwarded-for. Esto es necesario para la administración correcta de determinadas IP relacionadas con las características (IP prohibida, Smart bloqueos de Extranet,...) y podría dar lugar a reducir la seguridad si se configuró incorrectamente. 
- Los equilibradores de carga deben admitir SNI. En el evento no es así, asegúrese de que AD FS está configurado para crear enlaces HTTPS para administrar los clientes SNI que no son compatibles con.
- Los equilibradores de carga deben usar el punto de conexión de sondeo de estado HTTP de AD FS para detectar si los servidores de AD FS o WAP estén en funcionamiento y excluyan si no es aceptar devuelve una respuesta 200. 

### <a name="what-multi-forest-configurations-are-supported-by-ad-fs"></a>¿Qué configuraciones de bosques múltiples son compatibles con AD FS? 

AD FS admite la configuración de varios bosques múltiples y se basa en la red de confianza de AD DS subyacente para autenticar a los usuarios a través de varios dominios de confianza. Se recomienda encarecidamente confianzas de bosques de 2 vías, tal como se trata de una configuración más sencilla para asegurarse de que el subsistema de confianza funciona correctamente sin problemas. Además, 

- En el caso de una confianza de bosque unidireccional como un bosque de la red Perimetral que contiene las identidades de socios, se recomienda implementar ADFS en el bosque corp y tratar el bosque de la red Perimetral como otra relación de confianza del proveedor de notificaciones local conectado a través de LDAP. En caso de que no se puede seguir esta opción, deberá ejecutar ADFS en el bosque de "confianza" con una cuenta de servicio en la "bosque de confianza" que tiene acceso completo a los usuarios en el bosque de "confianza".
- Aunque las confianzas de nivel de dominio se admiten y pueden funcionar, se recomienda encarecidamente mover a un modelo de nivel de confianza de bosque. Además, deberá asegurarse de que deben trabajar con precisión el enrutamiento de UPN y resolución de nombres NETBIOS. 



## <a name="design"></a>Diseño

### <a name="what-third-party-multi-factor-authentication-providers-are-available-for-ad-fs"></a>¿Qué proveedores de autenticación multifactor de terceros están disponibles para AD FS? 
A continuación es una lista de proveedores de terceros, de que somos conscientes.  Siempre puede haber proveedores disponibles que no sabemos acerca y se actualizará la lista a medida que Aprendemos sobre ellos.

- [Identidad de Gemalto y servicios de seguridad](http://www.gemalto.com/identity)
- [servicio de autenticación empresarial inWebo](http://www.inwebo.com/)
- [Conector de API de MFA de personas de inicio de sesión](https://www.loginpeople.com)
- [Agente de autenticación de SecurID RSA para servicios de federación de Microsoft Active Directory](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)
- [Agente de servicio (SAS) de autenticación de SafeNet para AD FS](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)
- [Servicio de autenticación de Mobile Swisscom Id.](http://swisscom.ch/mid)
- [Validación de Symantec y el servicio de protección de ID (VIP)](http://www.symantec.com/vip-authentication-service) 

### <a name="are-third-party-proxies-supported-with-ad-fs"></a>¿Se admiten servidores proxy de terceros con AD FS?
Sí, los servidores proxy de terceros pueden colocarse delante el Proxy de aplicación Web, pero cualquier proxy de terceros debe admitir la [protocolo MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx) para usarse en lugar de Web Application Proxy.

### <a name="what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip"></a>¿Qué servidores proxy de terceros están disponibles para AD FS que admite MS-ADFSPIP?

A continuación es una lista de proveedores de terceros, de que somos conscientes.  Siempre puede haber proveedores disponibles que no sabemos acerca y se actualizará la lista a medida que Aprendemos sobre ellos.

- [F5 Administrador de directivas de acceso](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-third-party-integration-13-1-0/12.html#guid-1ee8fbb3-1b33-4982-8bb3-05ae6868d9ee)

### <a name="where-is-the-capacity-planning-sizing-spreadsheet-for-ad-fs-2016"></a>¿Dónde está el planeamiento de capacidad hoja de cálculo de tamaño para AD FS 2016?
Se puede descargar la versión de AD FS 2016 de la hoja de cálculo [aquí](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
También puede utilizarse para AD FS en Windows Server 2012 R2.

### <a name="how-can-i-ensure-my-ad-fs-and-wap-servers-support-apples-atp-requirements"></a>¿Cómo me aseguro Mi asistencia de servidores de AD FS y WAP los requisitos de ATP de Apple?

Apple ha publicado un conjunto de requisitos que se denomina App Transport Security (ATS) que pueden afectar a las llamadas desde aplicaciones de iOS que se autentican en AD FS.  Puede asegurarse de que su instancia de AD FS y servidores WAP cumplan porque nos aseguramos de que admiten la [los requisitos para conectarse utilizando ATS](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  
En concreto, debe comprobar que los servidores de AD FS y WAP admiten TLS 1.2 y que los conjuntos de cifrado negociado de la conexión TLS admitirá la confidencialidad directa perfecta.

Puede habilitar y deshabilitar las versiones 1.0, 1.1 y 1.2 de SSL 2.0 y 3.0 y TLS mediante [administrar protocolos SSL en AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md).

Para garantizar su AD FS y WAP servidores negocian solo conjuntos de cifrado TLS que admiten ATP, puede deshabilitar todos los conjuntos de cifrado que no están en el [lista de conjuntos de cifrado compatibles de ATP](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  Para ello, use el [cmdlets de Windows PowerShell de TLS](https://technet.microsoft.com/itpro/powershell/windows/tls/index). 

## <a name="developer"></a>Desarrollador

### <a name="when-generating-an-idtoken-with-adfs-for-a-user-authenticated-against-ad-how-is-the-sub-claim-generated-in-the-idtoken"></a>¿Cuando se genera un id_token con ADFS para un usuario autenticado en AD, cómo se genera la notificación "sub" en el id_token?
El valor de notificación de "sub" es el hash del Id. de cliente + el valor de notificación delimitador.

### <a name="what-is-the-lifetime-of-the-refresh-tokenaccess-token-when-the-user-logs-in-via-a-remote-claims-provider-trust-over-ws-fedsaml-p"></a>¿Qué es la duración del token de acceso y el token de actualización cuando el usuario inicia sesión a través de una confianza de proveedor de notificaciones remotas a través de WS-Fed/SAML-P? 
La duración del token de actualización será la duración del token que obtuvo de ADFS de confianza de proveedor de notificaciones remotas. La duración del token de acceso será la duración del token del usuario de confianza para el que se emite el token de acceso.

### <a name="i-need-to-return-profile-and-email-scopes-as-well-in-addition-to-the-openid-scope-can-i-obtain-additional-information-using-scopes-how-to-do-it-in-ad-fs"></a>Es necesario devolver los ámbitos del perfil y correo electrónico también además el ámbito OpenId. ¿Puedo obtener información adicional con ámbitos? ¿Para saber cómo hacerlo en AD FS?
Puede usar el id_token personalizada para agregar información relevante en el valor de id_token. Para obtener más información, vea el artículo [personalizar las notificaciones que se emitan en id_token](../development/Custom-Id-Tokens-in-AD-FS.md).

### <a name="how-to-issue-json-blobs-inside-jwt-tokens"></a>¿Cómo emitir los blobs json dentro de los tokens JWT?
Un tipo de valor especial ("http://www.w3.org/2001/XMLSchema#json") y escape character(\x22) Esto se agregó en AD FS 2016. Inicie el ejemplo siguiente para la regla de emisión y también la salida final del token de acceso.

Regla de emisión de ejemplo:

    => issue(Type = "array_in_json", ValueType = "http://www.w3.org/2001/XMLSchema#json", Value = "{\x22Items\x22:[{\x22Name\x22:\x22Apple\x22,\x22Price\x22:12.3},{\x22Name\x22:\x22Grape\x22,\x22Price\x22:3.21}],\x22Date\x22:\x2221/11/2010\x22}");

Notificaciones emitidas en el token de acceso:

    "array_in_json":{"Items":[{"Name":"Apple","Price":12.3},{"Name":"Grape","Price":3.21}],"Date":"21/11/2010"}

### <a name="can-i-pass-resource-value-as-part-of-the-scope-value-like-how-requests-are-done-against-azure-ad"></a>¿Puedo pasar el valor de recurso como parte del valor de ámbito, similar a cómo se realizan solicitudes en Azure AD? 
Con AD FS en el servidor de 2019, ahora puede pasar el valor del recurso incrustado en el parámetro de ámbito. El parámetro de ámbito puede organizarse ahora como una lista separada por espacios, donde cada entrada es la estructura como recurso y el ámbito. Por ejemplo  
**< crear una solicitud de ejemplo válido >**

### <a name="does-ad-fs-support-pkce-extension"></a>¿Admite la extensión PKCE AD FS?
AD FS en el servidor de 2019 admite la clave de prueba para código Exchange (PKCE) para el flujo de concesión de código de autorización de OAuth 

## <a name="operations"></a>Operaciones

### <a name="how-do-i-replace-the-ssl-certificate-for-ad-fs"></a>¿Cómo se puede reemplazar el certificado SSL para AD FS? 
El certificado SSL de AD FS no es el mismo que el certificado de comunicaciones de servicio de AD FS se encuentra en el complemento de administración de AD FS.  Para cambiar el certificado SSL de AD FS, necesitará usar PowerShell. Siga las instrucciones en el artículo siguiente:

[Administración de certificados SSL en AD FS y WAP 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

### <a name="how-can-i-enable-or-disable-tlsssl-settings-for-ad-fs"></a>¿Cómo puedo habilitar o deshabilitar la configuración de TLS/SSL para AD FS
Para deshabilitar o habilitar los protocolos SSL y conjuntos de cifrado, use lo siguiente:

[Administrar los protocolos SSL en AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md)

### <a name="does-the-proxy-ssl-certificate-have-to-be-the-same-as-the-ad-fs-ssl-certificate"></a>¿El certificado SSL de proxy debe ser el mismo que el certificado SSL de AD FS?  
Use las instrucciones siguientes relacionadas con el certificado SSL de proxy y el certificado SSL de AD FS:


- Si se usa el proxy para las solicitudes de proxy AD FS que usan la autenticación integrada de Windows, el certificado SSL de proxy deben ser el mismo (usar la misma clave) que el certificado SSL de servidor de federación
- Si la propiedad de AD FS que es "ExtendedProtectionTokenCheck" habilitada (el valor predeterminado en AD FS), el certificado SSL de proxy debe ser el mismo (usar la misma clave) que el certificado SSL de servidor de federación
- En caso contrario, el certificado SSL de proxy puede tener una clave diferente del certificado SSL de AD FS, pero debe cumplir el mismo [requisitos](../overview/AD-FS-2016-Requirements.md)

### <a name="how-can-i-configure-promptlogin-behavior-for-ad-fs"></a>¿Cómo puedo configurar mensaje = el comportamiento de inicio de sesión de AD FS?
Para obtener más información sobre cómo configurar el símbolo del sistema = inicio de sesión, vea [el símbolo del sistema de servicios de federación de Active Directory = compatibilidad con parámetros de inicio de sesión](../operations/AD-FS-Prompt-Login.md).

### <a name="how-can-i-configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>¿Cómo se puede configurar los exploradores la usen la autenticación integrada de Windows (WIA) con AD FS?

Para obtener información sobre cómo configurar los exploradores vea [configurar exploradores para utilizar autenticación integrada de Windows (WIA) con AD FS](../operations/Configure-AD-FS-Browser-WIA.md).

### <a name="can-i-trun-off-browserssoenabled"></a>¿Se puede truncar desactivar BrowserSsoEnabled?
Si no tiene directivas de control de acceso basadas en dispositivo en AD FS o Windows Hello para la inscripción de certificados de empresa con ADFS; puede desactivar BrowserSsoEnabled. BrowserSsoEnabled permite ADFS recopilar un PRT (Token de actualización principal) de cliente que contiene información del dispositivo. Sin ese dispositivo de autenticación de ADFS no funcionará en dispositivos Windows 10.

### <a name="how-long-are-ad-fs-tokens-valid"></a>¿Cuánto tiempo son válidos los tokens de AD FS?

A menudo, esta pregunta significa "cuánto obtienen los usuarios un inicio de sesión único (SSO) sin tener que escribir las nuevas credenciales, ¿cómo puedo como un administrador controlar que?"  Este comportamiento y las opciones de configuración que controlan, se describen en el artículo [único inicio de sesión en configuración de AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-2016-single-sign-on-settings).

Las vigencias predeterminadas de los diversos cookies y los tokens se enumeran a continuación (así como los parámetros que controlan las duraciones):

**Dispositivos registrados**

- Cookies PRT y el inicio de sesión único: 90 días como máximo, se rige por PSSOLifeTimeMins. (Dispositivo proporcionado se usa al menos cada 14 días, que está controlado por DeviceUsageWindow)

- Token de actualización: calcula en función de lo anterior, para proporcionar un comportamiento coherente

- access_token: 1 hora de forma predeterminada, en función de usuario de confianza

- ID_token: igual que el token de acceso

**Dispositivos no registrados**

- Cookies de SSO: 8 horas de forma predeterminada, se rige por SSOLifetimeMins.  Cuando tenga iniciada (KMSI) está habilitada, el valor predeterminado es 24 horas y se pueden configurar a través de KMSILifetimeMins.


- Token de actualización: 8 horas de forma predeterminada. 24 horas con KMSI habilitado

- access_token: 1 hora de forma predeterminada, en función de usuario de confianza

- ID_token: igual que el token de acceso

### <a name="does-ad-fs-support-http-strict-transport-security-hsts"></a>¿Se admite seguridad de transporte estricto (HSTS) de HTTP de AD FS?  

Seguridad de transporte estricto de HTTP (HSTS) es un mecanismo de directiva de seguridad de web que ayuda a mitigar los ataques de degradación de protocolo y al secuestro de cookie para los servicios que tienen puntos de conexión HTTP y HTTPS. Permite a los servidores web declarar que los exploradores web (u otros agentes de usuario de cumplimiento) solo deben interactuar con él mediante HTTPS y nunca a través del protocolo HTTP.

Todos los extremos de AD FS para el tráfico de autenticación web se abren exclusivamente a través de HTTPS.  Como resultado, AD FS mitiga eficazmente las amenazas que proporciona el mecanismo de directiva de seguridad de transporte estricto HTTP (por diseño, no hay ninguna degradación a HTTP dado que no hay ningún agente de escucha en HTTP). Además, AD FS impide que las cookies que se envían a otro servidor con los extremos del protocolo HTTP al marcar todas las cookies con la marca segura.

Por lo tanto, implementar HSTS en un servidor de AD FS no es necesario porque nunca se puede degradar.  Por motivos de cumplimiento, servidores de AD FS cumplen estos requisitos porque nunca pueden usar HTTP y todas las cookies se marquen como seguras.

### <a name="x-ms-forwarded-client-ip-does-not-contain-the-ip-of-the-client-but-contains-ip-of-the-firewall-in-front-of-the-proxy-where-can-i-get-the-right-ip-of-the-client"></a>X-ms-reenviados-ip del cliente no contiene la dirección IP del cliente, pero contiene IP del firewall delante del proxy. ¿Dónde puedo obtener la dirección IP correcta del cliente?
No se recomienda hacer la terminación SSL antes de WAP. En caso de que se realiza la terminación SSL delante de lo WAP, el X-ms-reenviados-client-ip contendrá la dirección IP del dispositivo de red delante de WAP. A continuación encontrará una breve descripción de varias direcciones IP relacionado con las notificaciones que son compatibles con AD FS:
 - x-ms-client-ip: Dirección IP del dispositivo que conecta al STS de red.  En el caso de una solicitud de extranet siempre contiene la dirección IP de lo WAP.
 - x-ms-reenviados-client-ip: Notificación multivalor que contiene los valores que se reenvían a ADFS por Exchange Online además de la dirección IP del dispositivo que ha conectado a la WAP.
 - Userip: Para las solicitudes de extranet esta notificación contendrá el valor de x-ms-reenviados-ip del cliente.  Para las solicitudes de la intranet, esta notificación contendrá el mismo valor que x-ms-client-ip.

### <a name="i-am-trying-to-get-additional-claims-on-the-user-info-endpoint-but-its-only-returning-subject-how-can-i-get-additional-claims"></a>Estoy intentando obtener notificaciones adicionales en el punto de conexión de la información de usuario, pero su devolución único asunto. ¿Cómo se puede obtener notificaciones adicionales?
El punto de conexión ADFS userinfo siempre devuelve la notificación del firmante, como se especifica en los estándares de OpenID. AD FS no proporcionan notificaciones adicionales que se solicita a través del extremo de la información de usuario. Si necesita notificaciones adicionales en el token de identificador, consulte [Tokens de identificador personalizado en AD FS](../development/custom-id-tokens-in-ad-fs.md).

### <a name="why-do-i-see-a-lot-of-1021-errors-on-my-ad-fs-servers"></a>¿Por qué veo muchos errores 1021 en Mis servidores de AD FS?
Este evento se registra normalmente para un acceso a los recursos no válido en AD FS para el recurso 00000003-0000-0000-c000-000000000000. Este error se debe a un comportamiento erróneo del cliente que intenta obtener un token de acceso para el servicio de Azure AD Graph. Puesto que el recurso no está presente en AD FS, esto da como resultado 1021 Id. de evento en los servidores de AD FS. Es seguro pasar por alto las advertencias o errores para recursos 00000003-0000-0000-c000-000000000000 en AD FS. 

### <a name="why-am-i-seeing-a-warning-for-failure-to-add-the-ad-fs-service-account-to-the-enterprise-key-admins-group"></a>¿Por qué veo una advertencia para un error al agregar la cuenta de servicio de AD FS al grupo Administradores de empresas clave?
Este grupo sólo se crea cuando existe un controlador de dominio de Windows 2016 con la función FSMO PDC en el dominio. Para resolver el error, puede crear el grupo manualmente y siga el siguiente para conceder el permiso necesario después de agregar la cuenta de servicio como miembro del grupo.
1.  Abra **Usuarios y equipos de Active Directory**. 
2.  **Haga clic en** su nombre de dominio en el panel de navegación y **haga clic en** propiedades.
3.  **Haga clic en** seguridad (si falta la ficha seguridad, activar características avanzadas en el menú Ver).
4.  **Haga clic en** avanzada. **Haga clic en** agregar. **Haga clic en** seleccionar una entidad de seguridad.
5.  Aparecerá el cuadro de diálogo Seleccionar usuario, equipo, grupo o cuenta de servicio.  En Escriba el nombre del objeto al cuadro de texto seleccionar, escriba el grupo de administración de claves.  Haga clic en Aceptar.
6.  En el se aplica al cuadro de lista, seleccione **objetos de usuario descendiente**.
7.  Uso de la barra de desplazamiento, desplácese hasta el final de la página y **haga clic en** Borrar todo.
8.  En el **propiedades** sección, seleccione **leer msDS-KeyCredentialLink** y **escribir msDS-KeyCredentialLink**.

### <a name="why-does-modern-authentication-from-android-devices-fail-if-the-server-does-not-send-all-the-intermediate-certificates-in-the-chain-with-the-ssl-cert"></a>¿Por qué la autenticación moderna de dispositivos Android producirá un error si el servidor no envía todos los certificados intermedios de la cadena con el certificado SSL?

Los usuarios federados pueden experimentar la autenticación a Azure AD para las aplicaciones que usan el error de la biblioteca ADAL de Android. La aplicación obtendrá un **AuthenticationException** cuando intenta mostrar la página de inicio de sesión. En la instancia de AD FS chrome página de inicio de sesión podría denominarse como no segura.

Android: a través de todas las versiones y todos los dispositivos: no permite descargar certificados adicionales desde el **authorityInformationAccess** campo del certificado. Esto es cierto del explorador de Chrome. Cualquier certificado de autenticación de servidor que no tiene certificados intermedios se producirá este error si no se pasa la cadena de certificados completa de AD FS. 

Una solución adecuada para este problema consiste en configurar los servidores de AD FS y WAP para enviar los certificados intermedios necesarios junto con el certificado SSL. 

Al exportar el certificado SSL de un equipo, puede importar en el equipo el almacén personal, de AD FS y servidores WAP, asegúrese de exportar la clave privada y seleccione **intercambio de información Personal: PKCS #12**. 

Es importante que la casilla para **incluir todos los certificados en la ruta de certificación si es posible** se activa, así como **exportar todas las propiedades extendidas**. 

Ejecute certlm.msc en los servidores de Windows e importar el *. PFX en el almacén de certificados personales del equipo. Esto hará que el servidor para pasar la cadena de certificados completa a la biblioteca ADAL. 

>[!NOTE]
> También debe actualizarse para incluir la cadena de certificados completa si está presente el certificado de almacén de red equilibradores de carga

### <a name="does-ad-fs-support-head-requests"></a>¿Se admite las solicitudes HEAD de AD FS?
AD FS no admite las solicitudes HEAD.  Las aplicaciones no deberían utilizar las solicitudes HEAD con extremos de AD FS.  Esto puede provocar que las respuestas de error HTTP inesperado o diferida.  Además, puede ver eventos de error inesperado en el registro de eventos de AD FS.

### <a name="why-am-i-not-seeing-a-refresh-token-when-i-am-logging-in-with-a-remote-idp"></a>¿Por qué no veo un token de actualización cuando estoy iniciando sesión con un IdP remoto?
No se emite un token de actualización si el token emitido por el IdP tiene un validty de menos de 1 hora. Para asegurarse de que se emite un token de actualización, aumentar la validez del token emitido por el proveedor de identidades a más de 1 hora.

### <a name="do-we-have-any-way-to-change-rp-token-encryption-algorithm"></a>¿Hay alguna forma de cambiar el algoritmo de cifrado de tokens de usuario de confianza?
De forma predeterminada, el cifrado de tokens de usuario de confianza se establece en AES256 y no se puede cambiar en cualquier otro valor.

### <a name="on-a-mixed-mode-farm-i-get-error-when-trying-to-set-the-new-ssl-certificate-using-set-adfssslcertificate--thumbprint-how-can-i-update-the-ssl-certificate-in-a-mixed-mode-ad-fs-farm"></a>En una granja de modo mixto, obtengo errores al intentar establecer el nuevo certificado SSL mediante Set-AdfsSslCertificate-huella digital. ¿Cómo puedo actualizar el certificado SSL en una granja de AD FS de modo mixto?
En servidores de WAP todavía puede usar Set-WebApplicationProxySslCertificate. En los servidores ADFS, deberá usar netsh. Siga los pasos que se indican a continuación:

1. Seleccionar subconjunto de servidores de AD FS 2016 de mantenimiento (por ejemplo, quite del equilibrador de carga)
2. En los servidores seleccionados en #1, importe el certificado nuevo a través de MMC
3. Eliminar los certificados existentes

    a. sslcert hostnameport=fs.contoso.com:443 b de Netsh http delete. netsh http delete sslcert hostnameport = c localhost:443. netsh http delete sslcert hostnameport=fs.contoso.com:49443

4.  Agregar el nuevo certificado

    a. netsh http agregar sslcert hostnameport=fs.contoso.com:443 certhash = HUELLACERT appid = {5d89a20c-beab-4389-9447-324788eb944a} certstorename = MY sslctlstorename = AdfsTrustedDevices
    
    b. netsh http agregar sslcert hostnameport = localhost:443 certhash = HUELLACERT appid = {5d89a20c-beab-4389-9447-324788eb944a} certstorename = MY sslctlstorename = AdfsTrustedDevices
    
    c. netsh http agregar sslcert hostnameport=fs.contoso.com:49443 certhash = HUELLACERT appid = {5d89a20c-beab-4389-9447-324788eb944a} certstorename = MY sslctlstorename = AdfsTrustedDevices

5. Reinicie el servicio de ADFS en el servidor seleccionado
6. Quitar el subconjunto de los servidores WAP para mantenimiento
7. En los servidores WAP seleccionados, importe el certificado nuevo a través de MMC
8. Establecer el nuevo certificado en WAP mediante el cmdlet

    a. Set-WebApplicationProxySslCertificate -Thumbprint " CERTTHUMBPRINT"

9. Reinicie el servicio en los servidores WAP seleccionados
10. Coloque los servidores WAP y AD FS seleccionados en el entorno de producción. 
    
Realizar la actualización en el resto de AD FS y servidores WAP de forma similar. 

### <a name="is-adfs-supported-when-web-application-proxy-wap-servers-are-behind-azure-web-application-firewallwaf"></a>¿Es compatible con AD FS cuando los servidores Proxy de aplicación Web (WAP) están detrás de Firewall(WAF) de aplicación Web de Azure?
AD FS y servidores de aplicaciones Web admiten cualquier firewall que no se realizaría la terminación SSL en el punto de conexión. Además, los servidores de AD FS/WAP disponen de mecanismos integrados para evitar los ataques web comunes, como-cross-site scripting proxy ADFS y cumplir todos los requisitos definidos por el [protocolo MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx).