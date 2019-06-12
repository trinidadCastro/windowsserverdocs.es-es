---
ms.assetid: 8a64545b-16bd-4c13-a664-cdf4c6ff6ea0
title: Escenarios de AD FS para desarrolladores
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb1bc5776ea4d24f274c79563d9e346b104de6d9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444220"
---
# <a name="ad-fs-scenarios-for-developers"></a>Escenarios de AD FS para desarrolladores


AD FS en Windows Server 2016 [AD FS 2016] permite agregar estándar OpenID Connect y OAuth 2.0 basados en autenticación y autorización a las aplicaciones que se está desarrollando la industria, y hacer que esas aplicaciones autenticar a los usuarios directamente en AD FS.    
  
AD FS 2016 también es compatible con WS-Federation, WS-Trust y SAML, protocolos y los perfiles se han admitido en las versiones anteriores.  Si está interesado en las instrucciones para desarrolladores para estos protocolos, consulte el artículo aquí.  En este artículo se centrará en cómo usar y beneficiarse de la compatibilidad con el protocolo más reciente.  
  
## <a name="why-modern-authentication"></a>¿Por qué la autenticación moderna  
Si bien puede continuar mediante AD FS para el inicio de sesión en con WS-Federation, WS-Trust y SAML protocolos al igual que podía tienen antes, con los protocolos más recientes, obtendrá las siguientes ventajas:  
  
* **Simplicidad y coherencia**  
    * Use el mismo conjunto de API y patrones para habilitar el inicio de sesión para:   
        *   varios tipos de aplicaciones (servidor, escritorio, móvil, explorador)  
        *   varias plataformas (android, iOS, Windows)  
        *   las aplicaciones dentro de la red corporativa u hospedado en la nube  
    * Usar el mismo conjunto de bibliotecas ya que puede usar para autenticar usuarios en Azure AD  
* **Flexibility**  
    * Autorización de usuario estándar, además de habilitar escenarios más complejos, como:      
        * ? 3 segmentos el inicio de sesión en los flujos en el que autoriza a un usuario de una aplicación web o servicio para acceder a recursos que residen con otra aplicación web o servicio.    
        * ? Flujos de servidor a servidor en el que un servicio de nivel intermedio, tiene acceso a un API de back-end  
        * ? Aplicaciones de página única (SPA) basadas en JavaScript  
* **Soporte técnico del sector**  
    * OAuth 2.0 y OpenID Connect disfrutan de amplia utilización en la industria, por lo que el conocimiento de estos patrones le ayudará a habilitar la autenticación y autorización fuera de un entorno de Active Directory también  
  
## <a name="how-it-works-the-basics"></a>Cómo funciona: Los conceptos básicos  
Puede agregar la autenticación moderna de AD FS a su aplicación con el mismo conjunto de herramientas y bibliotecas ya que puede usar para autenticar usuarios en Azure AD.   
  
En escenarios de AD FS, por supuesto, es AD FS y no Azure AD que actúa como proveedor de identidades y autorización de servidor.  En caso contrario, los conceptos son exactamente los mismos: los usuarios proporcionen sus credenciales y obtener tokens, ya sea directamente o a través de un intermediario, para tener acceso a los recursos.  
  
El escenario más básico consta de un usuario o el "propietario del recurso", interactuar con un explorador para tener acceso a una aplicación web:  
  
![AD FS para desarrolladores](media/ADFS_DEV_1.png)  
  
La aplicación web se denomina a un "cliente" porque inicia la solicitud al servidor de autorización (AD FS) para un token de acceso al recurso.  El recurso puede hospedarse en la propia aplicación web o esté accesible como una API web en alguna parte de la red o internet.   El usuario o el "propietario del recurso" autoriza a la aplicación web de cliente para recibir ese token de acceso al proporcionar credenciales al servidor de autorización.    
  
