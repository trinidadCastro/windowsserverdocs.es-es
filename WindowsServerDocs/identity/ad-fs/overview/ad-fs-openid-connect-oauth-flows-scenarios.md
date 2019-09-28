---
ms.assetid: 8a64545b-16bd-4c13-a664-cdf4c6ff6ea0
title: AD FS los flujos de OpenID Connect/OAuth y los escenarios de aplicación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e1e0235e50945fadd09fe9dd5ffeaf6d7119e482
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385605"
---
# <a name="ad-fs-openid-connectoauth-flows-and-application-scenarios"></a>AD FS los flujos de OpenID Connect/OAuth y los escenarios de aplicación
Se aplica a AD FS 2016 y versiones posteriores


|Escenario|Tutorial de escenario con ejemplos|Flujo/concesión de OAuth 2,0|Tipo de cliente|
|-----|-----|-----|-----|
|Aplicación de una sola página</br> | &bull;[Ejemplo de uso de Adal](../development/Single-Page-Application-with-AD-FS.md)|[Explícito](#implicit-grant-flow)|Public| 
|Aplicación web que inicia la sesión de los usuarios</br> | &bull;[Ejemplo con OWIN](../development/enabling-openid-connect-with-ad-fs.md)|[Código de autorización](#authorization-code-grant-flow)|Público, confidencial|  
|La aplicación nativa llama a la API Web</br>|&bull;[Ejemplo con MSAL](../development/msal/adfs-msal-native-app-web-api.md)</br>&bull;[Ejemplo de uso de Adal](../development/native-client-with-ad-fs.md)|[Código de autorización](#authorization-code-grant-flow)|Public|   
|La aplicación web llama a la API Web</br>|&bull;[Ejemplo con MSAL](../development/msal/adfs-msal-web-app-web-api.md)</br>&bull;[Ejemplo de uso de Adal](../development/enabling-oauth-confidential-clients-with-ad-fs.md)|[Código de autorización](#authorization-code-grant-flow)|Material| 
|La API Web llama a otra API Web en nombre del usuario (OBO)</br>|&bull;[Ejemplo con MSAL](../development/msal/adfs-msal-web-api-web-api.md)</br>&bull;[Ejemplo de uso de Adal](../development/ad-fs-on-behalf-of-authentication-in-windows-server.md)|[En nombre de](#on-behalf-of-flow)|La aplicación web actúa como confidencial| 
|La aplicación de demonio llama a la API Web||[Credenciales de cliente](#client-credentials-grant-flow)|Material| 
|La aplicación web llama a la API Web mediante credenciales de usuario||[Credenciales de contraseña del propietario del recurso](#resource-owner-password-credentials-grant-flow-not-recommended)|Público, confidencial| 
|API Web llama a la aplicación no explorador||[Código de dispositivo](#device-code-flow)|Público, confidencial| 

## <a name="implicit-grant-flow"></a>Flujo de concesión implícita 
 
En el caso de las aplicaciones de una sola página (AngularJS, Ember. js, reAct. js, etc.), AD FS admite el flujo de concesión implícita OAuth 2,0. El flujo implícito se describe en la [especificación OAuth 2,0](https://tools.ietf.org/html/rfc6749#section-4.2). Su principal ventaja es que permite que la aplicación obtenga tokens de AD FS sin realizar un intercambio de credenciales del servidor back-end. Esto permite a la aplicación iniciar sesión del usuario, mantener la sesión y obtener tokens para otras API Web en el código JavaScript del cliente. Hay algunas consideraciones de seguridad importantes que se deben tener en cuenta al usar el flujo implícito específicamente alrededor del [cliente](https://tools.ietf.org/html/rfc6749#section-10.3).  
 
Si quiere usar el flujo implícito y AD FS para agregar autenticación a la aplicación de JavaScript, siga los pasos generales que se indican a continuación.  
  
### <a name="protocol-diagram"></a>Diagrama del Protocolo

En el diagrama siguiente se muestra el aspecto del flujo de inicio de sesión implícito completo y en las secciones siguientes se describe cada paso con más detalle.  

![Inicio de sesión implícito](media/adfs-scenarios-for-developers/implicit.png)

### <a name="request-id-token-and-access-token"></a>Token de identificador de solicitud y token de acceso 
 
Para iniciar la sesión inicial del usuario en la aplicación, puede enviar una solicitud de autenticación de OpenID Connect y obtener ID_token y token de acceso desde el punto de conexión de AD FS.  
 
```
// Line breaks for legibility only 
 
https://adfs.contoso.com/adfs/oauth2/authorize? 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&response_type=id_token+token 
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F 
&scope=openid 
&response_mode=fragment 
&state=12345 
```


|Parámetro|Obligatorio/opcional|Descripción| 
|-----|-----|-----|
|client_id|necesarias|El identificador de la aplicación (cliente) que el AD FS asignado a la aplicación.| 
|response_type|necesarias|Debe incluir `id_token` para el inicio de sesión en OpenID Connect. También puede incluir response_type @ no__t-0. El uso de token aquí permitirá a la aplicación recibir un token de acceso inmediatamente desde el punto de conexión autorizado sin tener que realizar una segunda solicitud al punto de conexión del token.| 
|redirect_uri|necesarias|El redirect_uri de la aplicación, en el que la aplicación puede enviar y recibir las respuestas de autenticación. Debe coincidir exactamente con uno de los redirect_uris que configuró en AD FS.| 
|nonce|necesarias|Un valor incluido en la solicitud, generado por la aplicación, que se incluirá en el ID_token resultante como una demanda. A continuación, la aplicación puede comprobar este valor para mitigar los ataques de reproducción de tokens. Normalmente, el valor es una cadena única aleatoria que se puede usar para identificar el origen de la solicitud. Solo es necesario cuando se solicita un ID_token.|
|scope|opcional|Lista de ámbitos separados por espacios. Para OpenID Connect, debe incluir el ámbito `openid`.|
|resource|opcional|La dirección URL de la API Web.</br>Nota: si se usa la biblioteca de cliente de MSAL, no se envía el parámetro de recurso. En su lugar, la dirección URL del recurso se envía como parte del parámetro de ámbito:`scope = [resource url]//[scope values e.g., openid]`</br>Si el recurso no se pasa aquí o en el ámbito, ADFS usará un recurso predeterminado urn: Microsoft: UserInfo. las directivas de recursos UserInfo, como MFA, emisión o Directiva de autorización, no se pueden personalizar.| 
|response_mode|opcional| Especifica el método que debe usarse para enviar el token resultante de nuevo a la aplicación. Tiene como valor predeterminado `fragment`.| 
|State|opcional|Un valor incluido en la solicitud que también se devolverá en la respuesta del token. Puede ser una cadena de cualquier contenido que desee. Normalmente, se usa un valor único generado de forma aleatoria para evitar ataques de falsificación de solicitudes entre sitios. El estado también se usa para codificar información sobre el estado del usuario en la aplicación antes de que se produjera la solicitud de autenticación, como la página o la vista en la que estaban.| 
|prompt|opcional|Indica el tipo de interacción del usuario que se requiere. Los únicos valores válidos en este momento son login y none.</br>- `prompt=login` obligará al usuario a escribir sus credenciales en esa solicitud, negando el inicio de sesión único. </br>- `prompt=none` es lo contrario, se asegurará de que el usuario no presente ninguna solicitud interactiva en cualquier momento. Si la solicitud no se puede completar silenciosamente a través del inicio de sesión único, AD FS devolverá un error interaction_required.| 
|login_hint|opcional|Se puede usar para rellenar previamente el campo de nombre de usuario y dirección de correo electrónico de la página de inicio de sesión del usuario, si conoce el nombre de usuario con antelación. A menudo, las aplicaciones usarán este parámetro durante la reautenticación, ya que ya han extraído el nombre de usuario `upn`de un `id_token`inicio de sesión anterior mediante la declaración de.| 
|domain_hint|opcional|Si se incluye, omitirá el proceso de detección basado en dominio que pasa el usuario en la página de inicio de sesión, lo que conduce a una experiencia de usuario ligeramente más sencilla.| 

En este momento, se le pedirá al usuario que escriba sus credenciales y que complete la autenticación. Una vez que el usuario se autentica, el AD FS autorizar punto de conexión devolverá una respuesta a la aplicación en el parámetro redirect_uri indicado, mediante el método especificado en el parámetro response_mode.  
 
### <a name="successful-response"></a>Respuesta correcta 
 
Una respuesta correcta con `response_mode=fragment and response_type=id_token+token` es similar a la siguiente:  
 
```
// Line breaks for legibility only 
 
GET https://localhost/myapp/# 
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZEstZnl0aEV... 
&token_type=Bearer 
&expires_in=3599 
&scope=openid  
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZstZnl0aEV1Q... 
&state=12345 
```


|Parámetro|Descripción| 
|-----|-----|
|access_token|Se incluye si response_type incluye @ no__t-0.|
|token_type|Se incluye si response_type incluye @ no__t-0. Siempre será Bearer.| 
|expires_in| Se incluye si response_type incluye @ no__t-0. Indica el número de segundos que el token es válido, para el almacenamiento en caché.| 
|scope| Indica los ámbitos para los que será válido el access_token.|  
|id_token|Se incluye si response_type incluye @ no__t-0. Un JSON Web Token firmado (JWT). La aplicación puede descodificar los segmentos de este token para solicitar información sobre el usuario que ha iniciado sesión. La aplicación puede almacenar en caché los valores y mostrarlos, pero no debe depender de los límites de autorización o seguridad.| 
|State|Si se incluye un parámetro de estado en la solicitud, debe aparecer el mismo valor en la respuesta. La aplicación debe comprobar que los valores de estado de la solicitud y la respuesta son idénticos.|

### <a name="refresh-tokens"></a>Tokens de actualización 
La concesión implícita no proporciona tokens de actualización.  `id_tokens` Y `access_tokens` expirarán después de un breve período de tiempo, por lo que la aplicación debe estar preparada para actualizar estos tokens periódicamente. Para actualizar cualquier tipo de token, puede realizar la misma solicitud de iframe oculta anterior mediante el `prompt=none` parámetro para controlar el comportamiento de la plataforma de identidad. Si desea recibir `new id_token`, asegúrese de usar `response_type=id_token`. 

## <a name="authorization-code-grant-flow"></a>Flujo de concesión de código de autorización 
 
La concesión de código de autorización de OAuth 2,0 puede usarse en aplicaciones web para obtener acceso a recursos protegidos, como las API Web. El flujo de código de autorización de OAuth 2,0 se describe en [la sección 4,1 de la especificación de oauth 2,0](https://tools.ietf.org/html/rfc6749). Se usa para realizar la autenticación y autorización en la mayoría de los tipos de aplicaciones, incluidas las aplicaciones web y las aplicaciones instaladas de forma nativa. El flujo permite a las aplicaciones adquirir de forma segura access_tokens que se pueden usar para tener acceso a los recursos que confían AD FS.  
 
### <a name="protocol-diagram"></a>Diagrama del Protocolo 
 
En un nivel alto, el flujo de autenticación para una aplicación nativa tiene un aspecto similar al siguiente:

![Flujo de concesión de código de autorización](media/adfs-scenarios-for-developers/authorization.png)

### <a name="request-an-authorization-code"></a>Solicitar un código de autorización 
 
El flujo de código de autorización comienza con el cliente que dirige al usuario al extremo/Authorize. En esta solicitud, el cliente indica los permisos que necesita adquirir del usuario: 
 
```
// Line breaks for legibility only 
 
https://adfs.contoso.com/adfs/oauth2/authorize? 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&response_type=code 
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F 
&response_mode=query 
&resource=https://webapi.com/ 
&scope=openid 
&state=12345 
```

|Parámetro|Obligatorio/opcional|Descripción|
|-----|-----|-----| 
|client_id|necesarias|El identificador de la aplicación (cliente) que el AD FS asignado a la aplicación.|  
|response_type|necesarias| Debe incluir código para el flujo de código de autorización.| 
|redirect_uri|necesarias|`redirect_uri` De la aplicación, donde se pueden enviar y recibir las respuestas de autenticación de la aplicación. Debe coincidir exactamente con uno de los redirect_uris que registró en el AD FS del cliente.|  
|resource|opcional|La dirección URL de la API Web.</br>Nota: si se usa la biblioteca de cliente de MSAL, no se envía el parámetro de recurso. En su lugar, la dirección URL del recurso se envía como parte del parámetro de ámbito:`scope = [resource url]//[scope values e.g., openid]`</br>Si el recurso no se pasa aquí o en el ámbito, ADFS usará un recurso predeterminado urn: Microsoft: UserInfo. las directivas de recursos UserInfo, como MFA, emisión o Directiva de autorización, no se pueden personalizar.| 
|scope|opcional|Lista de ámbitos separados por espacios.|
|response_mode|opcional|Especifica el método que debe usarse para enviar el token resultante de nuevo a la aplicación. Puede ser uno de los siguientes: </br>-consulta </br>-Fragment </br>- form_post</br>`query` proporciona el código como un parámetro de cadena de consulta en el URI de redirección. Si está solicitando el código, puede usar Query, Fragment o form_post.  `form_post` @ no__t-1executes una publicación que contenga el código en el URI de redirección.|
|State|opcional|Un valor incluido en la solicitud que también se devolverá en la respuesta del token. Puede ser una cadena de cualquier contenido que desee. Normalmente, se usa un valor único generado de forma aleatoria para evitar ataques de falsificación de solicitudes entre sitios. El valor también puede codificar información sobre el estado del usuario en la aplicación antes de que se produjera la solicitud de autenticación, como la página o la vista en la que estaban.|
|prompt|opcional|Indica el tipo de interacción del usuario que se requiere. Los únicos valores válidos en este momento son login y none.</br>- `prompt=login` obligará al usuario a escribir sus credenciales en esa solicitud, negando el inicio de sesión único. </br>- `prompt=none` es lo contrario, se asegurará de que el usuario no presente ninguna solicitud interactiva en cualquier momento. Si la solicitud no se puede completar silenciosamente a través del inicio de sesión único, AD FS devolverá un error interaction_required.|
|login_hint|opcional|Se puede usar para rellenar previamente el campo de nombre de usuario y dirección de correo electrónico de la página de inicio de sesión del usuario, si conoce el nombre de usuario con antelación. A menudo, las aplicaciones usarán este parámetro durante la reautenticación, ya que ya han extraído el nombre de usuario `upn`de un `id_token`inicio de sesión anterior mediante la declaración de.|
|domain_hint|opcional|Si se incluye, omitirá el proceso de detección basado en dominio que pasa el usuario en la página de inicio de sesión, lo que conduce a una experiencia de usuario ligeramente más sencilla.|
|code_challenge_method|opcional|El método que se usa para codificar code_verifier para el parámetro code_challenge. Puede presentar uno de los siguientes valores: </br>-Plain </br>- S256 </br>Si se excluye, se supone que code_challenge es texto no cifrado si se incluye @ no__t-0 @ no__t-1is. AD FS admite tanto Plain como S256. Para obtener más información, consulte el [documento RFC de PKCE](https://tools.ietf.org/html/rfc7636).|
|code_challenge|opcional| Se usa para proteger las concesiones de código de autorización a través de la clave de prueba para el intercambio de código (PKCE) desde un cliente nativo. Obligatorio si `code_challenge_method` se incluye. Para obtener más información, consulte el [documento RFC de PKCE](https://tools.ietf.org/html/rfc7636)|

En este momento, se le pedirá al usuario que escriba sus credenciales y que complete la autenticación. Una vez que el usuario se autentica, el AD FS devolverá una respuesta a la aplicación `redirect_uri`en el indicado, mediante el método `response_mode`especificado en el parámetro.  
 
### <a name="successful-response"></a>Respuesta correcta 
 
Una respuesta correcta mediante response_mode = Query tiene el siguiente aspecto: 
 
```
GET https://adfs.contoso.com/common/oauth2/nativeclient? 
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq... 
&state=12345  
```


|Parámetro|Descripción|
|-----|-----|
|code|`authorization_code` Que solicitó la aplicación. La aplicación puede usar el código de autorización para solicitar un token de acceso para el recurso de destino. Authorization_codes son de corta duración, normalmente expiran después de unos 10 minutos.|
|State|Si un `state` parámetro se incluye en la solicitud, debe aparecer el mismo valor en la respuesta. La aplicación debe comprobar que los valores de estado de la solicitud y la respuesta son idénticos.|

### <a name="request-an-access-token"></a>Solicitar un token de acceso 
 
Ahora que ha adquirido un `authorization_code` y que el usuario le ha concedido permiso, puede canjear el código `access_token` para el recurso deseado. Para ello, envíe una solicitud POST al punto de conexión/token:  
 
```
// Line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com/ 
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr... 
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F 
&grant_type=authorization_code 
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for confidential clients (web apps)  
```

|Parámetro|Obligatorio/opcional|Descripción|
|-----|-----|-----| 
|client_id|necesarias|El identificador de la aplicación (cliente) que el AD FS asignado a la aplicación.| 
|grant_type|necesarias|Debe ser `authorization_code` para el flujo de código de autorización.| 
|code|necesarias|`authorization_code` Que adquirió en el primer segmento del flujo.| 
|redirect_uri|necesarias|El mismo `redirect_uri` valor que se usó para `authorization_code`adquirir.| 
|client_secret|necesario para Web Apps|El secreto de aplicación que creó durante el registro de la aplicación en AD FS. No debe usar el secreto de aplicación en una aplicación nativa porque client_secrets no se puede almacenar de forma confiable en los dispositivos. Es necesario para aplicaciones web y API Web, que tienen la capacidad de almacenar client_secret de forma segura en el lado servidor. El secreto de cliente debe estar codificado como URL antes de enviarse. Estas aplicaciones también pueden usar una autenticación basada en claves al firmar un JWT y agregarlo como parámetro client_assertion.| 
|code_verifier|opcional|El mismo `code_verifier` que se usó para obtener authorization_code. Obligatorio si se usó PKCE en la solicitud de concesión de código de autorización. Para obtener más información, consulte el [documento RFC de PKCE](https://tools.ietf.org/html/rfc7636).</br>Nota: se aplica a AD FS 2019 y versiones posteriores.| 

### <a name="successful-response"></a>Respuesta correcta 
 
Una respuesta de token correcta tendrá el siguiente aspecto: 
 
```
{ 
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...", 
    "token_type": "Bearer", 
    "expires_in": 3599, 
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...", 
    "refresh_token_expires_in": 28800, 
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...", 
} 
```


|Parámetro|Descripción| 
|-----|-----|
|access_token|El token de acceso solicitado. La aplicación puede usar este token para autenticarse en el recurso protegido (API Web).| 
|token_type|Indica el valor de tipo de token. El único tipo que admite AD FS es Bearer.
|expires_in|Cuánto tiempo es válido el token de acceso (en segundos).
|refresh_token|Un token de actualización de OAuth 2,0. La aplicación puede usar este token para adquirir tokens de acceso adicionales una vez que expire el token de acceso actual. Refresh_tokens son de larga duración y se pueden usar para conservar el acceso a los recursos durante largos períodos de tiempo.| 
|refresh_token_expires_in|Cuánto tiempo es válido el token de actualización (en segundos).| 
|id_token|Un JSON Web Token (JWT). La aplicación puede descodificar los segmentos de este token para solicitar información sobre el usuario que ha iniciado sesión. La aplicación puede almacenar en caché los valores y mostrarlos, pero no debe confiar en ellos para ningún límite de autorización o seguridad.|

### <a name="use-the-access-token"></a>Usar el token de acceso 
 
```
GET /v1.0/me/messages 
Host: https://webapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q... 
 ```

### <a name="refresh-the-access-token"></a>Actualizar el token de acceso 
 
Access_tokens son de corta duración y debe actualizarlas después de que expiren para seguir teniendo acceso a los recursos. Puede hacerlo mediante el envío de otra solicitud POST a @ no__t-0 @ no__t-1endpoint, y esta vez proporcionando el refresh_token en lugar del código. Los tokens de actualización son válidos para todos los permisos para los que el cliente ya ha recibido el token de acceso. 
 
Los tokens de actualización no tienen una duración especificada. Normalmente, las duraciones de los tokens de actualización son relativamente largas. Sin embargo, en algunos casos, los tokens de actualización caducan, se revocan o carecen de privilegios suficientes para la acción deseada. La aplicación necesita esperar y controlar correctamente los errores devueltos por el extremo de emisión de tokens.  
 
Aunque los tokens de actualización no se revocan cuando se usan para adquirir nuevos tokens de acceso, se espera que descarte el token de actualización anterior. La especificación OAuth 2,0 indica: "El servidor de autorización puede emitir un nuevo token de actualización, en cuyo caso el cliente debe descartar el token de actualización anterior y reemplazarlo por el nuevo token de actualización. El servidor de autorización puede revocar el token de actualización anterior después de emitir un nuevo token de actualización al cliente ". 
 
```
// Line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq... 
&grant_type=refresh_token 
&client_secret=JqQX2PNo9bpM0uEihUPzyrh      // NOTE: Only required for confidential clients (web apps)  
```


|Parámetro|Obligatorio/opcional|Descripción| 
|-----|-----|-----|
|client_id|necesarias|El identificador de la aplicación (cliente) que el AD FS asignado a la aplicación.| 
|grant_type|necesarias|Debe ser `refresh_token` para este segmento del flujo de código de autorización.| 
|resource|opcional|La dirección URL de la API Web.</br>Nota: si se usa la biblioteca de cliente de MSAL, no se envía el parámetro de recurso. En su lugar, la dirección URL del recurso se envía como parte del parámetro de ámbito:`scope = [resource url]//[scope values e.g., openid]`</br>Si el recurso no se pasa aquí o en el ámbito, ADFS usará un recurso predeterminado urn: Microsoft: UserInfo. las directivas de recursos UserInfo, como MFA, emisión o Directiva de autorización, no se pueden personalizar.|
|scope|opcional|Lista de ámbitos separados por espacios.| 
|refresh_token|necesarias|Refresh_token adquirido en el segundo segmento del flujo.| 
|client_secret|necesario para Web Apps| El secreto de aplicación que creó en el portal de registro de aplicaciones para la aplicación. No debe usarse en una aplicación nativa, ya que client_secrets no se puede almacenar de forma confiable en los dispositivos. Es necesario para aplicaciones web y API Web, que tienen la capacidad de almacenar client_secret de forma segura en el lado servidor. Estas aplicaciones también pueden usar una autenticación basada en claves al firmar un JWT y agregarlo como parámetro client_assertion.|

### <a name="successful-response"></a>Respuesta correcta 
Una respuesta de token correcta tendrá el siguiente aspecto: 
 
```
{ 
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...", 
    "token_type": "Bearer", 
    "expires_in": 3599, 
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...", 
    "refresh_token_expires_in": 28800, 
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...", 
}  
```
|Parámetro|Descripción| 
|-----|-----|
|access_token|El token de acceso solicitado. La aplicación puede usar este token para autenticarse en el recurso protegido, como una API Web.| 
|token_type|Indica el valor de tipo de token. El único tipo que admite AD FS es Bearer|
|expires_in|Cuánto tiempo es válido el token de acceso (en segundos).|
|scope|Los ámbitos para los que es válido el access_token.| 
|refresh_token|Un token de actualización de OAuth 2,0. La aplicación puede usar este token para adquirir tokens de acceso adicionales una vez que expire el token de acceso actual. Refresh_tokens son de larga duración y se pueden usar para conservar el acceso a los recursos durante largos períodos de tiempo.| 
|refresh_token_expires_in|Cuánto tiempo es válido el token de actualización (en segundos).| 
|id_token|Un JSON Web Token (JWT). La aplicación puede descodificar los segmentos de este token para solicitar información sobre el usuario que ha iniciado sesión. La aplicación puede almacenar en caché los valores y mostrarlos, pero no debe confiar en ellos para ningún límite de autorización o seguridad.|

## <a name="on-behalf-of-flow"></a>Flujo en nombre de 
 
El flujo en nombre de OAuth 2,0 (OBO) sirve para el caso de uso en el que una aplicación invoca una API de servicio o Web, que a su vez necesita llamar a otra API de servicio o Web. La idea es propagar la identidad del usuario delegado y los permisos a través de la cadena de solicitud. Para que el servicio de nivel intermedio realice solicitudes autenticadas en el servicio de bajada, debe proteger un token de acceso del AD FS, en nombre del usuario.  
 
### <a name="protocol-diagram"></a>Diagrama del Protocolo 
Supongamos que el usuario se ha autenticado en una aplicación mediante el flujo de concesión de código de autorización OAuth 2,0 descrito anteriormente. En este punto, la aplicación tiene un token de acceso para la API A (token A) con las notificaciones del usuario y el consentimiento para acceder a la API Web de nivel intermedio (API A). Asegúrese de que el cliente solicita el ámbito de user_impersonation en el token. Ahora, la API a debe realizar una solicitud autenticada a la API Web de bajada (API B). 

Los pasos siguientes constituyen el flujo OBO y se explican con la ayuda del diagrama siguiente. 

![Flujo en nombre de](media/adfs-scenarios-for-developers/obo.png)

  1. La aplicación cliente realiza una solicitud a la API a con el token A.  
  Nota: Mientras configura el flujo de OBO en AD FS Asegúrese `user_impersonation` de que el ámbito está seleccionado `user_impersonation` y el ámbito de solicitud de cliente en la solicitud. 
  2. La API a se autentica en el extremo de emisión de tokens de AD FS y solicita un token para acceder a la API B. Nota: Mientras configura este flujo en AD FS Asegúrese de que la API A también se registra como una aplicación de servidor con clientID que tiene el mismo valor que el identificador de recurso en la API A. Para obtener más detalles, consulte el ejemplo "en nombre de" en Agregar vínculo.  
  3. El extremo de emisión de tokens de AD FS valida las credenciales de la API a con el token A y emite el token de acceso para la API B (token B). 
  4. El token B se establece en el encabezado de autorización de la solicitud a la API B. 
  5. La API B devuelve los datos del recurso protegido. 

### <a name="service-to-service-access-token-request"></a>Solicitud de token de acceso de servicio a servicio 
 
Para solicitar un token de acceso, realice una solicitud HTTP POST al punto de conexión del token de AD FS con los parámetros siguientes.  


### <a name="first-case-access-token-request-with-a-shared-secret"></a>Primer caso: Solicitud de token de acceso con un secreto compartido 
 
Cuando se usa un secreto compartido, una solicitud de token de acceso de servicio a servicio contiene los parámetros siguientes: 


|Parámetro|Obligatorio/opcional|Descripción|
|-----|-----|-----| 
|grant_type|necesarias|Tipo de solicitud de token. Para una solicitud con un JWT, el valor debe ser urn: IETF: params: OAuth: Grant-Type: JWT-Bearer.|  
|client_id|necesarias|El identificador de cliente que se configura al registrar la primera API Web como una aplicación de servidor (aplicación de nivel intermedio). Debe ser el mismo que el identificador de recurso usado en el primer segmento, es decir, la dirección URL de la primera API Web.| 
|client_secret|necesarias|El secreto de aplicación que creó durante el registro de la aplicación de servidor en AD FS.| 
|assertion|necesarias|Valor del token usado en la solicitud.|  
|requested_token_use|necesarias|Especifica cómo se debe procesar la solicitud. En el flujo OBO, el valor debe establecerse en on_behalf_of| 
|resource|necesarias|El identificador de recurso proporcionado al registrar la primera API Web como la aplicación de servidor (aplicación de nivel intermedio). El identificador de recurso debe ser la dirección URL de la segunda aplicación de nivel intermedio de API Web llamará en nombre del cliente.|
|scope|opcional|Una lista separada por espacios de ámbitos para la solicitud de token.| 

#### <a name="example"></a>Ejemplo 
 
Lo siguiente `HTTP POST` solicita un token de acceso y un token de actualización 
 
```
//line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: adfs.contoso.com  
Content-Type: application/x-www-form-urlencoded 
 
grant_type=urn:ietf:params:oauth:grant-type:jwt-bearer 
&client_id=https://webapi.com/ 
&client_secret=BYyVnAt56JpLwUcyo47XODd 
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIm… 
&resource=https://secondwebapi.com/
&requested_token_use=on_behalf_of
&scope=openid    
```

### <a name="second-case-access-token-request-with-a-certificate"></a>Segundo caso: Solicitud de token de acceso con un certificado 
 
Una solicitud de token de acceso de servicio a servicio con un certificado contiene los parámetros siguientes: 


|Parámetro|requerido/opcional|Descripción|
|-----|-----|-----| 
|grant_type|necesarias|Tipo de solicitud de token. Para una solicitud con un JWT, el valor debe ser urn: IETF: params: OAuth: Grant-Type: JWT-Bearer. |
|client_id|necesarias|El identificador de cliente que se configura al registrar la primera API Web como una aplicación de servidor (aplicación de nivel intermedio). Debe ser el mismo que el identificador de recurso usado en el primer segmento, es decir, la dirección URL de la primera API Web.|  
|client_assertion_type|necesarias|El valor debe ser urn: IETF: params: OAuth: Client-Assertion-Type: JWT-Bearer.| 
|client_assertion|necesarias|Una aserción (un token web JSON) que debe crear y firmar con el certificado registrado como credenciales para la aplicación.|  
|assertion|necesarias|Valor del token usado en la solicitud.| 
|requested_token_use|necesarias|Especifica cómo se debe procesar la solicitud. En el flujo OBO, el valor debe establecerse en on_behalf_of| 
|resource|necesarias|El identificador de recurso proporcionado al registrar la primera API Web como la aplicación de servidor (aplicación de nivel intermedio). El identificador de recurso debe ser la dirección URL de la segunda aplicación de nivel intermedio de API Web llamará en nombre del cliente.|
|scope|opcional|Una lista separada por espacios de ámbitos para la solicitud de token.|


Tenga en cuenta que los parámetros son casi iguales que en el caso de la solicitud por secreto compartido, salvo que el parámetro client_secret se sustituye por dos parámetros: client_assertion_type y client_assertion. 

#### <a name="example"></a>Ejemplo 
El siguiente HTTP POST solicita un token de acceso para la API Web con un certificado.

``` 
// line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer 
&client_id= https://webapi.com/ 
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer 
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNS… 
&resource=https://secondwebapi.com/
&requested_token_use=on_behalf_of
&scope= openid 
```    

### <a name="service-to-service-access-token-response"></a>Respuesta de token de acceso de servicio a servicio 
 
Una respuesta correcta es una respuesta JSON OAuth 2,0 con los parámetros siguientes. 


|Parámetro|Descripción|
|-----|-----| 
|token_type|Indica el valor de tipo de token. El único tipo que admite AD FS es Bearer. | 
|scope|El ámbito de acceso concedido en el token.| 
|expires_in|Período de tiempo, en segundos, que el token de acceso es válido.| 
|access_token|El token de acceso solicitado. El servicio de llamada puede usar este token para autenticarse en el servicio de recepción.| 
|id_token|Un JSON Web Token (JWT). La aplicación puede descodificar los segmentos de este token para solicitar información sobre el usuario que ha iniciado sesión. La aplicación puede almacenar en caché los valores y mostrarlos, pero no debe confiar en ellos para ningún límite de autorización o seguridad.| 
|refresh_token|El token de actualización para el token de acceso solicitado. El servicio de llamada puede usar este token para solicitar otro token de acceso después de que expire el token de acceso actual.|
|Refresh_token_expires_in|Período de tiempo, en segundos, que el token de actualización es válido. 

### <a name="success-response-example"></a>Ejemplo de respuesta correcta 
 
En el ejemplo siguiente se muestra una respuesta correcta a una solicitud de un token de acceso para la API Web. 

``` 
{ 
  "token_type": "Bearer", 
  "scope": openid, 
  "expires_in": 3269, 
  "access_token": "eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1t" 
  "id_token": "aWRfdG9rZW49ZXlKMGVYQWlPaUpLVjFRaUxDSmhiR2NpT2lKU1V6STFOa" 
  "refresh_token": "OAQABAAAAAABnfiG…" 
  "refresh_token_expires_in": 28800, 
} 
```  
 
 
Usar el token de acceso para acceder al recurso protegido ahora el servicio de nivel intermedio puede usar el token adquirido anteriormente para realizar solicitudes autenticadas a la API Web de bajada, estableciendo el token en el encabezado de autorización.  

#### <a name="example"></a>Ejemplo 
``` 
GET /v1.0/me HTTP/1.1 
Host: https://secondwebapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1tQ… 
``` 

## <a name="client-credentials-grant-flow"></a>Flujo de concesión de credenciales de cliente 
 
Puede usar la concesión de credenciales de cliente de OAuth 2,0 especificada en [RFC 6749](https://tools.ietf.org/html/rfc6749#section-4.4)para tener acceso a los recursos hospedados en Web mediante la identidad de una aplicación. Este tipo de concesión se usa normalmente para las interacciones de servidor a servidor que se deben ejecutar en segundo plano, sin interacción inmediata con un usuario. Estos tipos de aplicaciones se conocen a menudo como demonios o cuentas de servicio. 

El flujo de concesión de credenciales de cliente de OAuth 2,0 permite a un servicio Web (cliente confidencial) usar sus propias credenciales, en lugar de suplantar a un usuario, para autenticarse al llamar a otro servicio Web. En este escenario, el cliente suele ser un servicio Web de nivel intermedio, un servicio demonio o un sitio Web. Para un mayor nivel de seguridad, el AD FS también permite al servicio de llamada usar un certificado (en lugar de un secreto compartido) como credencial. 

### <a name="protocol-diagram"></a>Diagrama del Protocolo 

En el diagrama siguiente se muestra el flujo de concesión de credenciales de cliente. 

![Credenciales de cliente](media/adfs-scenarios-for-developers/credentials.png)

### <a name="request-a-token"></a>Solicitar un token 
 
Para obtener un token mediante la concesión de credenciales de cliente, `POST` envíe una solicitud al punto de conexión de/token AD FS:  
 
### <a name="first-case-access-token-request-with-a-shared-secret"></a>Primer caso: Solicitud de token de acceso con un secreto compartido 
 
```
POST /adfs/oauth2/token HTTP/1.1            
//Line breaks for clarity 
 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
client_id=535fb089-9ff3-47b6-9bfb-4f1264799865 
&client_secret=qWgdYAmab0YSkuL1qKv5bPX 
&grant_type=client_credentials 
```

|Parámetro|Obligatorio/opcional|Descripción|
|-----|-----|-----| 
|client_id|necesarias|El identificador de la aplicación (cliente) que el AD FS asignado a la aplicación.| 
|scope|opcional|Una lista separada por espacios de ámbitos en los que desea que el usuario dé su consentimiento.| 
|client_secret|necesarias|El secreto de cliente que ha generado para la aplicación en el portal de registro de aplicaciones. El secreto de cliente debe estar codificado como URL antes de enviarse.| 
|grant_type|necesarias|Debe establecerse en `client_credentials`.|

### <a name="second-case-access-token-request-with-a-certificate"></a>Segundo caso: Solicitud de token de acceso con un certificado 

``` 
POST /adfs/oauth2/token HTTP/1.1                
 
// Line breaks for clarity 
 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05 
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer 
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg 
&grant_type=client_credentials  
```

|Parámetro|Obligatorio/opcional|Descripción| 
|-----|-----|-----|
|client_assertion_type|necesarias|El valor se debe establecer en urn: IETF: params: OAuth: Client-Assertion-Type: JWT-Bearer.| 
|client_assertion|necesarias|Una aserción (un token web JSON) que debe crear y firmar con el certificado registrado como credenciales para la aplicación.|  
|grant_type|necesarias|Debe establecerse en `client_credentials`.|
|client_id|opcional|El identificador de la aplicación (cliente) que el AD FS asignado a la aplicación. Esto forma parte de client_assertion, por lo que no es necesario pasarlo aquí.| 
|scope|opcional|Una lista separada por espacios de ámbitos en los que desea que el usuario dé su consentimiento.| 

### <a name="use-a-token"></a>Uso de un token 
 
Ahora que ha adquirido un token, use el token para hacer solicitudes al recurso. Cuando expire el token, repita la solicitud al punto de conexión de/token para adquirir un token de acceso nuevo.  
 
```
GET /v1.0/me/messages 
Host: https://webapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...  
```

## <a name="resource-owner-password-credentials-grant-flow-not-recommended"></a>Flujo de concesión de credenciales de contraseña del propietario del recurso (no recomendado) 
 
La concesión de credenciales de contraseña de propietario de recursos (ROPC) permite a una aplicación iniciar sesión del usuario mediante la administración directa de su contraseña. El flujo ROPC requiere un alto grado de confianza y exposición de los usuarios, y solo debe usar este flujo cuando no se pueden usar otros flujos más seguros.  
 
### <a name="protocol-diagram"></a>Diagrama del Protocolo 
 
En el diagrama siguiente se muestra el flujo de ROPC.

![Flujo de ROPC](media/adfs-scenarios-for-developers/resource.png)

### <a name="authorization-request"></a>Solicitud de autorización 
El flujo ROPC es una solicitud única: envía la identificación del cliente y las credenciales del usuario al IDP y, a continuación, recibe los tokens en la devolución. El cliente debe solicitar la dirección de correo electrónico (UPN) y la contraseña del usuario antes de hacerlo. Inmediatamente después de una solicitud correcta, el cliente debe liberar de forma segura las credenciales del usuario de la memoria. Nunca debe guardarlos.  

```
// Line breaks and spaces are for legibility only. 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com  
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&scope= openid  
&username=myusername@contoso.com 
&password=SuperS3cret 
&grant_type=password 
```


|Parámetro|Obligatorio/opcional|Descripción| 
|-----|-----|-----|
|client_id|necesarias|Client ID| 
|grant_type|necesarias|Debe establecerse en contraseña.| 
|username|necesarias|Dirección de correo electrónico del usuario.| 
|password|necesarias|Contraseña del usuario.| 
|scope|opcional|Lista de ámbitos separados por espacios.|

### <a name="successful-authentication-response"></a>Respuesta de autenticación correcta 
En el ejemplo siguiente se muestra una respuesta de token correcta: 

```
{ 
    "token_type": "Bearer", 
    "scope": "openid", 
    "expires_in": 3599, 
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIn...", 
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...", 
    "refresh_token_expires_in": 28800, 
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDR..." 
}  
```


|Parámetro|Descripción| 
|-----|-----|
|token_type|Siempre se establece en Bearer.| 
|scope|Si se devolvió un token de acceso, este parámetro muestra los ámbitos para los que el token de acceso es válido.| 
|expires_in|Número de segundos durante los que es válido el token de acceso incluido.| 
|access_token|Emitido para los ámbitos que se solicitaron.| 
|id_token|Un JSON Web Token (JWT). La aplicación puede descodificar los segmentos de este token para solicitar información sobre el usuario que ha iniciado sesión. La aplicación puede almacenar en caché los valores y mostrarlos, pero no debe confiar en ellos para ningún límite de autorización o seguridad.| 
|refresh_token_expires_in|Número de segundos que el token de actualización incluido es válido para.| 
|refresh_token|Se emite si el parámetro de ámbito original incluía offline_access.|

Puede usar el token de actualización para adquirir nuevos tokens de acceso y tokens de actualización con el mismo flujo descrito en la sección sobre el flujo de concesión de código de autenticación anterior.   

## <a name="device-code-flow"></a>Flujo de código de dispositivo 
 
La concesión de código de dispositivo permite a los usuarios iniciar sesión en dispositivos con restricción de entrada, como una televisión inteligente, un dispositivo IoT o una impresora. Para habilitar este flujo, el dispositivo tiene el usuario que visita una página web en el explorador en otro dispositivo para iniciar sesión. Una vez que el usuario inicia sesión, el dispositivo puede obtener tokens de acceso y tokens de actualización según sea necesario. 
 
### <a name="protocol-diagram"></a>Diagrama del Protocolo 
 
El flujo de código del dispositivo completo es similar al diagrama siguiente. A continuación se describe cada uno de los pasos que se describen más adelante en este artículo. 
 
![Flujo de código de dispositivo](media/adfs-scenarios-for-developers/device.png)

### <a name="device-authorization-request"></a>Solicitud de autorización de dispositivo 
El cliente debe comprobar primero con el servidor de autenticación para un dispositivo y el código de usuario que se usa para iniciar la autenticación. El cliente recopila esta solicitud del punto de conexión/devicecode. En esta solicitud, el cliente debe incluir también los permisos que necesita adquirir del usuario. Desde el momento en que se envía esta solicitud, el usuario solo tiene 15 minutos para iniciar sesión (el valor habitual de expires_in), por lo que solo debe hacer esta solicitud cuando el usuario haya indicado que está listo para iniciar sesión. 

```
// Line breaks are for legibility only. 
 
POST https://adfs.contoso.com/adfs/oauth2/devicecode 
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
scope=openid 
```


|Parámetro|Condición|Descripción|
|-----|-----|-----| 
|client_id|necesarias|El identificador de la aplicación (cliente) que el AD FS asignado a la aplicación.| 
|scope|opcional|Lista de ámbitos separados por espacios.|

### <a name="device-authorization-response"></a>Respuesta de autorización del dispositivo 
Una respuesta correcta será un objeto JSON que contenga la información necesaria para permitir que el usuario inicie sesión. 


|Parámetro|Descripción|
|-----|-----| 
|device_code|Cadena larga usada para comprobar la sesión entre el cliente y el servidor de autorización. El cliente utiliza este parámetro para solicitar el token de acceso del servidor de autorización.| 
|user_code|Una cadena corta que se muestra al usuario que se usa para identificar la sesión en un dispositivo secundario.| 
|verification_uri|El URI al que debe ir el usuario con user_code para poder iniciar sesión.| 
|verification_uri_complete|El URI al que debe ir el usuario con user_code para poder iniciar sesión. Esto se rellenará con user_code para que el usuario no tenga que escribir user_code| 
|expires_in|El número de segundos antes de que expiren device_code y user_code.| 
|Interval|El número de segundos que el cliente debe esperar entre las solicitudes de sondeo.| 
|mensaje|Una cadena legible para el usuario con instrucciones para el usuario. Se puede localizar mediante la inclusión de un parámetro de consulta en la solicitud del formulario? MKT = XX-XX, rellenando el código de referencia cultural del idioma adecuado.  

### <a name="authenticating-the-user"></a>Autenticación del usuario 
Después de recibir el user_code y el verification_uri, el cliente los muestra al usuario, lo que les indica que inicien sesión con su teléfono móvil o explorador de PC. Además, el cliente puede usar un código QR o un mecanismo similar para mostrar el verfication_uri_complete, que llevará a cabo el paso de escribir user_code para el usuario. Mientras el usuario se autentica en el verification_uri, el cliente debe sondear el punto de conexión de/token para el token solicitado mediante device_code. 

```
POST https://adfs.contoso.com /adfs/oauth2/token 
Content-Type: application/x-www-form-urlencoded 
 
grant_type: urn:ietf:params:oauth:grant-type:device_code 
client_id: 6731de76-14a6-49ae-97bc-6eba6914391e 
device_code: GMMhmHCXhWEzkobqIHGG_EnNYYsAkukHspeYUk9E8 
```


|Parámetro|necesarias|Descripción|
|-----|-----|-----| 
|grant_type|necesarias|Debe ser urn: IETF: params: OAuth: Grant-Type: device_code| 
|client_id|necesarias|Debe coincidir con el client_id utilizado en la solicitud inicial.| 
|code|necesarias|Device_code devuelto en la solicitud de autorización del dispositivo.|

### <a name="successful-authentication-response"></a>Respuesta de autenticación correcta 
Una respuesta de token correcta tendrá el siguiente aspecto:  


|Parámetro|Descripción|
|-----|-----| 
|token_type|Siempre es el portador.| 
|scope|Si se devolvió un token de acceso, se enumeran los ámbitos para los que es válido el token de acceso.| 
|expires_in|Número de segundos antes de que el token de acceso incluido sea válido para.| 
|access_token|Emitido para los ámbitos que se solicitaron.| 
|id_token|Se emite si el parámetro de ámbito original incluía el ámbito de OpenID.| 
|refresh_token|Se emite si el parámetro de ámbito original incluía offline_access.| 
|refresh_token_expires_in|Número de segundos antes de que el token de actualización incluido sea válido para.| 


## <a name="related-content"></a>Contenidos relacionados  
Consulte [AD FS desarrollo](../AD-FS-Development.md) para obtener una lista completa de los artículos de tutoriales, que proporcionan instrucciones paso a paso sobre el uso de los flujos relacionados. 
