---
ms.assetid: aa892a85-f95a-4bf1-acbb-e3c36ef02b0d
title: Novedades en Servicios de federación de Active Directory (AD FS) para Windows Server 2016
description: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/22/2020
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: adce37d8d06399d3a00221a12f3449244720ade7
ms.sourcegitcommit: 840d1d8851f68936db3934c80796fb8722d3c64a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519487"
---
# <a name="whats-new-in-active-directory-federation-services"></a>Novedades de Servicios de federación de Active Directory (AD FS)


## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2019"></a>Novedades de Servicios de federación de Active Directory (AD FS) para Windows Server 2019

### <a name="protected-logins"></a>Inicios de sesión protegidos
A continuación se ofrece un breve resumen de las actualizaciones de los inicios de sesión protegidos disponibles en AD FS 2019:
- **Proveedores de autenticación externos como principales** los clientes ahora pueden usar productos de autenticación de terceros como primer factor y no exponer contraseñas como primer factor. En los casos en los que un proveedor de autenticación externo puede demostrar dos factores, puede reclamar MFA. 
- **Autenticación de contraseña como autenticación adicional** : los clientes tienen una opción de bandeja de entrada totalmente compatible para usar la contraseña solo para el factor adicional después de que se use la opción contraseña less como primer factor. Esto mejora la experiencia del cliente de ADFS 2016, donde los clientes tenían que descargar un adaptador de Github, que se admite tal cual. 
- **Módulo de evaluación de riesgos conectables** : ahora los clientes pueden crear sus propios módulos de complementos para bloquear determinados tipos de solicitudes durante la fase de autenticación previa. Esto facilita a los clientes el uso de la inteligencia en la nube, como Identity Protection, para bloquear los inicios de sesión para usuarios de riesgo o transacciones de riesgo.  Para obtener más información, vea [Complementos de compilación con AD FS modelo de evaluación de riesgos de 2019](../../ad-fs/development/ad-fs-risk-assessment-model.md) 
- **Mejoras de ESL** : mejora el QFE de ESL en 2016 agregando las siguientes funcionalidades.
    - Permite que los clientes estén en modo auditoría mientras están protegidos por la funcionalidad de bloqueo de la extranet "clásica" disponible desde 2012R2 de ADFS. Actualmente, 2016 clientes no tendrían protección mientras estaba en modo auditoría. 
    - Habilita el umbral de bloqueo independiente para las ubicaciones conocidas. Esto permite que varias instancias de aplicaciones que se ejecutan con una cuenta de servicio común reviertan las contraseñas con la menor cantidad de impacto. 

### <a name="additional-security-improvements"></a>Mejoras de seguridad adicionales
Las siguientes mejoras de seguridad adicionales están disponibles en AD FS 2019:
- **PSH remoto con inicio de sesión de tarjeta inteligente** : los clientes ahora pueden usar tarjetas inteligentes para conectarse de forma remota a ADFS a través de PSH y usarla para administrar todas las funciones de PSH incluyen cmdlets de PSH de varios nodos.
- **Personalización del encabezado HTTP** : ahora los clientes pueden personalizar los encabezados HTTP emitidos durante las respuestas de ADFS. Esto incluye los siguientes encabezados
     - HSTS: esto transmite que los puntos de conexión de ADFS solo se pueden usar en puntos de conexión HTTPS para que un explorador compatible aplique
     - x-Frame-Options: permite a los administradores de ADFS permitir a usuarios de confianza específicos insertar iFrames para páginas de inicio de sesión interactivas de ADFS. Debe usarse con cuidado y solo en hosts HTTPS. 
     - Encabezado futuro: también se pueden configurar otros encabezados futuros. 

Para obtener más información, consulte [Personalización de encabezados de respuesta de seguridad http con AD FS 2019](../../ad-fs/operations/customize-http-security-headers-ad-fs.md) 

### <a name="authenticationpolicy-capabilities"></a>Capacidades de autenticación/Directiva
Las siguientes capacidades de autenticación y Directiva están en AD FS 2019:
- **Especificar el método de autenticación para la autenticación adicional por RP** : los clientes ahora pueden usar reglas de notificaciones para decidir qué proveedor de autenticación adicional debe invocar para el proveedor de autenticación adicional. Esto resulta útil para dos casos de uso
    - Los clientes están pasando de un proveedor de autenticación adicional a otro. De esta manera, cuando los usuarios incorporan a un proveedor de autenticación más reciente, pueden usar grupos para controlar qué proveedor de autenticación adicional se llama.
    - Los clientes necesitan un proveedor de autenticación adicional específico (por ejemplo, un certificado) para ciertas aplicaciones. 