## <a name="how-it-works-components"></a>Cómo funciona: componentes  
OAuth 2.0 y OpenID Connect escenarios en la marca de AD FS usa el mismo conjunto de herramientas y bibliotecas que usar cuando Azure AD es el proveedor de identidades.  Estos componentes son:  
* Biblioteca de autenticación de Active Directory (ADAL): las bibliotecas de cliente que facilitan recopilar las credenciales de usuario, crear y enviar solicitudes de token y recuperar los tokens resultantes.    
* Middleware OWIN (Open Web Interface para. NET): Aunque OWIN es un proyecto en función de comunidad, Microsoft ha creado un conjunto de bibliotecas del lado servidor para la protección de aplicaciones web y API web con OpenID Connect y OAuth 2.0  
  
Las funciones de estos componentes se muestran en el diagrama siguiente:  
  
![AD FS para desarrolladores](media/ADFS_DEV_2.png)  
  
## <a name="modeling-these-scenarios-in-ad-fs-2016"></a>Estos escenarios de modelado en AD FS 2016  
  
### <a name="application-groups"></a>Grupos de aplicaciones  
Para representar estos escenarios en la directiva de AD FS, hemos introducido un nuevo concepto denominado grupos de aplicaciones.  Un grupo de aplicaciones puede contener cualquier número y la combinación de los siguientes tipos fundamentales de la aplicación:  
  
  
  
Grupo de aplicaciones / tipo de aplicación  |Descripción  |Rol    
---------|---------|---------  
Aplicación nativa     |  A veces se llama a un cliente público, esto se pretende ser una aplicación cliente que se ejecuta en un equipo o dispositivo y con la que el usuario interactúa.       | Solicitudes de tokens desde el servidor de autorización (AD FS) para el acceso de usuario a los recursos.  Envía solicitudes HTTP a los recursos protegidos, mediante los tokens como encabezados HTTP.        
Aplicación de servidor     |   Una aplicación web que se ejecuta en un servidor y suele ser accesible para los usuarios a través de un explorador.  Dado que es capaz de mantener su propio secreto de cliente' ' o una credencial, se denomina a veces, un cliente confidencial.      | Solicitudes de tokens desde el servidor de autorización (AD FS) para el acceso de usuario a los recursos.  Envía solicitudes HTTP a los recursos protegidos, mediante los tokens como encabezados HTTP.               
Web API     |  Tiene acceso los recursos de servidor al usuario. Piense en estos elementos como la nueva representación de "confianza".| Consume los tokens obtenidos por los clientes  
  
### <a name="differences-from-ad-fs-2012-r2"></a>Diferencias de AD FS 2012 R2  
Grupos de aplicaciones combinan elementos de confianza y la autorización que AD FS 2012 R2 expuestos por separado, como usuarios de confianza, los clientes y los permisos de la aplicación.  
  
Las tablas siguientes se comparan los métodos que los objetos de confianza de aplicación correspondiente se crean en AD FS 2012 R2 frente a AD FS 2016:  
  
AD FS en Windows Server 2012 R2|En PowerShell|Administración de AD FS  
---------|---------|---------  
Agregar a cliente nativo|Add-AdfsClient|N/A  
Agregar la aplicación de servidor como cliente|Add-AdfsClient|N/A  
Agregar Web API / recursos|Add-AdfsRelyingPartyTrust|Crear relación de confianza para usuario autenticado  
  
AD FS 2016|En PowerShell|Administración de AD FS  
---------|---------|---------  
Agregar a cliente nativo|Add-AdfsNativeClientApplication|Agregar grupo de aplicaciones nativa  
Agregar la aplicación de servidor como cliente|Add-AdfsServerApplication|Agregar grupo de aplicaciones de servidor  
Agregar Web API / recursos|Add-AdfsWebApiApplication|Agregar grupo de aplicaciones de API Web  
  
