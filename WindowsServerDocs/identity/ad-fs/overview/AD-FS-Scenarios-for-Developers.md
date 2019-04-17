---
ms.assetid: 8a64545b-16bd-4c13-a664-cdf4c6ff6ea0
title: Escenarios de AD FS para desarrolladores
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 753b2b235cb1d73ab47588f8f229410c1f81db40
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="ad-fs-scenarios-for-developers"></a>Escenarios de AD FS para desarrolladores

>Se aplica a: Windows Server 2016

AD FS en Windows Server 2016 [AD FS 2016] te permite agregar conectar estándar de OpenID y OAuth 2.0 basados en la autenticación y autorización a las aplicaciones que vas a desarrollar la industria y tiene estas aplicaciones autenticar usuarios directamente en AD FS.    
  
AD FS 2016 también admite la federación de WS, WS-Trust, y SAML protocolos y perfiles se admiten en las versiones anteriores.  Si estás interesado en instrucciones de desarrollo para estos protocolos, consulta el artículo aquí.  En este artículo se centrará en cómo usar y beneficiarse de la compatibilidad con el protocolo más reciente.  
  
## <a name="why-modern-authentication"></a>¿Por qué autenticación moderna  
Si bien puedes seguir usándolo AD FS de inicio de sesión en con WS-federación, WS-Trust y protocolos SAML tal y como lo tienen antes, con los protocolos más reciente, obtienes las ventajas siguientes:  
  
* **Simplicidad y la coherencia**  
    * Usa el mismo conjunto de patrones y API para habilitar el inicio de sesión para:   
        *   varios tipos de aplicaciones (servidor, escritorio, mobile, navegador)  
        *   varias plataformas (android, iOS, Windows)  
        *   aplicaciones en la red corporativa u hospedados en la nube  
    * Usar el mismo conjunto de bibliotecas que ya pueden usar para autenticar usuarios en Azure AD  
* **Flexibilidad**  
    * Además de autorización de usuario estándar, habilitar escenarios más complejos, como:      
        * ? segmentos 3 el inicio de sesión de flujos en el que un usuario autoriza una aplicación o servicio web para acceder a los recursos que se encuentran con otra aplicación web o servicio.    
        * ? Flujos de servidor a servidor en el que un servicio de nivel intermedio, accede a una API de back-end  
        * ? Aplicaciones de una página (SPA) basadas en JavaScript  
* **Soporte técnico del sector**  
    * OAuth 2.0 y conectar OpenID disfrutan el uso de ancho a través de la industria, para conocimiento de estos patrones le ayudará a habilitar la autenticación y autorización fuera así como un entorno de Active Directory  
  
## <a name="how-it-works-the-basics"></a>Cómo funciona: conceptos básicos  
Autenticación de AD FS moderna puedes agregar a la aplicación mediante el mismo conjunto de herramientas y las bibliotecas que ya pueden usar para autenticar usuarios en Azure AD.   
  
En escenarios de AD FS por supuesto, es AD FS y no Azure AD que actúa como el proveedor de identidad y autorización servidor.  De lo contrario los conceptos son exactamente los mismos: los usuarios proporcionar sus credenciales y obtener tokens, directa o a través de un intermediario, para tener acceso a recursos.  
  
El escenario más básico consta de un usuario o "propietario de recursos", interactuar con un explorador para acceder a una aplicación web:  
  
![AD FS para desarrolladores](media/ADFS_DEV_1.png)  
  
La aplicación web se denomina a un "cliente" porque se inicia la solicitud al servidor de autorización (AD FS) para un token de acceso al recurso.  El recurso puede estar hospedado por la propia aplicación web, o puede ser accesible como una API web en algún lugar de la red o internet.   El usuario o "propietario de recursos" autoriza la aplicación web de cliente para recibir ese token de acceso al proporcionar credenciales para el servidor de autorización.    
  
