---
ms.assetid: acc9101b-841c-4540-8b3c-62a53869ef7a
title: "PREGUNTAS MÁS FRECUENTES DE AD FS DE 2016"
description: "Preguntas más frecuentes de AD FS de 2016"
author: jenfieldmsft
ms.author: billmath
manager: femila
ms.date: 03/06/2018
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 313447d2c92c15505434ec5c39898ca84aef46db
ms.sourcegitcommit: 556361fe7c73c75d6cdc46a67dc88679fbe89c61
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2018
---
# <a name="ad-fs-frequently-asked-questions-faq"></a>AD FS preguntas más frecuentes (P+F)

>Se aplica a: Windows Server 2016

La siguiente documentación es un lugar a las preguntas más frecuentes en relación con los servicios de federación de Active Directory.  El documento se han dividido en grupos en función del tipo de pregunta.

## <a name="deployment"></a>Implementación 

### <a name="how-can-i-upgrademigrate-from-previous-versions-of-ad-fs"></a>¿Cómo puedo actualizar o migrar de versiones anteriores de AD FS
Puedes actualizar AD FS mediante una de las siguientes acciones:


- Windows Server 2012 R2 AD FS a Windows Server 2016 AD FS
    - [Actualizar a AD FS en Windows Server 2016 con una base de datos WID](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)
    - [Actualizar a AD FS en Windows Server 2016 con una base de datos SQL](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL.md)