### <a name="application-permissions-and-consent"></a>Consentimiento y permisos de aplicación  
De forma predeterminada, los clientes de un grupo de aplicaciones pueden acceder a los recursos en el mismo grupo.  No tiene el administrador configurar los permisos de aplicación específica.  Grupos de aplicaciones también permiten a los administradores especificar los ámbitos permitidos, como openid o user_impersonation.  Las descripciones de escenarios a continuación especifican exactamente qué ámbitos son necesarios para el escenario.  
  
Dado que AD FS usa un modelo de consentimiento del administrador, los usuarios no solicitará consentimiento al tener acceso a los recursos.  Al configurar el grupo de aplicaciones, el administrador en vigor da su consentimiento en nombre de todos los usuarios de la aplicación.  
  
## <a name="supported-scenarios"></a>Escenarios admitidos  
La siguiente sección describe los escenarios que se admiten con más detalle.  
  
### <a name="tokens-used"></a>Tokens usados  
Asegúrese de estos escenarios de uso de los tres tipos de token:  
  
* **id_token:** Un token de JWT que se usa para representar la identidad del usuario. La notificación "aud" o audiencia del id_token coincide con el identificador de cliente de la aplicación nativa o servidor.  
* **access_token:** Usa un token de JWT de Oauth y OpenID conectan escenarios y está pensado para ser consumido por el recurso.  La notificación "aud" o audiencia del token debe coincidir con el identificador del recurso o API Web.  
* **refresh_token:** Este token se envía en lugar de recopilar las credenciales de usuario para proporcionar un inicio de sesión único en la experiencia.  Este token se emitió y consumido por AD FS y no es legible por los clientes o los recursos.    
  
### <a name="native-client-to-web-api"></a>Native client para API Web  
Este escenario permite al usuario de una aplicación cliente nativa para llamar a una API de Web de AD FS 2016 protegido.  
* La aplicación cliente nativa usa ADAL para enviar la autorización y token solicita a AD FS, solicitar las credenciales del usuario según sea necesario, a continuación, envía el token resultante como un encabezado HTTP en la solicitud a la API Web  
* [Esta parte es sólo con fines de demostración] La API web lee las notificaciones del objeto ClaimsPrincipal que origina el token de acceso enviado por el cliente y los envía al cliente.  
  
![Descripción del flujo de protocolo](media/ADFS_DEV_3.png)  
  
1.  La aplicación cliente nativa, inicia el flujo con una llamada a la biblioteca ADAL.  Esto desencadena un basada en el explorador autorizar un extremo HTTP GET para la instancia de AD FS:  
  
**Solicitud de autorización:**  
GET <https://fs.contoso.com/adfs/oauth2/authorize?>  
  
Parámetro|Valor  
---------|---------  
response_type|"code"  
resource|RP ID (identificador) de la API Web en el grupo de aplicaciones  
client_id|Id. de la aplicación nativa en el grupo de aplicaciones de cliente  
redirect_uri|URI de la aplicación nativa en el grupo de aplicaciones de redirección  
  
**Respuesta de solicitud de autorización:**  
Si el usuario no ha iniciado sesión antes, se solicitará al usuario las credenciales.    
AD FS responde devolviendo un código de autorización como el parámetro "code" en el componente de consulta de los redirect_uri.  Por ejemplo: HTTP/1.1 302 encontrado ubicación:  **<http://redirect_uri:80/?code=&lt;code&gt>;.**  
  
2. El cliente nativo, a continuación, envía el código, junto con los parámetros siguientes, en el extremo de token de AD FS:  
  
**Solicitud de token:**  
EXPONER https://fs.contoso.com/adfs/oauth2/token  
  
Parámetro|Valor  
---------|---------  
grant_type|"authorization_code" 
code|código de autorización de 1  
resource|RP ID (identificador) de la API Web en el grupo de aplicaciones  
client_id|Id. de la aplicación nativa en el grupo de aplicaciones de cliente  
redirect_uri|URI de la aplicación nativa en el grupo de aplicaciones de redirección  
  
**Respuesta de solicitud de token:**  
AD FS responde con HTTP 200 con el elemento access_token, refresh_token y id_token en el cuerpo.  
  