## <a name="how-it-works-components"></a>Cómo funciona: componentes  
OAuth 2.0 y conectar OpenID escenarios hacer AD FS utiliza el mismo conjunto de herramientas y las bibliotecas que usas cuando Azure AD es el proveedor de identidad.  Estos componentes son:  
* Biblioteca de autenticación de Active Directory (ADAL): bibliotecas de cliente que facilitan recopilar las credenciales de usuario, creación y envío de solicitudes de tokens y recuperar los tokens resultantes.    
* Software intermedio OWIN (Open Web Interface para. NET): OWIN mientras es un proyecto de la Comunidad en función, Microsoft ha creado un conjunto del servidor de las bibliotecas que para la protección de web API con conectar OpenID y OAuth 2.0 y las aplicaciones web  
  
En el siguiente diagrama se muestran las funciones de estos componentes:  
  
![AD FS para desarrolladores](media/ADFS_DEV_2.png)  
  
## <a name="modeling-these-scenarios-in-ad-fs-2016"></a>Estos escenarios de modelado en AD FS de 2016  
  
### <a name="application-groups"></a>Grupos de aplicaciones  
Para representar estos escenarios en la directiva de AD FS, hemos incluido un nuevo concepto denominado grupos de aplicaciones.  Un grupo de aplicaciones puede contener cualquier número y la combinación de los siguientes tipos fundamentales de la aplicación:  
  
  
  
Grupo de aplicaciones / tipo de aplicación  |Descripción  |Función    
---------|---------|---------  
Aplicación nativa     |  A veces denominado a cliente público, esto pretende ser una aplicación de cliente que se ejecuta en un equipo o dispositivo y con la que el usuario interactúa.       | Tokens de solicitudes desde el servidor de autorización (AD FS) para el acceso de usuario a los recursos.  Envía solicitudes HTTP a recursos protegidos, con los tokens como los encabezados HTTP.        
Aplicación de servidor     |   Una aplicación web que se ejecuta en un servidor y suele ser accesible para los usuarios a través de un explorador.  Dado que es capaz de mantener su propia 'clave secreta de cliente' o las credenciales, a veces se denomina a un cliente confidencial.      | Tokens de solicitudes desde el servidor de autorización (AD FS) para el acceso de usuario a los recursos.  Envía solicitudes HTTP a recursos protegidos, con los tokens como los encabezados HTTP.               
Web API     |  El recurso final el usuario tiene acceso. Considerar como la nueva representación de "usuarios de confianza".| Consume tokens de los clientes  
  
### <a name="differences-from-ad-fs-2012-r2"></a>Diferencias de AD FS 2012 R2  
Grupos de aplicaciones combinan los elementos de autorización y confianza que AD FS 2012 R2 expuestos por separado, como los usuarios de confianza, los clientes y los permisos de la aplicación.  
  
Las tablas siguientes se comparan los métodos que correspondiente objetos de confianza de la aplicación se crean en AD FS 2012 R2 vs AD FS de 2016:  
  
AD FS en Windows Server 2012 R2|En PowerShell|Administración de AD FS  
---------|---------|---------  
Agregar a cliente nativo|AdfsClient agregar|NA  
Agregar la aplicación de servidor como cliente|AdfsClient agregar|NA  
Agregar Web API y recursos|AdfsRelyingPartyTrust agregar|Crear usuario de confianza  
  
AD FS DE 2016|En PowerShell|Administración de AD FS  
---------|---------|---------  
Agregar a cliente nativo|AdfsNativeClientApplication agregar|Agregar grupo de aplicaciones nativo  
Agregar la aplicación de servidor como cliente|AdfsServerApplication agregar|Agrega el grupo de aplicaciones de servidor  
Agregar Web API y recursos|AdfsWebApiApplication agregar|Agrega el grupo de aplicaciones de Web API  
  
