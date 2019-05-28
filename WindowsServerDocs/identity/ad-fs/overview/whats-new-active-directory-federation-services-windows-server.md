---
ms.assetid: aa892a85-f95a-4bf1-acbb-e3c36ef02b0d
title: Novedades en Servicios de federación de Active Directory (AD FS) para Windows Server 2016
description: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 04/23/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fbb289c16d82da79aded49e3af4134ac7f6df325
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188696"
---
# <a name="whats-new-in-active-directory-federation-services"></a>Novedades de Servicios de federación de Active Directory (AD FS)


## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2019"></a>Novedades de Active Directory Federation Services para Windows Server 2019

### <a name="protected-logins"></a>Inicios de sesión protegidos
Este es un breve resumen de las actualizaciones de los inicios de sesión disponibles en AD FS 2019 protegidos:
- **Los proveedores de autenticación externos como principal** -los clientes pueden ahora usar 3rd productos de autenticación de terceros como el primer factor y no se exponen las contraseñas como el primer factor. En los casos donde un proveedor de autenticación externo puede resultar 2 factores puede reclamar MFA. 
- **Autenticación de contraseña como autenticación adicional** -los clientes tienen una opción totalmente compatible de la Bandeja de entrada a usar contraseñas solo para factorizar el adicionales después de una contraseña menos opción se utiliza como el primer factor. Esto mejora la experiencia del cliente de ADFS 2016 donde los clientes tenían para descargar un adaptador de github que es compatible tal cual. 
- **Módulo de evaluación de riesgos acoplable** -los clientes ahora pueden crear su propios módulos bloquear ciertos tipos de solicitudes durante la fase de autenticación previa de complemento. Esto facilita a los clientes usar inteligencia en la nube como la protección de identidad para bloquear inicios de sesión para usuarios de riesgo o transacciones de riesgo.  Para obtener más información, consulte [ compilar los complementos con el modelo de evaluación del riesgo de AD FS de 2019](../../ad-fs/development/ad-fs-risk-assessment-model.md) 
- **Mejoras de ESL** -mejora el QFE ESL en 2016 al agregar las siguientes funcionalidades
    - Permite a los clientes a estar en modo auditoría mientras esté protegido por la funcionalidad de bloqueo de extranet 'clásico' disponible desde 2012 R2 AD FS. Actualmente, los clientes de 2016 no tendrían protección en el modo auditoría. 
    - Permite el umbral de bloqueo independientes para ubicaciones conocidas. Esto hace posible que varias instancias de aplicaciones que se ejecutan con una cuenta de servicio común para sustituir las contraseñas con la menor cantidad de impacto. 

### <a name="additional-security-improvements"></a>Mejoras de seguridad adicionales
Las siguientes mejoras de seguridad adicionales están disponibles en AD FS 2019:
- **PSH remoto mediante el inicio de sesión de tarjeta inteligente** : los clientes pueden ahora usar las tarjetas inteligentes en equipo remoto conexión a AD FS a través de PSH y uso que administrar PSH todas las funciones incluyen cmdlets PSH de varios nodos.
- **Personalización del encabezado HTTP** -los clientes ahora pueden personalizar encabezados HTTP que se emite durante las respuestas ADFS. Esto incluye los siguientes encabezados
     - HSTS: Esto transmite que los puntos de conexión de AD FS solo se utilice en puntos de conexión HTTPS para un explorador compatible para exigir
     - x-frame-options: Permite a los administradores de AD FS permitir entidades específicas de usuario de confianza incrustar iFrames para las páginas de inicio de sesión interactivo de ADFS. Debe usarse con cuidado y solo en hosts HTTPS. 
     - Encabezado futura: Los encabezados adicionales que futuros pueden configurarse también. 

Para obtener más información, consulte [encabezados de respuesta HTTP personalizar seguridad con AD FS de 2019](../../ad-fs/operations/customize-http-security-headers-ad-fs.md) 

### <a name="authenticationpolicy-capabilities"></a>Capacidades de autenticación de directiva
Son las siguientes capacidades de directiva de autenticación de AD FS 2019:
- **Especifique el método de autenticación para la autenticación adicional por RP** -los clientes pueden ahora el uso de reglas para decidir qué proveedor de autenticación adicional para invocar el proveedor de autenticación adicional de las notificaciones. Esto es útil para casos de uso 2
    - Los clientes están pasando de proveedor de autenticación adicional uno a otro. Así como que los usuarios incorporarlos a un proveedor de autenticación más reciente puede usar grupos para controlar qué autenticación adicional que se denomina proveedor.
    - Los clientes tienen necesidades de un proveedor de autenticación adicional específico (por ejemplo, el certificado) para ciertas aplicaciones. 