3. La aplicación nativa, a continuación, envía el elemento access_token de la respuesta anterior como el encabezado de autorización en la solicitud HTTP a la API web.  
  
### <a name="single-sign-on-behavior"></a>Comportamiento de inicio de sesión único  
Cliente posteriores solicitudes dentro de 1 hora (de forma predeterminada) el access_token seguirá siendo válido en la memoria caché y una nueva solicitud no activará ningún tráfico a AD FS.  El elemento access_token se capturarán automáticamente de la memoria caché por ADAL.  
  
Después de que expire el token de acceso, ADAL enviará automáticamente una solicitud basada en token de actualización para el extremo de token de AD FS (omitiendo la solicitud de autorización automáticamente).  
**Actualizar solicitud de token:**  
EXPONER https://fs.contoso.com/adfs/oauth2/token
   

Parámetro|Valor|
---------|---------
grant_type|"refresh_token"|
resource|RP ID (identificador) de la API Web en el grupo de aplicaciones|
client_id|Id. de la aplicación nativa en el grupo de aplicaciones de cliente
refresh_token|el token de actualización emitido por AD FS en respuesta a la solicitud de token inicial

  
  
**Respuesta de solicitud de token de actualización:**  
Si el token de actualización está dentro de < SSO_period >, la solicitud dará como resultado un nuevo token de acceso. No se solicita al usuario las credenciales.  Para obtener más información sobre la configuración, consulte SSO [AD FS en configuración de sesión único](../../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)  
  
Si el token de actualización ha expirado, la solicitud da como resultado un 401 de HTTP con error "invalid_grant" y "error_description" "MSIS9615: El token de actualización recibido en el parámetro refresh_token ha caducado". En este caso, ADAL envía automáticamente una nueva solicitud de autorización con la apariencia de #1 anterior.    
  
### <a name="web-browser-to-web-app"></a>Explorador Web a aplicación Web   
En este escenario, un usuario con un explorador debe tener acceso a recursos hospedados por una aplicación web.    
Hay dos escenarios que realizar esta acción.  
  
#### <a name="oauth-confidential-client"></a>Cliente confidencial de OAuth  
Este escenario es similar a la anterior en la que hay una solicitud de autorización, seguida por un código para el intercambio de token.  La aplicación web (que se modela como una aplicación de servidor de AD FS) inicia la solicitud de autorización a través del explorador e intercambia el código para el token (conectándose directamente a AD FS)  
  
![Descripción del flujo de protocolo](media/ADFS_DEV_4.png)  
  
1. Extremo de autorización de una autorización de solicitud a través del explorador, que envía una solicitud HTTP GET a la instancia de AD FS el inicia la aplicación Web  
   **Solicitud de autorización**:  
   GET <https://fs.contoso.com/adfs/oauth2/authorize?>  
  
Parámetro|Valor  
---------|---------  
response_type|"code"  
resource|RP ID (identificador) de la API Web en el grupo de aplicaciones  
client_id|Id. de la aplicación nativa en el grupo de aplicaciones de cliente  
redirect_uri|URI de web app (aplicación de servidor) en el grupo de aplicaciones de redirección  
  
Respuesta de solicitud de autorización:  
Si el usuario no ha iniciado sesión antes, se solicitará al usuario las credenciales.  
AD FS responde devolviendo un código de autorización, como el parámetro "code" en el componente de consulta de los redirect_uri, por ejemplo: HTTP/1.1 302 encontrado ubicación: <https://webapp.contoso.com/?code=&lt;code&gt>;.  
  
2. Como resultado la 302 anterior, el explorador inicia una solicitud HTTP GET a la aplicación web, por ejemplo: OBTENER <http://redirect_uri:80/?code=&lt;code&gt>;.   
  
3. En este momento en que la aplicación web, que ha recibido el código, inicia una solicitud para el extremo de token de AD FS y enviar el siguiente  
   **Solicitud de token:**  
   EXPONER https://fs.contoso.com/adfs/oauth2/token  
  