### <a name="application-permissions-and-consent"></a>Consentimiento y permisos de la aplicación  
De manera predeterminada, los clientes de un grupo de aplicaciones pueden acceder a los recursos en el mismo grupo.  El administrador no tiene que configurar los permisos de aplicación específica.  Grupos de aplicaciones también permiten a los administradores especificar los ámbitos permitidos como openid o user_impersonation.  Las siguientes descripciones de escenario especifican exactamente qué ámbitos son necesarios para qué escenario.  
  
Dado que AD FS utiliza un modelo de consentimiento del administrador, los usuarios no se les pide consentimiento al obtener acceso a recursos.  Al configurar el grupo de aplicaciones, el administrador en vigor proporciona su consentimiento en nombre de todos los usuarios de la aplicación.  
  
## <a name="supported-scenarios"></a>Escenarios admitidos  
La siguiente sección describe los escenarios que admitimos con más detalle.  
  
### <a name="tokens-used"></a>Tokens usados  
Hacer que estos escenarios usa tres tipos de tokens:  
  
* **ID_token:** token de un JWT usado para representar la identidad del usuario. La reclamación 'aud' o público de la id_token coincide con el identificador de cliente de la aplicación nativos o servidor.  
* **access_token:** token un JWT usan Oauth y OpenID conectan escenarios y están destinadas para usarlo en el recurso.  La reclamación 'aud' o público de este token debe coincidir con el identificador del recurso o API Web.  
* **refresh_token:** este token se envía en lugar de recopilar las credenciales de usuario para proporcionar un inicio de sesión único en la experiencia.  Este token se emitió y consumida por AD FS y no es legible para los clientes o los recursos.    
  
### <a name="native-client-to-web-api"></a>Cliente nativo de Web API  
Este escenario, permite al usuario de una aplicación de cliente nativo para llamar a una API Web de 2016 protegido de AD FS.  
* La aplicación cliente nativo usa ADAL para enviar autorización y token las solicitudes que AD FS, solicitar las credenciales del usuario según sea necesario, a continuación, envía el token resultante como un encabezado HTTP en la solicitud a la API para Web  
* [Esta parte es solo con fines de demostración] La API web lee las notificaciones desde el objeto de ClaimsPrincipal que obtuviste el token de acceso enviado por el cliente y los envía al cliente.  
  
![Descripción del flujo de protocolo](media/ADFS_DEV_3.png)  
  
1.  La aplicación cliente nativo inicia el flujo con una llamada a la biblioteca ADAL.  Esto desencadena basada en un navegador HTTP GET para la AD FS autorizar extremo:  
  
**Solicitud de autorización:**  
GET https://fs.contoso.com/adfs/oauth2/authorize?  
  
Parámetro|Valor  
---------|---------  
response_type|"código"  
recursos|Punto de reunión ID (identificador) de Web API en el grupo de aplicaciones  
client_id|Id. de la aplicación nativa en el grupo de aplicaciones de cliente  
redirect_uri|Redirigir el URI de aplicación nativa en el grupo de aplicaciones  
  
**Respuesta de la solicitud de autorización:**  
Si el usuario no ha iniciado sesión antes, se pide al usuario las credenciales.    
AD FS responde devolviendo un código de autorización como el parámetro "code" en el componente de consulta de la redirect_uri.  Por ejemplo: HTTP/1.1 302 encuentra ubicación: **http://redirect_uri:80 /? código =&lt;código&gt;.**  
  
2.  El cliente nativo, a continuación, envía el código, junto con los siguientes parámetros, hacia el extremo de token de AD FS:  
  
**Solicitud de token:**  
POST https://fs.contoso.com/adfs/oauth2/token  
  
Parámetro|Valor  
---------|---------  
grant_type|"authorization_code" 
código|código de autorización de 1  
recursos|Punto de reunión ID (identificador) de Web API en el grupo de aplicaciones  
client_id|Id. de la aplicación nativa en el grupo de aplicaciones de cliente  
redirect_uri|Redirigir el URI de aplicación nativa en el grupo de aplicaciones  
  
**Respuesta de la solicitud de token:**  
AD FS responde con un HTTP 200 con la access_token, refresh_token y id_token en el cuerpo.  
  