- **Restringir la autenticación de dispositivos basados en TLS solo a aplicaciones que lo requieran** , ahora los clientes pueden restringir las autenticaciones de dispositivo basadas en TLS de cliente solo a las aplicaciones que realizan el acceso condicional basado en dispositivos. Esto impide que se produzcan mensajes no deseados para la autenticación de dispositivos (o errores si la aplicación cliente no puede controlar) para las aplicaciones que no requieren autenticación de dispositivo basada en TLS.
- **Compatibilidad con la actualización de MFA** : actualmente, AD FS admite la posibilidad de volver a hacer la credencial de segundo factor en función de la actualización de la credencial del segundo factor. Esto permite a los clientes realizar una transacción inicial con dos factores y solo solicitar el segundo factor de forma periódica. Esto solo está disponible para las aplicaciones que pueden proporcionar un parámetro adicional en la solicitud y no es un valor configurable en ADFS. Este parámetro es compatible con Azure AD cuando está configurada la opción "recordar mi MFA para X días" y la marca "supportsMFA" está establecida en true en la configuración de confianza de dominio federado en Azure AD. 

### <a name="sign-in-sso-improvements"></a>Mejoras en SSO de inicio de sesión
Se han realizado las siguientes mejoras en el inicio de sesión único en AD FS 2019:

- [Experiencia de usuario paginada con el tema centrado](../operations/AD-FS-paginated-sign-in.md) : ADFS ahora se ha pasado a un flujo de experiencia del usuario paginada que permite que ADFS valide y proporcione una experiencia de inicio de sesión más fluida. ADFS ahora usa una interfaz de usuario centrada (en lugar del lado derecho de la pantalla). Es posible que necesite un logotipo y imágenes de fondo más recientes para alinearse con esta experiencia. Esto también refleja la funcionalidad ofrecida en Azure AD.
- **Corrección de errores: estado de SSO persistente para dispositivos Win10 cuando se realiza la autenticación de PRT**   Esto soluciona un problema en el que el estado de MFA no se mantuvo cuando se usa la autenticación de PRT para dispositivos Windows 10. El resultado del problema fue que a los usuarios finales les se les pediera la credencial de segundo factor (MFA) con frecuencia. La corrección también hace que la experiencia sea coherente cuando la autenticación de dispositivo se realiza correctamente a través de TLS de cliente y a través del mecanismo de PRT. 


### <a name="suppport-for-building-modern-line-of-business-apps"></a>Soporte técnico para compilar aplicaciones de línea de negocio modernas
Se ha agregado la siguiente compatibilidad para la compilación de aplicaciones LOB modernas en AD FS 2019:

 - **Flujo/Perfil de dispositivo de OAuth** : AD FS admite ahora el perfil de flujo de dispositivo de OAuth para realizar inicios de sesión en dispositivos que no tienen un área de la superficie de la interfaz de usuario para admitir experiencias de inicio de sesión enriquecidas. Esto permite al usuario completar la experiencia de inicio de sesión en un dispositivo diferente. Esta funcionalidad es necesaria para CLI de Azure experiencia en Azure Stack y se puede usar en otros casos. 
 - La **eliminación del parámetro "Resource"** -AD FS ahora ha quitado el requisito de especificar un parámetro de recurso que está en línea con las especificaciones actuales de OAuth. Los clientes ahora pueden proporcionar el identificador de la relación de confianza para usuario autenticado como parámetro de ámbito, además de los permisos solicitados. 
 - **Encabezados de CORS en AD FS respuestas** : ahora los clientes pueden crear aplicaciones de una sola página que permitan a las bibliotecas del lado cliente de JS validar la firma del ID_token consultando las claves de firma del documento de detección de OIDC en AD FS. 
 - **Compatibilidad con PKCE** : AD FS agrega compatibilidad con PKCE para proporcionar un flujo de código de autenticación seguro en OAuth. Esto agrega una capa adicional de seguridad a este flujo para evitar el secuestro del código y su reproducción desde un cliente diferente. 
 - **Corrección de errores: envío de x5t y notificaciones Kid** : se trata de una corrección de errores secundaria. AD FS ahora envía también la demanda ' Kid ' para denotar la sugerencia de identificador de clave para comprobar la firma. Anteriormente AD FS solo se envió como una demanda de "x5t".