Parámetro|Valor  
---------|---------  
grant_type|"authorization_code"  
code|código de autorización de 2 anterior  
resource|RP ID (identificador) de la API Web en el grupo de aplicaciones  
client_id|Id. de cliente de la aplicación web (aplicación de servidor) en el grupo de aplicaciones  
redirect_uri|URI de web app (aplicación de servidor) en el grupo de aplicaciones de redirección  
client_secret|Secreto de la aplicación web (aplicación de servidor) en el grupo de aplicaciones. **Nota: Credencial del cliente no necesita ser un client_secret.  AD FS admite la capacidad de usar certificados o autenticación integrada de Windows también.**  
  
**Respuesta de solicitud de token:**  
AD FS responde con HTTP 200 con el elemento access_token, refresh_token y id_token en el cuerpo.  
notificaciones  
4. La web, aplicación, a continuación, ya sea consume el elemento access_token de la respuesta anterior (en el caso de que la propia aplicación web hospeda el recurso), o en caso contrario, envía como el encabezado de autorización en la solicitud HTTP a la API web.  
  
#### <a name="single-sign-on-behavior"></a>Comportamiento de inicio de sesión único  
Mientras el token de acceso seguirán siendo válido durante 1 hora (de forma predeterminada) en la caché del cliente, es posible que piense que la segunda solicitud funcionará como se muestra en el escenario de cliente nativo anterior - que una solicitud nueva desencadenará ningún tráfico a AD FS como configurará automáticamente y el token de acceso recuperarse de la memoria caché por ADAL.  Sin embargo, es posible que la aplicación web puede enviar distinto autorización y las solicitudes de token, el primero mediante un vínculo de dirección URL distinto, como en nuestro ejemplo.  
  
En este caso, es la cookie de inicio de sesión único de AD FS explorador que permite que AD FS emitir un nuevo código de autorización sin preguntar al usuario las credenciales. A continuación, llama a la aplicación web para AD FS para el nuevo código de autorización para un nuevo token de acceso de exchange.  No se solicita al usuario las credenciales.  
  
En caso contrario, si la aplicación web está smart basta con saber que si el usuario ya está autenticado, se puede omitir la solicitud authorize y ya sea:  
* el token de acceso almacenada en caché, si no ha expirado, se recupera y utiliza, o   
* se puede enviar una solicitud basada en token de solicitud al punto de conexión de token de AD FS, como se describe a continuación  
  
**Actualizar solicitud de token:**  
EXPONER https://fs.contoso.com/adfs/oauth2/token
   
Parámetro|Valor  
---------|---------  
grant_type|"refresh_token"  
resource|RP ID (identificador) de la API Web en el grupo de aplicaciones  
client_id|Id. de cliente de la aplicación web (aplicación de servidor) en el grupo de aplicaciones  
refresh_token|Actualizar el token emitido por AD FS en respuesta a la solicitud de token inicial  
client_secret|Secreto de la aplicación web (aplicación de servidor) en el grupo de aplicaciones  
  
**Respuesta de solicitud de token de actualización:**  
Si el token de actualización está dentro de < SSO_period >, la solicitud dará como resultado un nuevo token de acceso. No se solicita al usuario las credenciales. Para obtener más información sobre la configuración, consulte SSO [AD FS en configuración de sesión único](../../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)   
  
Si el token de actualización ha expirado, la solicitud da como resultado un 401 de HTTP con error "invalid_grant" y "error_description" "MSIS9615: El token de actualización recibido en el parámetro refresh_token ha caducado". En este caso, ADAL envía automáticamente una nueva solicitud de autorización con la apariencia de #1 anterior.    
  