3.  La aplicación nativa, a continuación, envía la parte access_token de la respuesta anterior como el encabezado de autorización en la solicitud HTTP a la API web.  
  
### <a name="single-sign-on-behavior"></a>Comportamiento de inicio de sesión único  
Cliente posterior solicitudes dentro de 1 hora (de forma predeterminada) la access_token seguirán siendo válidos en la memoria caché, y una nueva solicitud de no desencadenará todo el tráfico a AD FS.  ADAL automáticamente cargará el access_token desde la memoria caché.  
  
Después de que el token de acceso expira, ADAL enviará automáticamente una solicitud de basado en token de actualización hacia el extremo de token de AD FS (omitiendo la solicitud de autorización automáticamente).  
**Solicitud de token de actualización:**  
POST https://fs.contoso.com/adfs/oauth2/token
   

Parámetro|Valor|
---------|---------
grant_type|"refresh_token"|
recursos|Punto de reunión ID (identificador) de Web API en el grupo de aplicaciones|
client_id|Id. de la aplicación nativa en el grupo de aplicaciones de cliente
refresh_token|el token de actualización emitido por AD FS en respuesta a la solicitud de token inicial

  
  
**Respuesta de la solicitud de token de actualización:**  
Si el token de actualización es dentro de < SSO_period >, la solicitud dará como resultado un nuevo token de acceso. No se pide al usuario las credenciales.  Para obtener más información sobre la consulta de la configuración de inicio de sesión único [AD FS inicio de sesión único en configuración](../../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)  
  
Si el token de actualización ha expirado, resultados de la solicitud de HTTP 401 con error "invalid_grant" y "error_description" "MSIS9615: ha caducado el token de actualización recibido en el parámetro refresh_token". En este caso, ADAL envía automáticamente una nueva solicitud de autorización que es similar a 1 # anterior.    
  
### <a name="web-browser-to-web-app"></a>Explorador Web para la aplicación Web   
En este escenario, un usuario con un explorador necesita acceder a recursos hospedados por una aplicación web.    
Existen dos escenarios que logran esto.  
  
#### <a name="oauth-confidential-client"></a>Cliente confidencial de OAuth  
Este escenario es parecido a lo anterior en los que hay una solicitud de autorización, seguida de un código para el intercambio de tokens.  La aplicación web (modelada como una aplicación de servidor de AD FS) inicia la solicitud de autorización a través del explorador e intercambia el código para el token (si se conecta directamente a AD FS)  
  
![Descripción del flujo de protocolo](media/ADFS_DEV_4.png)  
  
1.  Inicia de aplicación Web solicitar una autorización a través del explorador, que envía un HTTP GET a la de AD FS autoriza extremo  
**Solicitud de autorización**:  
GET https://fs.contoso.com/adfs/oauth2/authorize?  
  
Parámetro|Valor  
---------|---------  
response_type|"código"  
recursos|Punto de reunión ID (identificador) de Web API en el grupo de aplicaciones  
client_id|Identificador de cliente de la aplicación nativa en el grupo de aplicaciones  
redirect_uri|Redirigir el URI de aplicación web (aplicación de servidor) en el grupo de aplicaciones  
  
Respuesta de la solicitud de autorización:  
Si el usuario no ha iniciado sesión antes, se pide al usuario las credenciales.  
AD FS responde devolviendo un código de autorización, como el parámetro "code" en el componente de consulta de la redirect_uri, por ejemplo: HTTP/1.1 302 encuentra ubicación: https://webapp.contoso.com/?code=&lt;código&gt;.  
  
2.  Como resultado la 302 anterior, el explorador inicia un HTTP GET para la aplicación web, por ejemplo: GET http://redirect_uri:80 /? código =&lt;código&gt;.   
  
3.  En este momento en que la aplicación web, que hayan recibido el código, inicia una solicitud para el extremo de token de AD FS, envío de la siguiente  
**Solicitud de token:**  
POST https://fs.contoso.com/adfs/oauth2/token  
  