### <a name="supportability-improvements"></a>Mejoras de compatibilidad
Las siguientes mejoras de compatibilidad no forman parte de AD FS 2019:
- **Enviar detalles del error a los administradores de AD FS** : permite a los administradores configurar los usuarios finales para que envíen registros de depuración relativos a un error en la autenticación del usuario final que se almacenará como un archivado para facilitar su consumo. Los administradores también pueden configurar una conexión SMTP para autoenviar el archivo comprimido a una cuenta de correo electrónico de evaluación de errores o para crear automáticamente un vale basado en el correo electrónico. 

### <a name="deployment-updates"></a>Actualizaciones de implementación
Las siguientes actualizaciones de implementación ahora se incluyen en AD FS 2019:
- **Nivel de comportamiento de la granja 2019** : al igual que con AD FS 2016, hay una nueva versión de nivel de comportamiento de granja necesaria para habilitar la nueva funcionalidad descrita anteriormente. Esto permite ir desde:
    - 2012 R2-> 2019
    - 2016-> 2019   

### <a name="saml-updates"></a>Actualizaciones de SAML
La siguiente actualización de SAML está en AD FS 2019:
- **Corrección de errores: corrección de errores en la Federación agregada** : hay numerosas correcciones de errores en torno a la compatibilidad con la Federación agregada (por ejemplo, infrecuente). Las correcciones se han realizado en torno a lo siguiente: 
  - Se ha mejorado el escalado para un número grande de entidades en el documento de metadatos de Federación agregado. anteriormente, se produciría el error "ADMIN0017". 
  - Consulta mediante el parámetro ' ScopeGroupID ' a través del cmdlet Get-AdfsRelyingPartyTrustsGroup PSH. 
  - Control de condiciones de error en torno a entityID duplicados


### <a name="azure-ad-style-resource-specification-in-scope-parameter"></a>Azure AD especificación de recursos de estilo en el parámetro de ámbito 
Anteriormente, AD FS necesitaba que el recurso y el ámbito desearan estar en un parámetro independiente en cualquier solicitud de autenticación. Por ejemplo, una solicitud de OAuth típica tendría el siguiente aspecto: 7 **https&#47;&#47;: FS.contoso.com/ADFS/OAuth2/Authorize?</br>response_type = Code & client_id = claimsxrayclient & Resource = urn: Microsoft:</br>ADFS: claimsxray & ámbito = OAuth & redirect_uri = HTTPS&#47;&#47;: adfshelp.Microsoft.com/</br> claimsxray/TokenResponse & prompt = login**
 
Con AD FS en el servidor 2019, ahora puede pasar el valor de recurso incrustado en el parámetro de ámbito. Esto es coherente con el modo en que se puede realizar la autenticación en Azure AD también. 

Ahora, el parámetro de ámbito se puede organizar como una lista separada por espacios donde cada entrada es una estructura como recurso/ámbito. 

> [!NOTE]
> Solo se puede especificar un recurso en la solicitud de autenticación. Si se incluye más de un recurso en la solicitud, AD FS devolverá un error y la autenticación no se realizará correctamente. 

### <a name="proof-key-for-code-exchange-pkce-support-for-oauth"></a>Clave de prueba para la compatibilidad de intercambio de código (PKCE) con oAuth 
Los clientes públicos de OAuth que usan la concesión de código de autorización son susceptibles de sufrir ataques de interceptación de código de autorización.  El ataque se describe correctamente en RFC 7636. Para mitigar este ataque, AD FS en Server 2019 admite la clave de prueba para el flujo de concesión de código de autorización de OAuth. 
 
Para aprovechar la compatibilidad con PKCE, esta especificación agrega parámetros adicionales a las solicitudes de autorización y token de acceso de OAuth 2,0.

![Proofkey](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/adfs2019.png)

A. El cliente crea y registra un secreto denominado "code_verifier" y deriva una versión transformada "t (code_verifier)" (denominada "code_challenge"), que se envía en la solicitud de autorización de OAuth 2,0 junto con el método de transformación "t_m". 

B. El extremo de autorización responde como de costumbre pero registra "t (code_verifier)" y el método de transformación. 

C. Después, el cliente envía el código de autorización en la solicitud de token de acceso como de costumbre, pero incluye el secreto "code_verifier" generado en (A). 

D. El AD FS transforma "code_verifier" y lo compara con "t (code_verifier)" de (B).  Se deniega el acceso si no son iguales. 