#### <a name="openid-connect-hybrid-flow"></a>OpenID Connect: Flujo híbrido  
Este escenario es similar a la anterior en el existe una solicitud de autorización iniciada por la aplicación web mediante el redireccionamiento del explorador y el código para el intercambio de token de la aplicación web a AD FS.  La diferencia en este escenario es que ADFS emite un id_token como parte de la respuesta de solicitud de autorización inicial.  
  
![Descripción del flujo de protocolo](media/ADFS_DEV_5.png)  
  
1.  Extremo de autorización de una autorización de solicitud a través del explorador, que envía una solicitud HTTP GET a la instancia de AD FS el inicia la aplicación Web  
  
**Solicitud de autorización:**  
GET <https://fs.contoso.com/adfs/oauth2/authorize?>  
  
Parámetro|Valor  
---------|---------  
response_type|"code+id_token"  
response_mode|"form_post"  
resource|RP ID (identificador) de la API Web en el grupo de aplicaciones  
client_id|Id. de cliente de la aplicación web (aplicación de servidor) en el grupo de aplicaciones  
redirect_uri|URI de web app (aplicación de servidor) en el grupo de aplicaciones de redirección  
  
**Respuesta de solicitud de autorización:**  
Si el usuario no ha iniciado sesión antes, se solicitará al usuario las credenciales.  
AD FS responde con un HTTP 200 y el formulario que contiene la siguiente como oculta los elementos:  
* código: el código de autorización  
* ID_token: un token de JWT que contienen notificaciones que describe la autenticación de usuario  
* El formulario se envía automáticamente a los redirect_uri de la aplicación web, envía el código y el id_token a la aplicación web.  
  
3. En este momento en que la aplicación web, que ha recibido el código, inicia una solicitud para el extremo de token de AD FS y enviar el siguiente  
  
**Solicitud de token:**  
EXPONER https://fs.contoso.com/adfs/oauth2/token
  
  
  
Parámetro|Valor  
---------|---------  
grant_type|"authorization_code"  
code|código de autorización desde arriba  
resource|RP ID (identificador) de la API Web en el grupo de aplicaciones  
client_id|Id. de cliente de la aplicación web (aplicación de servidor) en el grupo de aplicaciones  
redirect_uri|URI de web app (aplicación de servidor) en el grupo de aplicaciones de redirección  
client_secret|Secreto de la aplicación web (aplicación de servidor) en el grupo de aplicaciones  
  
**Respuesta de solicitud de token:**  
AD FS responde con HTTP 200 con el elemento access_token, refresh_token y id_token en el cuerpo.  
  
4. La web, aplicación, a continuación, ya sea consume el elemento access_token de la respuesta anterior (en el caso de que la propia aplicación web hospeda el recurso), o en caso contrario, envía como el encabezado de autorización en la solicitud HTTP a la API web.  
  
#### <a name="single-sign-on-behavior"></a>Comportamiento de inicio de sesión único  
El comportamiento de inicio de sesión único es el mismo que el flujo de cliente confidencial de Oauth 2.0 anterior.  
  
### <a name="on-behalf-of"></a>En nombre de  
En este escenario, una aplicación web usa el token de acceso original de un usuario para solicitar y obtener otro token de acceso para otra API Web, que la aplicación web, a continuación, tendrá acceso a que el usuario final.  Esto se denomina un flujo "en nombre de".  
  
![Descripción del flujo de protocolo](media/ADFS_DEV_6.png)  
  
Los pasos 1 y 2 funcionan igual que los pasos 3 y 4 en el flujo anterior.  
En el paso 3, el requisito clave es que el parámetro client_id, el identificador de cliente de la aplicación Web 2, debe coincidir con la API de Id. de Web de RP A.  En otras palabras, la audiencia del token de acceso que se intercambian para el nuevo token debe coincidir con el identificador de cliente de la entidad que solicita el nuevo token.  

## <a name="related-content"></a>Contenidos relacionados  
Consulte [AD FS desarrollo](../AD-FS-Development.md) para obtener la lista completa de artículos de tutoriales, que proporcionan instrucciones paso a paso sobre el uso de los flujos relacionados. 