Parámetro|Valor  
---------|---------  
grant_type|"authorization_code"  
código|código de autorización de 2 anterior  
recursos|Punto de reunión ID (identificador) de Web API en el grupo de aplicaciones  
client_id|Identificador de cliente de la aplicación web (aplicación de servidor) en el grupo de aplicaciones  
redirect_uri|Redirigir el URI de aplicación web (aplicación de servidor) en el grupo de aplicaciones  
client_secret|Clave secreta de la aplicación web (aplicación de servidor) en el grupo de la aplicación. **Nota: Credenciales del cliente no tiene que ser un client_secret.  AD FS admite la capacidad de usar certificados o autenticación integrada de Windows también.**  
  
**Respuesta de la solicitud de token:**  
AD FS responde con un HTTP 200 con la access_token, refresh_token y id_token en el cuerpo.  
reclamaciones  
4.  La web, aplicaciones, a continuación, ya sea consume la parte access_token de la respuesta anterior (en el caso en que la propia aplicación web hospeda el recurso) o de lo contrario, envía como el encabezado de autorización en la solicitud HTTP a la API web.  
  
#### <a name="single-sign-on-behavior"></a>Comportamiento de inicio de sesión único  
Mientras el token de acceso todavía son válido para 1 hora (de forma predeterminada) en la caché del cliente, se puede considerar que la segunda solicitud funcionará como en el escenario de cliente nativo anterior - que una nueva solicitud de no desencadenará todo el tráfico a AD FS como el token de acceso automáticamente cargará desde la memoria caché por ADAL.  Sin embargo, es posible que la aplicación web puede envía autorización diferente y solicitudes de token, vincula el primero a través de la dirección URL distinta, como en nuestro ejemplo.  
  
En este caso, es la cookie SSO de AD FS explorador que permite AD FS emitir un nuevo código de autorización sin pedir confirmación al usuario las credenciales. A continuación, llama a la aplicación web a AD FS para intercambiar el nuevo código de autorización para un nuevo token de acceso.  No se pide al usuario las credenciales.  
  
De lo contrario, si la aplicación web está inteligentes suficiente para saber que si el usuario ya se ha autenticado, puede omitirse la solicitud de autorización y cualquier:  
* el token de acceso en caché, si no expirado, se recupera y utiliza, o   
* una solicitud de token en función de la solicitud puede enviarse hacia el extremo de token de AD FS, tal como se describe a continuación  
  
**Solicitud de token de actualización:**  
POST https://fs.contoso.com/adfs/oauth2/token
   
Parámetro|Valor  
---------|---------  
grant_type|"refresh_token"  
recursos|Punto de reunión ID (identificador) de Web API en el grupo de aplicaciones  
client_id|Identificador de cliente de la aplicación web (aplicación de servidor) en el grupo de aplicaciones  
refresh_token|Actualizar el token de AD FS en respuesta a la solicitud de token inicial  
client_secret|Clave secreta de la aplicación web (aplicación de servidor) en el grupo de aplicaciones  
  
**Respuesta de la solicitud de token de actualización:**  
Si el token de actualización es dentro de < SSO_period >, la solicitud dará como resultado un nuevo token de acceso. No se pide al usuario las credenciales. Para obtener más información sobre la consulta de la configuración de inicio de sesión único [AD FS inicio de sesión único en configuración](../../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)   
  
Si el token de actualización ha expirado, resultados de la solicitud de HTTP 401 con error "invalid_grant" y "error_description" "MSIS9615: ha caducado el token de actualización recibido en el parámetro refresh_token". En este caso, ADAL envía automáticamente una nueva solicitud de autorización que es similar a 1 # anterior.    
  
#### <a name="openid-connect-hybrid-flow"></a>OpenID conexión: Flujo de híbrida  
Este escenario es parecido a lo anterior en el existe una solicitud de autorización iniciada por la aplicación web a través de redireccionamiento del explorador y un código para el intercambio de tokens de la aplicación web a AD FS.  La diferencia en este escenario es que AD FS emite un id_token como parte de la respuesta de la solicitud de autorización inicial.  
  