- Windows Server 2012 AD FS a Windows Server 2012 R2 AD FS
    - [Migrar a AD FS en Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- AD FS 2.0 para Windows Server 2012 AD FS
    - [Migrar a AD FS en Windows Server 2012](https://technet.microsoft.com/library/jj647765.aspx)
- AD FS 1.x a AD FS 2.0 
    - [Actualizar desde AD FS 1.x a AD FS 2.0](https://technet.microsoft.com/library/ff678035.aspx)

Si necesitas actualizar de AD FS 2.0 o 2.1 (Windows Server 2008 R2 o Windows Server 2012), debes usar los scripts en el cuadro (ubicados en C:\Windows\ADFS).

### <a name="why-does-ad-fs-installation-require-a-reboot-of-the-server"></a>¿Por qué requiere un reinicio del servidor de instalación de AD FS?

Se agregó compatibilidad con HTTP/2 en Windows Server 2016, pero no se pueden usar HTTP/2 para la autenticación de certificado de cliente.  Dado que muchos escenarios de AD FS hacen uso de autenticación de certificado de cliente y una cantidad considerable de los clientes no no admiten Reintentar solicita con HTTP/1.1, AD FS granja configuración volver a configura la local configuración del servidor HTTP a HTTP/1.1.  Esto requiere un reinicio del servidor.  

### <a name="is-using-windows-2016-wap-servers-to-publish-the-ad-fs-farm-to-the-internet-without-upgrading-the-back-end-ad-fs-farm-supported"></a>¿Utiliza Windows 2016 WAP servidores para publicar la granja de servidores de AD FS a internet sin actualizar la granja de servidores de AD FS back-end compatible?
Sí, esta configuración es compatible, pero no hay nuevas características de AD FS 2016 admitiría en esta configuración.  Esta configuración está pensada para ser temporal durante la fase de migración de AD FS 2012 R2a AD FS 2016 y no debe implementarse durante largos períodos de tiempo.

## <a name="design"></a>Diseño

### <a name="what-third-party-multi-factor-authentication-providers-are-available-for-ad-fs"></a>¿Qué proveedores de autenticación multifactor de terceros están disponibles para AD FS? 
A continuación se incluye una lista de proveedores de terceros que somos conscientes de.  Siempre puede haber proveedores disponibles que no conocemos y actualizaremos la lista a medida que Aprendemos sobre ellos.

- [Servicios de seguridad y la identidad Gemalto](http://www.gemalto.com/identity)
- [inWebo el servicio de autenticación de empresa](http://www.inwebo.com/)
- [Conector de personas MFA API de inicio de sesión](https://www.loginpeople.com)
- [Agente de autenticación de RSA SecurID para servicios de federación de Microsoft Active Directory](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)
- [Agente del servicio (SAS) de autenticación de SafeNet de AD FS](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)
- [Servicio de autenticación de Mobile Swisscom ID](http://swisscom.ch/mid)
- [Servicio de protección de identificador (VIP) y la validación de Symantec](http://www.symantec.com/vip-authentication-service) 

### <a name="are-third-party-proxies-supported-with-ad-fs"></a>¿Se admiten los servidores proxy de terceros con AD FS?
Sí, los servidores proxy de terceros pueden colocarse delante el Proxy de aplicación Web, pero debe ser compatible con cualquier proxy de terceros la [protocolo MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx) para usarse en lugar del Proxy de aplicación Web.

### <a name="what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip"></a>¿Qué servidores proxy de terceros están disponibles para AD FS que admiten MS-ADFSPIP?

A continuación se incluye una lista de proveedores de terceros que somos conscientes de.  Siempre puede haber proveedores disponibles que no conocemos y actualizaremos la lista a medida que Aprendemos sobre ellos.

- [F5 Administrador de directivas de acceso](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-third-party-integration-13-1-0/12.html#guid-1ee8fbb3-1b33-4982-8bb3-05ae6868d9ee)

### <a name="where-is-the-capacity-planning-sizing-spreadsheet-for-ad-fs-2016"></a>¿Dónde está el planeamiento de capacidad hoja de cálculo de tamaño de AD FS 2016?
Se puede descargar la versión de AD FS 2016 de la hoja de cálculo [aquí](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
También puede usarse para AD FS en Windows Server 2012 R2.

### <a name="how-can-i-ensure-my-ad-fs-and-wap-servers-support-apples-atp-requirements"></a>¿Cómo puedo asegurarme mi soporte de los servidores de AD FS y WAP requisitos de Promesa de Apple?

Apple ha publicado un conjunto de requisitos llamado seguridad de transporte de aplicación (ATS) que pueden afectar a las llamadas de las aplicaciones de iOS que autentican en AD FS.  Puedes asegurarte de que tu AD FS y servidores WAP cumplan asegurándose de que admiten la [requisitos para conectar mediante ATS](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  
En particular, debes comprobar que los servidores de AD FS y WAP admitan TLS 1.2 y que el conjunto de cifrado negociado de la conexión TLS admitirá confidencialidad directa.

Puede habilitar y deshabilitar las versiones de SSL 2.0 y 3.0 y TLS 1.0 y 1.1, 1.2 con [administrar los protocolos SSL en AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md).

Para garantizar su AD FS y WAP, servidores negocian solo conjuntos de cifrado TLS que admiten la Promesa, puedes deshabilitar todos los conjuntos de cifrado que no están en la [lista de los conjuntos de cifrado compatibles con Promesa](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  Para ello, usa el [cmdlets de Windows PowerShell de TLS](https://technet.microsoft.com/itpro/powershell/windows/tls/index). 


## <a name="operations"></a>Operaciones

### <a name="how-do-i-replace-the-ssl-certificate-for-ad-fs"></a>¿Cómo se puede reemplazar el certificado SSL de AD FS? 
El certificado SSL de AD FS no es lo mismo que el certificado de comunicaciones de servicio de AD FS se encuentra en el complemento de administración de AD FS.  Para cambiar el certificado SSL de AD FS, debes usar PowerShell. Sigue las instrucciones en el artículo siguiente:

[Administración de certificados SSL en AD FS y WAP 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

### <a name="how-can-i-enable-or-disable-tlsssl-settings-for-ad-fs"></a>¿Cómo habilitar o deshabilitar la configuración de SSL/TLS de AD FS
Para deshabilitar o habilitar los protocolos SSL y conjuntos de cifrado, usa lo siguiente:

[Administrar los protocolos SSL en AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md)

### <a name="does-the-proxy-ssl-certificate-have-to-be-the-same-as-the-ad-fs-ssl-certificate"></a>¿El certificado SSL de proxy que se tiene que ser el mismo que el certificado SSL de AD FS?  
Usa las siguientes directrices en relación con el certificado SSL de proxy y el certificado SSL de AD FS:


- Si se usa el proxy para las solicitudes de proxy AD FS que usan la autenticación integrada de Windows, el certificado SSL de proxy deben ser el mismo (usa la misma clave) como el certificado SSL de servidor de federación
- Si la propiedad de AD FS "ExtendedProtectionTokenCheck" está habilitada (el valor predeterminado en AD FS), el certificado SSL de proxy debe ser el mismo (usa la misma clave) como el certificado SSL de servidor de federación
- De lo contrario, el certificado SSL de proxy puede tener una clave diferente en el certificado SSL de AD FS, pero debe cumplir con la misma [requisitos](../overview/AD-FS-2016-Requirements.md)

### <a name="how-can-i-configure-promptlogin-behavior-for-ad-fs"></a>¿Cómo puedo configurar símbolo del sistema = comportamiento de inicio de sesión de AD FS?
Para obtener información sobre cómo configurar el símbolo del sistema = inicio de sesión, consulta [pedir los servicios de federación de Active Directory = compatibilidad con parámetros de inicio de sesión](../operations/AD-FS-Prompt-Login.md).

### <a name="how-can-i-configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>¿Cómo puedo configurar los exploradores usen la autenticación integrada de Windows (WIA) con AD FS?

Para obtener información sobre cómo configurar exploradores [configurar exploradores para usar la autenticación integrada de Windows (WIA) con AD FS](../operations/Configure-AD-FS-Browser-WIA.md).


### <a name="how-long-are-ad-fs-tokens-valid"></a>¿Cuánto son tokens de AD FS válidas?

¿A menudo, esta pregunta significa 'cuánto hacer los usuarios para tener inicio de sesión único (SSO) sin tener que introducir nuevas credenciales, y cómo puedo como administrador controlar que'?  Este comportamiento y las opciones de configuración que controlan, se describen en el artículo [aquí](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/operations/ad-fs-2016-single-sign-on-settings).

Las duraciones predeterminadas de los diversos cookies y tokens se enumeran a continuación (así como los parámetros que rigen las duraciones):

**Dispositivos registrados**

- Imp y SSO cookies: 90 días como máximos, se rigen por PSSOLifeTimeMins. (De dispositivos se usan al menos cada 14 días, que está controlado por DeviceUsageWindow)

- Token de actualización: calcula en función de la anterior para ofrecer un comportamiento coherente

- access_token: 1 hora de manera predeterminada, en función de la parte del usuario de confianza

- ID_token: igual que el token de acceso

**Dispositivos registrados detenido**

- Las cookies SSO: 8 horas de manera predeterminada, se rigen por SSOLifetimeMins.  Cuando se habilita mantener iniciado sesión (KMSI), el valor predeterminado es 24 horas y puede configurarse mediante KMSILifetimeMins.


- Token de actualización: 8 horas de manera predeterminada. 24 horas con KMSI habilitado

- access_token: 1 hora de manera predeterminada, en función de la parte del usuario de confianza

- ID_token: igual que el token de acceso

### <a name="does-ad-fs-support-http-strict-transport-security-hsts"></a>¿AD FS compatible con HTTP Strict Transport Security (HSTS)?  

HTTP Strict Transport Security (HSTS) es un mecanismo de directiva de seguridad de web lo que ayuda a mitigar los ataques de degradación de protocolo y secuestro de cookie para servicios que tienen los extremos HTTP y HTTPS. Permite a los servidores web declarar que los exploradores web (u otros complying agentes de usuario) solo deben interactuar con él mediante HTTPS y nunca mediante el protocolo HTTP.

Todos los extremos de AD FS para el tráfico de autenticación web se abren exclusivamente a través de HTTPS.  Como resultado, AD FS eficaz para reducir las amenazas que proporciona el mecanismo de directivas de HTTP Strict Transport Security (por diseño no es ninguna cambiar a HTTP debido a que no hay ningún agente de escucha en HTTP). Además, AD FS impide que las cookies se envíen a otro servidor con extremos del protocolo HTTP marcando todas las cookies con la marca segura.

Por lo tanto, la implementación de HSTS en un servidor de AD FS no es necesario porque nunca se puede degradar.  Para fines de cumplimiento, servidores de AD FS cumplan estos requisitos, porque no pueden usar HTTP y todas las cookies se marcan seguras.

### <a name="x-ms-forwarded-client-ip-does-not-contain-the-ip-of-the-client-but-contains-ip-of-the-firewall-in-front-of-the-proxy-where-can-i-get-the-right-ip-of-the-client"></a>X-ms-reenvía-ip del cliente no contiene la dirección IP del cliente, pero contiene IP del servidor de seguridad delante del proxy. ¿Dónde se puede obtener la dirección IP de derecha del cliente?
No se recomienda hacer terminación SSL antes WAP. En caso de que se realiza la terminación SSL delante el WAP, la X-ms-reenvía-ip del cliente contendrá la dirección IP del dispositivo de la red delante WAP. A continuación te mostramos una breve descripción de varias direcciones IP relacionados con las notificaciones que son compatibles con AD FS:
 - x-ms--dirección ip del cliente: red IP del dispositivo que conectado al STS.  En el caso de una solicitud extranet siempre contiene la dirección IP en el WAP.
 - x-ms-reenvía-ip del cliente: varios valores reclamación que contendrá los valores que se reenvía al ADFS por Exchange Online además de la dirección IP del dispositivo que conectado a la WAP.
 - Userip: Extranet solicitudes de esta notificación contendrá el valor de x-ms-reenvía-ip del cliente.  Para las solicitudes de intranet, esta notificación contendrá el mismo valor que x-ms--dirección ip del cliente.

### <a name="i-am-trying-to-get-additional-claims-on-the-user-info-endpoint-but-its-only-returning-subject-how-can-i-get-additional-claims"></a>Estoy tratando de obtener notificaciones adicionales en el extremo de la información de usuario, pero solo devuelve a asunto. ¿Cómo se puede obtener notificaciones adicionales?
El extremo de userinfo ADFS siempre devuelve la reclamación asunto tal como se especifica en los estándares de OpenID. AD FS no proporciona notificaciones adicionales solicitados a través del extremo UserInfo. Si necesitas reclamaciones adicionales en el token del identificador, consulte [Tokens de identificador personalizado en AD FS](../development/custom-id-tokens-in-ad-fs.md).

### <a name="why-do-i-see-alot-of-1021-errors-on-my-ad-fs-servers"></a>¿Por qué veo una gran cantidad de 1021 errores en los servidores de AD FS?
Este evento se registra, por lo general, para un acceso de recurso no válidas en AD FS para recursos 00000003-0000-0000-c000-000000000000. Este error aparece cuando un comportamiento del cliente en el que intenta obtener un token de acceso para el servicio de Azure AD gráfico erróneo. Dado que el recurso no está presente en AD FS, esto da como resultado 1021 de identificador de evento en los servidores de AD FS. Resulta seguro omitir advertencias o errores para recursos # ° 00000003-0000-0000-c000-000000000000, en AD FS. 

### <a name="why-am-i-seeing-a-warning-for-failure-to-add-the-ad-fs-service-account-to-the-enterprise-key-admins-group"></a>¿Por qué me aparece una advertencia del error al agregar la cuenta de servicio de AD FS al grupo de administradores de organización clave?
Este grupo solo se crea cuando existe un controlador de dominio de 2016 de Windows con la función de PDC FSMO en el dominio. Para resolver el error, puedes crear el grupo manualmente y sigue el siguiente para dar el permiso necesario después de agregar la cuenta del servicio como miembro del grupo.
1.  Abre **equipos y usuarios de Active Directory**. 
2.  **Haz clic en** tu nombre de dominio desde el panel de navegación y **haga clic en** propiedades.
3.  **Haz clic en** seguridad (si falta la pestaña seguridad, activar características avanzadas en el menú de vista).
4.  **Haz clic en** avanzada. **Haz clic en** agregar. **Haz clic en** seleccionar una entidad de seguridad.
5.  Aparece el cuadro de diálogo Seleccionar usuarios, equipo, cuenta de servicio o grupo.  En escribir el nombre del objeto al cuadro de selección de texto, escribe la clave de grupo de administradores.  Haz clic en Aceptar.
6.  En el cuadro de lista, selecciona **objetos de usuario descendiente**.
7.  Usar la barra de desplazamiento, desplázate hasta la parte inferior de la página y **haga clic en** Borrar todo.
8.  En la **propiedades** sección, selecciona **leer msDS KeyCredentialLink** y **escribir msDS KeyCrendentialLink**.

### <a name="why-does-modern-authentication-from-android-devices-fail-if-the-server-does-not-send-all-the-intermediate-certificates-in-the-chain-with-the-ssl-cert"></a>¿Por qué moderna autenticación desde dispositivos Android producirá un error si el servidor envía todos los certificados intermedios en la cadena con el certificado SSL?

Los usuarios federados pueden experimentar una autenticación a Azure AD para aplicaciones que usan el error de biblioteca ADAL Android. La aplicación obtenga una **AuthenticationException** cuando intenta mostrar la página de inicio de sesión. En la AD FS de cromo página de inicio de sesión podría llamarse como no seguro.

Android - entre todas las versiones y todos los dispositivos - no permite descargar certificados adicionales desde la **authorityInformationAccess** campo del certificado. Esto sucede del explorador Chrome también. Ningún certificado de autenticación de servidores que le faltan los certificados intermedios producirá este error si no se pasa la cadena de certificados completa de AD FS. 

Una solución adecuada para este problema es configurar los servidores de AD FS y WAP para enviar los certificados intermedios necesarios junto con el certificado SSL. 

Al exportar el certificado SSL, desde un equipo, para ser importados en el almacén del equipo personal, de los servidores de AD FS y WAP, asegúrate de exportar la privada en clave y selecciona **intercambio de información Personal: PKCS #12**. 

Es importante que la casilla de verificación para **incluir todos los certificados en la ruta de certificado si es posible** está activada, así como **exportar todas las propiedades ampliadas**. 

Ejecutar certlm.msc en los servidores de Windows e importar el *. PFX en el almacén de certificados Personal del equipo. Esto hará que el servidor pasar la cadena de certificados completa a la biblioteca ADAL. 

>[!NOTE]
> También se debe actualizar el certificado de almacén de red equilibradores de carga para incluir la cadena de certificados completa si está presente

### <a name="does-ad-fs-support-head-requests"></a>¿AD FS admite solicitudes HEAD?
AD FS no admite solicitudes HEAD.  Las aplicaciones no deberían utilizar solicitudes de cabeza en los extremos de AD FS.  Esto puede causar respuestas de error HTTP que son inesperados o diferido.  Además, puedes ver los eventos de error inesperado en el registro de eventos de AD FS.

### <a name="why-am-i-not-seeing-a-refresh-token-when-i-am-logging-in-with-a-remote-idp"></a>¿Por qué me no aparece un token de actualización cuando estoy iniciar sesión con un IdP remoto?
No se emite un token de actualización si el token de IdP tiene un validty de menos de 1 hora. Para garantizar que se emite un token de actualización, aumenta la validez de token emitido por el IdP más de 1 hora.