- **Restringir la autenticación de dispositivo en función de TLS solo a las aplicaciones que lo requieran** -los clientes ahora pueden restringir el cliente TLS en la función de las autenticaciones de dispositivo en solo acceso condicional basado en las aplicaciones que realizan el dispositivo. Esto evita que los mensajes no deseados para la autenticación de dispositivo (o errores si no puede controlar la aplicación cliente) para las aplicaciones que no requieren autenticación de dispositivo en función de TLS.
- **Compatibilidad de actualización MFA** -AD FS admite ahora la capacidad para volver a realizar 2nd credencial factor en función de la actualización de la credencial del 2nd factor. Esto permite a los clientes realizar una transacción inicial con 2 factores y pedir solo el 2nd factor de forma periódica. Esto solo está disponible para las aplicaciones que pueden proporcionar un parámetro adicional en la solicitud y no es un valor configurable en ADFS. Este parámetro es compatible con Azure AD cuando se configura "Recordar mi MFA para X días" y la marca 'supportsMFA' está establecida en true en la configuración de confianza de dominio federado en Azure AD. 

### <a name="sign-in-sso-improvements"></a>Mejoras en el inicio de sesión SSO
Se realizaron las siguientes mejoras de inicio de sesión único inicio de sesión de AD FS 2019:

- [Paginar la experiencia del usuario con el tema centrado](../operations/AD-FS-paginated-sign-in.md) -AD FS ahora se movieron a un flujo UX paginado que permite a ADFS validar y proporcionar una experiencia de inicio de sesión más más suave. AD FS ahora usa una interfaz de usuario centrado (en lugar de la derecha de la pantalla). Puede requerir más reciente del logotipo e imágenes de fondo para alinearse con esta experiencia. Esto refleja también funcionalidad ofrecida en Azure AD.
- **Corrección de errores: Estado de inicio de sesión único persistente para los dispositivos de Win10 al realizar la autenticación PRT** esto soluciona un problema donde el estado MFA no se conserva cuando se usa autenticación PRT para dispositivos Windows 10. El resultado del problema era que los usuarios finales haría que se pidan credenciales de 2nd factor (MFA) con frecuencia. La corrección también hace que la experiencia sean coherentes cuando la autenticación de dispositivo se realiza correctamente a través de TLS de cliente y el mecanismo PRT. 


### <a name="suppport-for-building-modern-line-of-business-apps"></a>Soporte técnico para compilar modernas aplicaciones de línea de negocio
Se agregó la siguiente compatibilidad para compilar modernas aplicaciones LOB a AD FS 2019:

 - **Flujo de OAuth dispositivo/perfil** -AD FS admite ahora el perfil de flujo de dispositivo de OAuth para llevar a cabo los inicios de sesión en los dispositivos que no tienen un área expuesta de la interfaz de usuario para admitir las experiencias enriquecidas de inicio de sesión. Esto permite al usuario completar la experiencia de inicio de sesión en un dispositivo diferente. Esta funcionalidad es necesaria para la experiencia de la CLI de Azure en Azure Stack y se puede usar en otros casos. 
 - **Eliminación del parámetro 'Resource'** -AD FS ahora ha quitado el requisito de especificar un parámetro de recurso que está en consonancia con las especificaciones de Oauth actuales. Los clientes ahora pueden proporcionar el identificador de la confianza de usuario de confianza como el parámetro de ámbito además a los permisos solicitados. 
 - **Encabezados de CORS en las respuestas de AD FS** -los clientes ahora pueden crear aplicaciones de página única que permiten las bibliotecas de JS validar la firma del id_token mediante la consulta de las claves de firma desde el documento de descubrimiento OIDC en AD FS de cliente. 
 - **Compatibilidad con PKCE** -AD FS agrega compatibilidad con PKCE para proporcionar un flujo de código de autenticación segura dentro de OAuth. Esto agrega una capa adicional de seguridad para este flujo para evitar el secuestro del código y reproducirlo desde un cliente diferente. 
 - **Corrección de errores: Notificación de envío x5t y kid** -se trata de una corrección de errores menor. AD FS ahora además envía la notificación 'kid' para denotar la sugerencia de Id. de clave para comprobar la firma. AD FS solo había enviado anteriormente esto como notificación "x5t".