#### <a name="faq"></a>Preguntas frecuentes 
**P.** ¿Puedo pasar el valor del recurso como parte del valor del ámbito, como la forma en que se realizan las solicitudes en Azure AD? 
</br>**A.** Con AD FS en el servidor 2019, ahora puede pasar el valor de recurso incrustado en el parámetro de ámbito. Ahora, el parámetro de ámbito se puede organizar como una lista separada por espacios donde cada entrada es una estructura como recurso/ámbito. Por ejemplo,  
**< crear una solicitud de ejemplo válida >**

**P.** ¿AD FS admite la extensión PKCE?
</br>**A.** AD FS en el servidor 2019 admite la clave de prueba para el intercambio de código (PKCE) para el flujo de concesión de código de autorización OAuth 

## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>Novedades en Servicios de federación de Active Directory (AD FS) para Windows Server 2016   
Si busca información sobre versiones anteriores de AD FS, consulte los siguientes artículos:  
 [ADFS en Windows Server 2012 o 2012 R2](https://technet.microsoft.com/library/hh831502.aspx) y [AD FS 2,0](https://technet.microsoft.com/library/adfs2.aspx)  

 Servicios de federación de Active Directory (AD FS) proporciona control de acceso e inicio de sesión único en una amplia variedad de aplicaciones, como Office 365, aplicaciones SaaS basadas en la nube y aplicaciones en la red corporativa.  
* En el caso de la organización de ti, le permite proporcionar inicio de sesión y control de acceso a aplicaciones modernas y antiguas, en el entorno local y en la nube, según el mismo conjunto de credenciales y directivas.    
* Para el usuario, proporciona un inicio de sesión sin problemas con las mismas credenciales de cuenta conocidas.  
* Para el desarrollador, proporciona una manera sencilla de autenticar a los usuarios cuyas identidades viven en el directorio de la organización, de forma que pueda centrar sus esfuerzos en la aplicación, no en la autenticación o la identidad.  

En este artículo se describen las novedades de AD FS en Windows Server 2016 (AD FS 2016).  

## <a name="eliminate-passwords-from-the-extranet"></a>Eliminación de contraseñas de la extranet  
AD FS 2016 habilita tres nuevas opciones para el inicio de sesión sin contraseñas, lo que permite a las organizaciones evitar el riesgo de poner en peligro la red de contraseñas de Phish, filtradas o robadas. 

### <a name="sign-in-with-azure-multi-factor-authentication"></a>Inicio de sesión con Azure multi-factor Authentication
AD FS 2016 se basa en las capacidades de multi-factor Authentication (MFA) de AD FS en Windows Server 2012 R2 al permitir el inicio de sesión con solo un código de Azure MFA, sin escribir primero un nombre de usuario y una contraseña.

* Con Azure MFA como método de autenticación principal, se solicita al usuario su nombre de usuario y el código de OTP de la aplicación Azure Authenticator.  
* Con Azure MFA como el método de autenticación secundario o adicional, el usuario proporciona las credenciales de autenticación principal (mediante la autenticación integrada de Windows, el nombre de usuario y la contraseña, la tarjeta inteligente o el certificado de usuario o dispositivo) y, a continuación, ve un mensaje de texto, voz o inicio de sesión de Azure MFA basado en OTP.  
* Con el nuevo adaptador de Azure MFA integrado, el programa de instalación y configuración de Azure MFA con AD FS nunca fue más sencillo.
* Las organizaciones pueden aprovechar Azure MFA sin necesidad de un servidor de Azure MFA local.
* Azure MFA puede configurarse para la intranet o extranet, o como parte de cualquier directiva de control de acceso.

Para más información sobre Azure MFA con AD FS
*  [Configuración de AD FS 2016 y Azure MFA](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-and-azure-mfa)  

### <a name="password-less-access-from-compliant-devices"></a>Acceso sin contraseña desde dispositivos compatibles
AD FS 2016 se basa en las capacidades de registro de dispositivos anteriores para habilitar el inicio de sesión y el control de acceso basado en el estado de cumplimiento del dispositivo. Los usuarios pueden iniciar sesión con las credenciales del dispositivo y el cumplimiento se vuelve a evaluar cuando cambian los atributos del dispositivo, de modo que siempre se pueden garantizar las directivas que se aplican.  Esto habilita directivas como

* Habilitación del acceso solo desde dispositivos administrados y/o conformes  
* Habilitar el acceso a Extranet solo desde dispositivos administrados y/o conformes  
* Requerir multi-factor Authentication para equipos que no están administrados o no son compatibles  

AD FS proporciona el componente local de las directivas de acceso condicional en un escenario híbrido. Al registrar los dispositivos con Azure AD para el acceso condicional a los recursos en la nube, la identidad del dispositivo puede usarse también para las directivas de AD FS.

![novedades](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/ADFS_ITPRO4.png)  

 Para más información sobre el uso del acceso condicional basado en dispositivos en la nube   
 *  [Acceso condicional de Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)

Para obtener más información acerca del uso del acceso condicional basado en dispositivos con AD FS
*  [Planeación del acceso condicional basado en dispositivos con AD FS](../../ad-fs/deployment/Plan-Device-based-Conditional-Access-on-Premises.md)  
* [Directivas de Access Control en AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="sign-in-with-windows-hello-for-business"></a>Iniciar sesión con Windows Hello para empresas  

> [!NOTE]
> Actualmente, Google Chrome y el [nuevo Microsoft Edge basado en](https://www.microsoft.com/edge?form=MB110A&OCID=MB110A) los exploradores de proyectos de código abierto de cromo no se admiten para el inicio de sesión único (SSO) basado en explorador con Microsoft Windows Hello para empresas. Use Internet Explorer o una versión anterior de Microsoft Edge.  

Los dispositivos de Windows 10 presentan Windows Hello y Windows Hello para empresas, y reemplazan las contraseñas de usuario con credenciales de usuario enlazadas a un dispositivo seguro protegidas por el gesto de un usuario (un PIN, un gesto biométrico como una huella digital o reconocimiento facial). AD FS 2016 admite estas nuevas funcionalidades de Windows 10 para que los usuarios puedan iniciar sesión en AD FS aplicaciones desde la intranet o la extranet sin necesidad de proporcionar una contraseña.

Para obtener más información acerca del uso de Microsoft Windows Hello para empresas en su organización
*  [Habilitación de Windows Hello para empresas en tu organización](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)

## <a name="secure-access-to-applications"></a>Acceso seguro a las aplicaciones

### <a name="modern-authentication"></a>Autenticación moderna
AD FS 2016 admite los protocolos modernos más recientes que proporcionan una mejor experiencia de usuario para Windows 10, así como los dispositivos y aplicaciones iOS y Android más recientes.  

Para obtener más información, vea [escenarios de AD FS para desarrolladores](../../ad-fs/overview/ad-fs-openid-connect-oauth-flows-scenarios.md)  


### <a name="configure-access-control-policies-without-having-to-know-claim-rules-language"></a>Configurar directivas de control de acceso sin tener que conocer el lenguaje de reglas de notificaciones  
Anteriormente, los administradores de AD FS tenían que configurar directivas mediante el lenguaje de reglas de notificaciones de AD FS, lo que dificultaba la configuración y el mantenimiento de las directivas. Con las directivas de control de acceso, los administradores pueden usar plantillas integradas para aplicar directivas comunes como
* Permitir solo el acceso a la intranet
* Permitir a todos y requerir MFA de extranet
* Permitir a todos y requerir MFA de un grupo específico

Las plantillas son fáciles de personalizar mediante un proceso controlado por el Asistente para agregar excepciones o reglas de directivas adicionales y se pueden aplicar a una o varias aplicaciones para una aplicación coherente de la Directiva.

Para obtener más información, consulte [directivas de control de acceso en AD FS.](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="enable-sign-on-with-non-ad-ldap-directories"></a>Habilitar el inicio de sesión con directorios que no son de AD LDAP  
Muchas organizaciones tienen una combinación de Active Directory y directorios de terceros. Con la adición de AD FS compatibilidad con la autenticación de usuarios almacenados en directorios compatibles con LDAP v3, AD FS ahora se puede usar para:
* Usuarios de otros directorios compatibles con LDAP v3
* Los usuarios de Active Directory bosques a los que no está configurada una confianza bidireccional de Active Directory
* Usuarios de Active Directory Lightweight Directory Services (AD LDS)

Para obtener más información, consulte [configurar la AD FS para autenticar a los usuarios almacenados en directorios LDAP.](../../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)  

## <a name="better-sign-in-experience"></a>Mejor experiencia de inicio de sesión
### <a name="customize-sign-in-experience-for-ad-fs-applications"></a>Personalización de la experiencia de inicio de sesión para aplicaciones AD FS  
Nos hemos escuchado de que la capacidad de personalizar la experiencia de inicio de sesión para cada aplicación sería una mejora de la facilidad de uso, especialmente para las organizaciones que proporcionan inicio de sesión para aplicaciones que representan varias empresas o marcas diferentes.  

Anteriormente, AD FS en Windows Server 2012 R2 ofrecía una experiencia de inicio de sesión común para todas las aplicaciones de usuario de confianza, con la capacidad de personalizar un subconjunto de contenido basado en texto por aplicación. Con Windows Server 2016, puede personalizar no solo los mensajes, sino imágenes, el logotipo y el tema web por aplicación. Además, puede crear nuevos temas web personalizados y aplicarlos por usuario de confianza.  

Para obtener más información, consulte [AD FS personalización de inicio de sesión de usuario.](../../ad-fs/operations/AD-FS-user-sign-in-customization.md)  



## <a name="manageability-and-operational-enhancements"></a>Mejoras operativas y de administración  
En la siguiente sección se describen los escenarios operativos mejorados que se incluyen en Servicios de federación de Active Directory (AD FS) en Windows Server 2016.  

### <a name="streamlined-auditing-for-easier-administrative-management"></a>Auditoría simplificada para facilitar la administración administrativa  
En AD FS para Windows Server 2012 R2, se generaron numerosos eventos de auditoría para una única solicitud y la información relevante sobre una actividad de inicio de sesión o de emisión de tokens está ausente (en algunas versiones de AD FS) o se reparte en varios eventos de auditoría. De forma predeterminada, se desactivan los eventos de auditoría AD FS debido a su naturaleza detallada.  
Con el lanzamiento de AD FS 2016, la auditoría se ha vuelto más optimizada y menos detallada.  

Para obtener más información, consulte [mejoras de auditoría en AD FS en Windows Server 2016.](../../ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server.md)  

### <a name="improved-interoperability-with-saml-20-for-participation-in-confederations"></a>Mejora de la interoperabilidad con SAML 2,0 para participar en confederaciones  
AD FS 2016 contiene compatibilidad con el protocolo SAML adicional, incluida la compatibilidad para importar confianzas basadas en metadatos que contienen varias entidades. Esto le permite configurar AD FS para participar en confederaciones como la Federación infrecuente y otras implementaciones que se ajustan al estándar eGov 2,0.  

Para obtener más información, consulte [interoperabilidad mejorada con SAML 2,0.](../../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)  

### <a name="simplified-password-management-for-federated-o365-users"></a>Administración simplificada de contraseñas para usuarios federados de O365  
Puede configurar Servicios de federación de Active Directory (AD FS) (AD FS) para enviar notificaciones de expiración de contraseñas a las relaciones de confianza para usuario autenticado (aplicaciones) que están protegidas por AD FS. La forma en que se usan estas notificaciones depende de la aplicación. Por ejemplo, con Office 365 como usuario de confianza, las actualizaciones se han implementado en Exchange y Outlook para notificar a los usuarios federados de sus contraseñas que pronto están expiradas.  

Para obtener más información, vea [configurar AD FS para enviar notificaciones de expiración de contraseñas.](../../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)  

### <a name="moving-from-ad-fs-in-windows-server-2012-r2-to-ad-fs-in-windows-server-2016-is-easier"></a>Pasar de AD FS en Windows Server 2012 R2 a AD FS en Windows Server 2016 es más fácil  
Anteriormente, la migración a una nueva versión de AD FS requería la exportación de la configuración de la granja anterior y la importación a una nueva granja de servidores en paralelo.  

Ahora, es mucho más fácil pasar de AD FS en Windows Server 2012 R2 a AD FS en Windows Server 2016. Basta con agregar un nuevo servidor de Windows Server 2016 a una granja de servidores de Windows Server 2012 R2 y la granja actuará en el nivel de comportamiento de la granja de servidores de Windows Server 2012 R2, por lo que parece y se comporta igual que una granja de Windows Server 2012 R2.  

A continuación, agregue los nuevos servidores de Windows Server 2016 a la granja, Compruebe la funcionalidad y quite los servidores más antiguos del equilibrador de carga. Una vez que todos los nodos de la granja ejecutan Windows Server 2016, está listo para actualizar el nivel de comportamiento de la granja a 2016 y comenzar a usar las nuevas características.  

Para obtener más información, consulte [actualizar a AD FS en Windows Server 2016.](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)  