![Descripción del flujo de protocolo](media/ADFS_DEV_5.png)  
  
1.  Inicia de aplicación Web solicitar una autorización a través del explorador, que envía un HTTP GET a la de AD FS autoriza extremo  
  
**Solicitud de autorización:**  
GET https://fs.contoso.com/adfs/oauth2/authorize?  
  
Parámetro|Valor  
---------|---------  
response_type|"código + id_token"  
response_mode|"form_post"  
recursos|Punto de reunión ID (identificador) de Web API en el grupo de aplicaciones  
client_id|Identificador de cliente de la aplicación web (aplicación de servidor) en el grupo de aplicaciones  
redirect_uri|Redirigir el URI de aplicación web (aplicación de servidor) en el grupo de aplicaciones  
  
**Respuesta de la solicitud de autorización:**  
Si el usuario no ha iniciado sesión antes, se pide al usuario las credenciales.  
AD FS responde con un HTTP 200 y un formulario que contiene la continuación como oculta elementos:  
* código: el código de autorización  
* ID_token: un token JWT que contiene reclamaciones que describe la autenticación de usuario  
2.  El formulario se registra automáticamente en el redirect_uri de la aplicación web, enviar el código y el id_token a la aplicación de la web.  
  
3.  En este momento en que la aplicación web, que hayan recibido el código, inicia una solicitud para el extremo de token de AD FS, envío de la siguiente  
  
**Solicitud de token:**  
POST https://fs.contoso.com/adfs/oauth2/token
  
  
  
Parámetro|Valor  
---------|---------  
grant_type|"authorization_code"  
código|código de autorización anterior  
recursos|Punto de reunión ID (identificador) de Web API en el grupo de aplicaciones  
client_id|Identificador de cliente de la aplicación web (aplicación de servidor) en el grupo de aplicaciones  
redirect_uri|Redirigir el URI de aplicación web (aplicación de servidor) en el grupo de aplicaciones  
client_secret|Clave secreta de la aplicación web (aplicación de servidor) en el grupo de aplicaciones  
  
**Respuesta de la solicitud de token:**  
AD FS responde con un HTTP 200 con la access_token, refresh_token y id_token en el cuerpo.  
  
4.  La web, aplicaciones, a continuación, ya sea consume la parte access_token de la respuesta anterior (en el caso en que la propia aplicación web hospeda el recurso) o de lo contrario, envía como el encabezado de autorización en la solicitud HTTP a la API web.  
  
#### <a name="single-sign-on-behavior"></a>Comportamiento de inicio de sesión único  
El comportamiento de inicio de sesión único es el mismo que el flujo de cliente confidenciales de Oauth 2.0 anterior.  
  
### <a name="on-behalf-of"></a>En nombre de  
En este escenario, una aplicación web usa el token de acceso original de un usuario para solicitar y obtener el token de acceso de otra para otro Web API, que la aplicación web, a continuación, tendrá acceso a medida que el usuario final.  Esto se denomina un flujo "en nombre de".  
  
![Descripción del flujo de protocolo](media/ADFS_DEV_6.png)  
  
Los pasos 1 y 2 funcionan igual que los pasos 3 y 4 en el flujo anterior.  
En el paso 3, el requisito clave es que el parámetro client_id, el identificador de cliente de la aplicación Web 2, debe coincidir con la API de Id. de Web de punto de reunión A. En otras palabras, el público del token de acceso que se intercambian el nuevo token debe coincidir con el identificador de cliente de la entidad que solicita el token de nuevo.  

## <a name="related-content"></a>Contenido relacionado  
Consulta [AD FS desarrollo](../AD-FS-Development.md) para obtener una lista completa de los artículos del tutorial, que proporcionan instrucciones paso a paso sobre cómo usar los flujos relacionados. 