### <a name="supportability-improvements"></a>Mejoras de compatibilidad
Las siguientes mejoras de compatibilidad no forman parte de AD FS 2019:
- **Enviar detalles del error a los administradores de AD FS** -permite a los administradores configurar los usuarios finales para enviar los registros de depuración relativas a un error en la autenticación de usuario final que se almacenará como un zip archivado para facilitar su consumo. Los administradores también pueden configurar una conexión SMTP a automail el archivo comprimido a una cuenta de correo electrónico de evaluación de prioridades o automática crear un vale según el correo electrónico. 

### <a name="deployment-updates"></a>Actualizaciones de implementación
Ahora se incluyen las siguientes actualizaciones de implementación de AD FS 2019:
- **Granja de servidores de nivel de comportamiento 2019** : como con AD FS 2016, hay una nueva versión de nivel de comportamiento de la granja de servidores que se requiere para habilitar la nueva funcionalidad se ha explicado anteriormente. Esto permite pasar de:
    - 2012 R2-> 2019
    - 2016 -> 2019   

### <a name="saml-updates"></a>Actualizaciones SAML
La siguiente actualización SAML está en AD FS 2019:
- **Corrección de errores: Corregir errores de federación agregada** -han numerosas correcciones de errores en torno a la compatibilidad con la federación agregada (por ejemplo, InCommon). Las correcciones se han estado disponibles lo siguiente: 
  - Mejora el ajuste de escala para grandes n.º de entidades en el documento de metadatos de federación agregados. Anteriormente, esto produciría un error con el error "ADMIN0017". 
  - Realice consultas mediante el parámetro 'ScopeGroupID' a través del cmdlet Get-AdfsRelyingPartyTrustsGroup PSH. 
  - Controlar las condiciones de error en torno a entityID duplicado


### <a name="azure-ad-style-resource-specification-in-scope-parameter"></a>Especificación de recursos de estilo de Azure AD en el parámetro de ámbito 
Anteriormente, AD FS requiere el recurso deseado y el ámbito en un parámetro independiente en cualquier solicitud de autenticación. Por ejemplo, una solicitud de oauth típica tendría el aspecto, como a continuación: 7 **https:&#47;&#47;fs.contoso.com/adfs/oauth2/authorize?</br> response_type = código & client_id = claimsxrayclient & recurso = urn: microsoft:</br>adfs:claimsxray á & mbito = oauth y los redirect_uri = https:&#47;&#47;adfshelp.microsoft.com/</br> ClaimsXray / TokenResponse & prompt = inicio de sesión**
 
Con AD FS en el servidor de 2019, ahora puede pasar el valor del recurso incrustado en el parámetro de ámbito. Esto es coherente con cómo se puede realizar la autenticación en Azure AD también. 

El parámetro de ámbito puede organizarse ahora como una lista separada por espacios, donde cada entrada es la estructura como recurso y el ámbito. Por ejemplo  

**< crear una solicitud de ejemplo válido >**
> [!NOTE]
> Solo un recurso puede especificarse en la solicitud de autenticación. Si se incluye más de un recurso en la solicitud, AD FS devolverá que un error y la autenticación no se realizará correctamente. 

### <a name="proof-key-for-code-exchange-pkce-support-for-oauth"></a>Clave de prueba para obtener soporte técnico de Exchange de código (PKCE) para oAuth 
Los clientes públicos de OAuth mediante la concesión de código de autorización son susceptibles a los ataques de interceptación de código de autorización.  El ataque es también se describe en RFC 7636. Para mitigar este ataque, AD FS en el servidor de 2019 admite la clave de prueba para código Exchange (PKCE) para el flujo de concesión de código de autorización de OAuth. 
 
Para aprovechar la compatibilidad con PKCE, esta especificación agrega parámetros adicionales para la autorización de OAuth 2.0 y las solicitudes de Token de acceso.

![Proofkey](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/adfs2019.png)

A. El cliente crea y registra un secreto llamado el "valor de code_verifier" y deriva de una versión transformada "t(code_verifier)" (denominado el "code_challenge"), que se envía en la solicitud de OAuth 2.0 autorización junto con el método de transformación "t_m". 

b. El extremo de autorización responde como de costumbre, pero los registros "t(code_verifier)" y el método de transformación. 

C. El cliente, a continuación, envía el código de autorización en la solicitud de Token de acceso como de costumbre, pero incluye el secreto "valor de code_verifier" generado en (A). 

D. AD FS transforma el "valor de code_verifier" y lo compara con "t(code_verifier)" de (B).  Acceso denegado si no son iguales. 

#### <a name="faq"></a>Preguntas más frecuentes 
**Q.** ¿Puedo pasar el valor de recurso como parte del valor de ámbito, similar a cómo se realizan solicitudes en Azure AD? 
</br>**A.** Con AD FS en el servidor de 2019, ahora puede pasar el valor del recurso incrustado en el parámetro de ámbito. El parámetro de ámbito puede organizarse ahora como una lista separada por espacios, donde cada entrada es la estructura como recurso y el ámbito. Por ejemplo  
**< crear una solicitud de ejemplo válido >**

**Q.** ¿Admite la extensión PKCE AD FS?
</br>**A.** AD FS en el servidor de 2019 admite la clave de prueba para código Exchange (PKCE) para el flujo de concesión de código de autorización de OAuth 

## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>Novedades en Servicios de federación de Active Directory (AD FS) para Windows Server 2016   
Si busca información sobre las versiones anteriores de AD FS, consulte los artículos siguientes:  
 [ADFS en Windows Server 2012 o 2012 R2](https://technet.microsoft.com/library/hh831502.aspx) y [AD FS 2.0](https://technet.microsoft.com/library/adfs2.aspx)  

 Servicios de federación de Active Directory proporciona control de acceso y el inicio de sesión único en una amplia variedad de aplicaciones, incluido Office 365, en la nube en función de las aplicaciones SaaS y aplicaciones en la red corporativa.  
* Para la organización de TI, permite proporcionar inicio de sesión en y control de acceso a las aplicaciones modernas y heredadas, local y en la nube, según el mismo conjunto de credenciales y las directivas.    
* Para el usuario, proporciona inicio de sesión sin problemas sobre el uso de las credenciales de cuenta de la misma y conocida.  
* Para los desarrolladores, proporciona una manera sencilla de autenticar a los usuarios cuyas identidades residen en el directorio de la organización para que pueda centrar sus esfuerzos en su aplicación, no la autenticación o identity.  

En este artículo se describe cuáles son las novedades de AD FS en Windows Server 2016 (AD FS 2016).  

## <a name="eliminate-passwords-from-the-extranet"></a>Eliminar contraseñas desde la Extranet  
Poner en peligro de AD FS 2016 habilita tres nuevas opciones de inicio de sesión sin contraseñas, permitiendo a las organizaciones evitar el riesgo de red de suplantar las identidades, las contraseñas perdidas o robadas. 

### <a name="sign-in-with-azure-multi-factor-authentication"></a>Inicie sesión con autenticación multifactor de Azure
AD FS 2016 basa en la autenticación multifactor capacidades (MFA) de AD FS en Windows Server 2012 R2, ya que permite el inicio de sesión sobre el uso de un código de Azure MFA, sin especificar primero un nombre de usuario y una contraseña.

* Con Azure MFA como método de autenticación principal, se solicita al usuario su nombre de usuario y el código OTP desde la aplicación Azure Authenticator.  
* Con Azure MFA como método de autenticación adicionales o secundarios, el usuario proporciona autenticación principal credenciales (mediante la autenticación integrada de Windows, nombre de usuario y contraseña, tarjeta inteligente o certificado de usuario o dispositivo), a continuación, ve un mensaje de texto, voz o OTP en la función de inicio de sesión de Azure MFA.  
* Con el nuevo adaptador de Azure MFA integrado, el programa de instalación y configuración para Azure MFA con AD FS nunca ha sido más sencillo.
* Las organizaciones pueden aprovechar las ventajas de Azure MFA sin necesidad de un servidor Azure MFA local.
* Azure MFA puede configurarse para intranet o extranet o como parte de ninguna directiva de control de acceso.

Para obtener más información sobre Azure MFA con AD FS
*  [Configuración de AD FS 2016 y Azure MFA](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-and-azure-mfa)  

### <a name="password-less-access-from-compliant-devices"></a>Sin contraseña acceso desde dispositivos compatibles
AD FS 2016 se basa en las funcionalidades de registro de dispositivo anterior para habilitar inicio de sesión y el control de acceso en función del estado de cumplimiento de dispositivos. Los usuarios pueden iniciar sesión con las credenciales del dispositivo y cumplimiento se vuelve a evaluar cuando cambian los atributos del dispositivo, por lo que siempre puede asegurarse de que se exigen directivas.  Esto permite, como las directivas

* Habilitar el acceso solo desde dispositivos que son compatibles o no administrados  
* Habilitar el acceso de Extranet solo desde dispositivos que son compatibles o no administrados  
* Requerir autenticación multifactor para los equipos que no están administrados o no compatibles  

AD FS proporciona el componente de entorno local en directivas de acceso condicional de un escenario híbrido. Al registrar los dispositivos con Azure AD para el acceso condicional a los recursos en la nube, la identidad del dispositivo puede usarse para directivas de AD FS, así.

![Novedades](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/ADFS_ITPRO4.png)  

 Para obtener más información sobre el uso de dispositivos en función de acceso condicional en la nube   
 *  [Acceso condicional de Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)

Para obtener más información sobre el uso de dispositivos en función de acceso condicional con AD FS
*  [Planeación de dispositivo en función de acceso condicional con AD FS](../../ad-fs/deployment/Plan-Device-based-Conditional-Access-on-Premises.md)  
* [Directivas de Control de acceso en AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="sign-in-with-windows-hello-for-business"></a>Inicie sesión con Windows Hello para empresas   
Dispositivos Windows 10 introducen Hello de Windows y Windows Hello para empresas, reemplazando las contraseñas de usuario con credenciales de usuario seguras de dispositivo enlazado protegidas por los gestos del usuario (un PIN, un gesto, como el reconocimiento facial o de huella biométrico). AD FS 2016 admite estas nuevas capacidades de Windows 10 para que los usuarios pueden iniciar sesión AD FS aplicaciones desde la intranet o extranet sin tener que proporcionar una contraseña.

Para obtener más información sobre el uso de Microsoft Windows Hello para empresas en su organización
*  [Habilitar Windows Hello para empresas en su organización](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)

## <a name="secure-access-to-applications"></a>Acceso seguro a aplicaciones

### <a name="modern-authentication"></a>Autenticación moderna
AD FS 2016 es compatible con los últimos protocolos modernos que proporcionan una mejor experiencia de usuario para Windows 10, así como la más reciente de iOS y dispositivos Android y aplicaciones.  

Para obtener más información, consulte [escenarios de AD FS para desarrolladores](../../ad-fs/overview/AD-FS-Scenarios-for-Developers.md)  


### <a name="configure-access-control-policies-without-having-to-know-claim-rules-language"></a>Configurar directivas de control de acceso sin tener que conocer el lenguaje de reglas de notificación  
Anteriormente, los administradores de AD FS tenían que configurar directivas mediante el lenguaje de reglas de notificación de AD FS, lo que dificulta a configurar y mantener las directivas. Con las directivas de control de acceso, los administradores pueden usar plantillas integradas para aplicar directivas comunes, como
* Permitir el acceso a la intranet solo
* Permitir todos los usuarios y solicitar MFA desde la Extranet
* Permitir todos los usuarios y solicitar MFA a un grupo específico

Las plantillas son fáciles de personalizar mediante un asistente controlado por el proceso para agregar las excepciones o reglas de directivas adicionales y se pueden aplicar a una o varias aplicaciones para la aplicación de directivas coherentes.

Para obtener más información, consulte [las directivas de control de acceso en AD FS.](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="enable-sign-on-with-non-ad-ldap-directories"></a>Habilitar inicio de sesión con directorios LDAP que no sean de AD  
Muchas organizaciones tienen una combinación de Active Directory y los directorios de terceros. Con la adición de compatibilidad de AD FS para autenticar a los usuarios almacenados en directorios LDAP v3 conforme, AD FS ahora se puede usar para:
* Usuarios de otros fabricantes, directorios LDAP v3 compatibles con
* Usuarios en bosques de Active Directory al que no está configurada una confianza bidireccional de Active Directory
* Usuarios en servicios de directorio ligero de Active Directory (AD LDS)

Para obtener más información, consulte [configurar AD FS para autenticar a los usuarios almacenados en directorios LDAP.](../../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)  

## <a name="better-sign-in-experience"></a>Mejor experiencia de inicio de sesión
### <a name="customize-sign-in-experience-for-ad-fs-applications"></a>Personalizar la experiencia para aplicaciones de AD FS de inicio de sesión  
Hemos sabido que la capacidad de personalizar la experiencia de inicio de sesión para cada aplicación sería una mejora de gran facilidad de uso, especialmente para organizaciones que proporcione el inicio de sesión en las aplicaciones que representan varias compañías diferentes o las marcas de usted.  

Anteriormente, AD FS en Windows Server 2012 R2 proporciona un inicio de sesión común en la experiencia para todas las aplicaciones de confianza, con la capacidad de personalizar un subconjunto de texto basada en contenido por aplicación. Con Windows Server 2016, puede personalizar no sólo los mensajes, pero imágenes, web y el logotipo de tema por aplicación. Además, puede crear nuevos temas web personalizados y aplicar estos por usuarios de confianza entidad.  

Para obtener más información, consulte [personalización de inicio de sesión de usuario de AD FS.](../../ad-fs/operations/AD-FS-user-sign-in-customization.md)  



## <a name="manageability-and-operational-enhancements"></a>Facilidad de uso y mejoras operativas  
La siguiente sección describe los escenarios operativos mejorados que se introdujeron con servicios de federación de Active Directory en Windows Server 2016.  

### <a name="streamlined-auditing-for-easier-administrative-management"></a>Simplificada de auditoría para que sea más fácil administración  
En AD FS para Windows Server 2012 R2 existe eran numerosos eventos de auditoría generados para una única solicitud y la información relevante acerca de un registro o actividad de emisión de tokens está ausente (en algunas versiones de AD FS) o se extiende en varios eventos de auditoría. De forma predeterminada, AD FS están desactivados eventos de auditoría debido a su naturaleza detallado.  
Con el lanzamiento de AD FS 2016, la auditoría se ha convertido en más sencilla y menos detallado.  

Para obtener más información, consulte [mejoras de auditorías para AD FS en Windows Server 2016.](../../ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server.md)  

### <a name="improved-interoperability-with-saml-20-for-participation-in-confederations"></a>Mejora la interoperabilidad con SAML 2.0 para la participación en confederaciones  
AD FS 2016 contiene soporte de protocolo SAML adicionales, incluida la compatibilidad para la importación de las confianzas en función de metadatos que contiene varias entidades. Esto le permite configurar AD FS para participar en confederaciones como federación InCommon y otras implementaciones que cumplen el estándar de 2.0 eGov.  

Para obtener más información, consulte [mejora la interoperabilidad con SAML 2.0.](../../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)  

### <a name="simplified-password-management-for-federated-o365-users"></a>Administración de contraseñas simplificada de federado a los usuarios de Office 365  
Puede configurar los servicios de federación de Active Directory (AD FS) para enviar notificaciones de expiración de contraseña para el usuario autenticado (aplicaciones) que está protegida por AD FS. ¿Cómo se usan estas notificaciones depende de la aplicación. Por ejemplo, con Office 365, como el usuario de confianza, se han implementado las actualizaciones para Exchange y Outlook para notificar a los usuarios federados de sus contraseñas pronto-a-haber-expirado.  

Para obtener más información, consulte [configurar AD FS para enviar notificaciones de expiración de contraseña.](../../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)  

### <a name="moving-from-ad-fs-in-windows-server-2012-r2-to-ad-fs-in-windows-server-2016-is-easier"></a>Movimiento de AD FS en Windows Server 2012 R2 a AD FS en Windows Server 2016 es más fácil  
Anteriormente, la migración a una nueva versión de AD FS requiere exportar configuración de la granja de servidores antigua e importar a una granja nueva, en paralela.  

Ahora, migración de AD FS en Windows Server 2012 R2 a AD FS en Windows Server 2016 se ha convertido en mucho más fácil. Basta con agregar un nuevo servidor de Windows Server 2016 a una granja de servidores de Windows Server 2012 R2 y la granja de servidores, que actuará en el nivel de comportamiento de granja de servidores de Windows Server 2012 R2, por lo que busca y se comporta como una granja de servidores de Windows Server 2012 R2.  

A continuación, agregar nuevos servidores de Windows Server 2016 a la granja de servidores, compruebe la funcionalidad y quite los servidores más antiguos del equilibrador de carga. Una vez que todos los nodos de la granja de servidores ejecutan Windows Server 2016, está listo para actualizar el nivel de comportamiento de la granja de servidores a 2016 y empezar a usar las nuevas características.  

Para obtener más información, consulte [actualización a AD FS en Windows Server 2016.](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)  
