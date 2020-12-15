---
description: 'Más información sobre: Flujos de AD FS OpenID Connect/OAuth y escenarios de aplicación'
ms.assetid: 8a64545b-16bd-4c13-a664-cdf4c6ff6ea0
title: Flujos de AD FS OpenID Connect/OAuth y escenarios de aplicación
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 2f2feb9485eb63ea1727fbec8b8300d5ef4f5c51
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97042093"
---
# <a name="ad-fs-openid-connectoauth-flows-and-application-scenarios"></a>Flujos de AD FS OpenID Connect/OAuth y escenarios de aplicación
Se aplica a AD FS 2016 y versiones posteriores


|Escenario|Tutorial del escenario con ejemplos|Flujo/concesión de OAuth 2.0|Tipo de cliente|
|-----|-----|-----|-----|
|Aplicación de una sola página</br> | &bull; [Ejemplo con ADAL](../development/Single-Page-Application-with-AD-FS.md)|[Implícito](#implicit-grant-flow)|Público|
|Aplicación web que inicia la sesión de los usuarios</br> | &bull; [Ejemplo con OWIN](../development/enabling-openid-connect-with-ad-fs.md)|[Código de autorización](#authorization-code-grant-flow)|Público, confidencial|
|La aplicación nativa llama a la API web</br>|&bull; [Ejemplo con MSAL](../development/msal/adfs-msal-native-app-web-api.md)</br>&bull; [Ejemplo con ADAL](../development/native-client-with-ad-fs.md)|[Código de autorización](#authorization-code-grant-flow)|Público|
|La aplicación web llama a la API web</br>|&bull; [Ejemplo con MSAL](../development/msal/adfs-msal-web-app-web-api.md)</br>&bull; [Ejemplo con ADAL](../development/enabling-oauth-confidential-clients-with-ad-fs.md)|[Código de autorización](#authorization-code-grant-flow)|Confidencial|
|La API web llama a otra API web en nombre del usuario</br>|&bull; [Ejemplo con MSAL](../development/msal/adfs-msal-web-api-web-api.md)</br>&bull; [Ejemplo con ADAL](../development/ad-fs-on-behalf-of-authentication-in-windows-server.md)|[En nombre de](#on-behalf-of-flow)|La aplicación web actúa como confidencial|
|La aplicación de demonio llama a la API web||[Credenciales de cliente](#client-credentials-grant-flow)|Confidencial|
|La aplicación web llama a la API web con las credenciales de usuario||[Credenciales de contraseña del propietario del recurso](#resource-owner-password-credentials-grant-flow-not-recommended)|Público, confidencial|
|La aplicación sin explorador llama a la API web||[Código de dispositivo](#device-code-flow)|Público, confidencial|

## <a name="implicit-grant-flow"></a>Flujo de concesión implícito

En el caso de las aplicaciones de una sola página (AngularJS, Ember.js, React.js, etc.), AD FS admite el flujo de concesión implícito de OAuth 2.0. El flujo implícito se describe en la  [especificación de OAuth 2.0](https://tools.ietf.org/html/rfc6749#section-4.2). Su principal ventaja es que permite que la aplicación obtenga tokens de AD FS sin realizar un intercambio de credenciales del servidor backend. Esto permite a la aplicación iniciar la sesión del usuario, mantener la sesión y obtener tokens para otras API web en el código JavaScript del cliente. Existen algunas consideraciones de seguridad importantes que se deben tener en cuenta al usar el flujo implícito específicamente en torno al  [cliente](https://tools.ietf.org/html/rfc6749#section-10.3).

Si quieres usar el flujo implícito y AD FS para agregar la autenticación a tu aplicación de JavaScript, sigue los pasos generales que se indican a continuación.

### <a name="protocol-diagram"></a>Diagrama del protocolo

El diagrama siguiente muestra el aspecto de todo el flujo de inicio de sesión implícito. En las secciones siguientes se describe cada paso más detalladamente.

![Inicio de sesión implícito](media/adfs-scenarios-for-developers/implicit.png)

### <a name="request-id-token-and-access-token"></a>Solicitud del token de identificador y el token de acceso

Para iniciar la sesión del usuario inicialmente en la aplicación, puedes enviar una solicitud de autenticación de OpenID Connect y obtener el parámetro id_token y el token de acceso del punto de conexión de AD FS.

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
|client_id|necesarias|Identificador de la aplicación (cliente) que AD FS asignó a la aplicación.|
|response_type|necesarias|Debe incluir  `id_token`  para el inicio de sesión de OpenID Connect. También puede incluir el valor de  `token` de response_type. El uso de token aquí permitirá que la aplicación reciba un token de acceso inmediatamente del punto de conexión de autorización sin tener que realizar una segunda solicitud al punto de conexión del token.|
|redirect_uri|requerido|Parámetro redirect_uri de la aplicación, donde la aplicación puede enviar y recibir respuestas de autenticación. Debe coincidir exactamente con uno de los parámetros redirect_uri que configuró en AD FS.|
|nonce|requerido|Valor generado por la aplicación que se incluye en la solicitud y que se incluirá en el parámetro id_token resultante como una notificación. La aplicación puede comprobar este valor para mitigar los ataques de reproducción de token. Normalmente, el valor es una cadena única aleatoria que se puede usar para identificar el origen de la solicitud. Solo es necesario cuando se solicita un parámetro id_token.|
|scope|opcional|Lista de ámbitos separados por espacios. Para OpenID Connect, debe incluir el ámbito  `openid`.|
|resource|opcional|Dirección URL de la API web.</br>Nota: si se usa la biblioteca de cliente de MSAL, el parámetro resource no se envía. En su lugar, el parámetro resource url se envía como parte del parámetro scope: `scope = [resource url]//[scope values e.g., openid]`</br>Si el parámetro resource no se pasa aquí ni en el parámetro scope, ADFS usará el recurso predeterminado urn:microsoft:userinfo. Las directivas de recursos userInfo, como las directivas MFA, de emisión o de autorización, no se pueden personalizar.|
|response_mode|opcional| Especifica el método que debe usarse para enviar el token resultante de nuevo a la aplicación. Se establece de forma predeterminada en `fragment`.|
|state|opcional|Valor incluido en la solicitud que también se devolverá en la respuesta del token. Puede ser una cadena con de cualquier contenido que quieras. Normalmente, se usa un valor único generado de forma aleatoria para evitar ataques de falsificación de solicitudes entre sitios. El parámetro state también se usa para codificar la información sobre el estado del usuario en la aplicación antes de la solicitud de autenticación, como la página o la vista en la que se encontraba.|
|símbolo del sistema|opcional|Indica el tipo de interacción de usuario requerida. Los únicos valores válidos en este momento son login y none.</br>- `prompt=login`  obligará al usuario a escribir sus credenciales en esa solicitud y a negar así el inicio de sesión único. </br>- `prompt=none`  es lo contrario. Garantizará que no se presente al usuario ninguna solicitud interactiva. Si la solicitud no se puede completar silenciosamente a través del inicio de sesión único, AD FS devolverá un error de tipo interaction_required.|
|login_hint|opcional|Se puede usar para rellenar previamente el campo de nombre de usuario o dirección de correo electrónico de la página de inicio de sesión del usuario, si conoces el nombre de usuario con antelación. A menudo, las aplicaciones usarán este parámetro durante la reautenticación, después de haber extraído el nombre de usuario de un inicio de sesión anterior mediante la notificación  `upn`  de `id_token`.|
|domain_hint|opcional|Si se incluye, omitirá el proceso de detección basado en dominio que realiza el usuario en la página de inicio de sesión, lo que conduce a una experiencia de usuario un poco más sencilla.|

En este momento, se pedirá al usuario que escriba sus credenciales y complete la autenticación. Cuando el usuario se autentique, el punto de conexión de autorización de AD FS devolverá una respuesta a la aplicación en el parámetro redirect_uri indicado, mediante el método especificado en el parámetro response_mode.

### <a name="successful-response"></a>Respuesta correcta

Una respuesta correcta con  `response_mode=fragment and response_type=id_token+token`  tiene un aspecto similar al siguiente:

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
|access_token|Se incluye si response_type incluye  `token`.|
|token_type|Se incluye si response_type incluye  `token`. Siempre será Bearer.|
|expires_in| Se incluye si response_type incluye  `token`. Indica el número de segundos que el token es válido con fines de almacenamiento en caché.|
|scope| Indica los ámbitos en los que el parámetro access_token será válido.|
|id_token|Se incluye si response_type incluye  `id_token`. Token web JSON (JWT) firmado. La aplicación puede descodificar los segmentos de este token para solicitar información sobre el usuario que ha iniciado sesión. La aplicación puede almacenar en caché los valores y mostrarlos, pero no debe depender de estos para ningún límite de seguridad o autorización.|
|state|Si se incluye un parámetro de estado en la solicitud, debería aparecer el mismo valor en la respuesta. La aplicación debería comprobar que los valores de state de la solicitud y la respuesta sean idénticos.|

### <a name="refresh-tokens"></a>Tokens de actualización
La concesión implícita no proporciona tokens de actualización. Tanto  `id_tokens` como  `access_tokens` expirarán tras un breve período de tiempo, por lo que la aplicación debe estar preparada para actualizar estos tokens periódicamente. Para actualizar cualquier tipo de token, puedes realizar la misma solicitud de iframe oculto anterior mediante el parámetro  `prompt=none`  para controlar el comportamiento de la plataforma de identidad. Si quieres recibir un parámetro `new id_token`, asegúrate de usar  `response_type=id_token`.

## <a name="authorization-code-grant-flow"></a>Flujo de concesión de código de autorización

La concesión de código de autorización de OAuth 2.0 puede usarse en aplicaciones web para obtener acceso a recursos protegidos, como las API web. El flujo de código de autorización de OAuth 2.0 se describe en la  [sección 4.1 de la especificación de OAuth 2.0](https://tools.ietf.org/html/rfc6749). Se usa para realizar la autenticación y la autorización en la mayoría de los tipos de aplicaciones, incluidas las aplicaciones web y las aplicaciones instaladas de forma nativa. El flujo permite que las aplicaciones adquieran de forma segura access_tokens que se pueden usar para acceder a los recursos que confían en AD FS.

### <a name="protocol-diagram"></a>Diagrama del protocolo

En un nivel alto, el flujo de autenticación de una aplicación nativa tiene un aspecto similar al siguiente:

![Flujo de concesión de código de autorización](media/adfs-scenarios-for-developers/authorization.png)

### <a name="request-an-authorization-code"></a>Solicitud de un código de autorización

El flujo de código de autorización comienza con el cliente que dirige al usuario al punto de conexión /authorize. En esta solicitud, el cliente indica los permisos que debe adquirir del usuario:

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
|client_id|necesarias|Identificador de la aplicación (cliente) que AD FS asignó a la aplicación.|
|response_type|necesarias| Debe incluir código para el flujo de código de autorización.|
|redirect_uri|necesarias|Parámetro `redirect_uri` de la aplicación, donde la aplicación puede enviar y recibir respuestas de autenticación. Debe coincidir exactamente con uno de los parámetros redirect_uri registrados en AD FS para el cliente.|
|resource|opcional|Dirección URL de la API web.</br>Nota: si se usa la biblioteca de cliente de MSAL, el parámetro resource no se envía. En su lugar, el parámetro resource url se envía como parte del parámetro scope: `scope = [resource url]//[scope values e.g., openid]`</br>Si el parámetro resource no se pasa aquí ni en el parámetro scope, ADFS usará el recurso predeterminado urn:microsoft:userinfo. Las directivas de recursos userInfo, como las directivas MFA, de emisión o de autorización, no se pueden personalizar.|
|scope|opcional|Lista de ámbitos separados por espacios.|
|response_mode|opcional|Especifica el método que debe usarse para enviar el token resultante de nuevo a la aplicación. Puede ser uno de los siguientes: </br>- query </br>- fragment </br>- form_post</br>`query`  proporciona el código como un parámetro de cadena de consulta en tu URI de redirección. Si estás solicitando el código, puedes usar query, fragment o form_post.  `form_post`  ejecuta una solicitud POST que contiene el código para el URI de redirección.|
|state|opcional|Valor incluido en la solicitud que también se devolverá en la respuesta del token. Puede ser una cadena con de cualquier contenido que quieras. Normalmente, se usa un valor único generado de forma aleatoria para evitar ataques de falsificación de solicitudes entre sitios. El valor también puede codificar la información sobre el estado del usuario en la aplicación antes de la solicitud de autenticación, como la página o la vista en la que se encontraba.|
|prompt|opcional|Indica el tipo de interacción de usuario requerida. Los únicos valores válidos en este momento son login y none.</br>- `prompt=login`  obligará al usuario a escribir sus credenciales en esa solicitud y a negar así el inicio de sesión único. </br>- `prompt=none`  es lo contrario. Garantizará que no se presente al usuario ninguna solicitud interactiva. Si la solicitud no se puede completar silenciosamente a través del inicio de sesión único, AD FS devolverá un error de tipo interaction_required.|
|login_hint|opcional|Se puede usar para rellenar previamente el campo de nombre de usuario o dirección de correo electrónico de la página de inicio de sesión del usuario, si conoces el nombre de usuario con antelación. A menudo, las aplicaciones usarán este parámetro durante la reautenticación, después de haber extraído el nombre de usuario de un inicio de sesión anterior mediante la notificación  `upn` de `id_token`.|
|domain_hint|opcional|Si se incluye, omitirá el proceso de detección basado en dominio que realiza el usuario en la página de inicio de sesión, lo que conduce a una experiencia de usuario un poco más sencilla.|
|code_challenge_method|opcional|Método utilizado para codificar el parámetro code_verifier para el parámetro code_challenge. Puede ser uno de los siguientes valores: </br>- plain </br>- S256 </br>Si se excluye y se incluye  `code_challenge` , se supone que el parámetro code_challenge es texto no cifrado. AD FS admite tanto plain como S256. Para obtener más información, consulta la  [RFC de PKCE](https://tools.ietf.org/html/rfc7636).|
|code_challenge|opcional| Se usa para proteger las concesiones de código de autorización a través de la clave de prueba para el intercambio de código (PKCE) desde un cliente nativo. Obligatorio si se incluye  `code_challenge_method` . Para obtener más información, consulta la  [RFC de PKCE](https://tools.ietf.org/html/rfc7636).|

En este momento, se pedirá al usuario que escriba sus credenciales y complete la autenticación. Cuando el usuario se autentique, AD FS devolverá una respuesta a la aplicación en el parámetro  `redirect_uri` indicado, mediante el método especificado en el parámetro  `response_mode` .

### <a name="successful-response"></a>Respuesta correcta

Una respuesta correcta con response_mode=query tendrá el aspecto siguiente:

```
GET https://adfs.contoso.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```


|Parámetro|Descripción|
|-----|-----|
|code|Parámetro `authorization_code` solicitado por la aplicación. La aplicación puede usar el código de autorización para solicitar un token de acceso para el recurso de destino. Los parámetros authorization_code tienen una duración corta y normalmente expiran después de unos 10 minutos.|
|state|Si un parámetro `state` está incluido en la solicitud, debería aparecer el mismo valor en la respuesta. La aplicación debería comprobar que los valores de state de la solicitud y la respuesta sean idénticos.|

### <a name="request-an-access-token"></a>Solicitud de un token de acceso

Ahora que has adquirido un parámetro `authorization_code` y que el usuario te ha concedido permiso, puedes canjear el código por un parámetro  `access_token`  para el recurso deseado. Para ello, envía una solicitud POST al punto de conexión /token:

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
|client_id|necesarias|Identificador de la aplicación (cliente) que AD FS asignó a la aplicación.|
|grant_type|necesarias|Debe ser  `authorization_code`  para el flujo de código de autorización.|
|code|necesarias|Parámetro `authorization_code` que has adquirido en el primer segmento del flujo.|
|redirect_uri|necesarias|El mismo valor de `redirect_uri` usado para adquirir el parámetro `authorization_code`.|
|client_secret|necesario para las aplicaciones web|Secreto de la aplicación que creaste durante el registro de la aplicación en AD FS. No debes usar el secreto de aplicación en una aplicación nativa porque el parámetro client_secrets no se puede almacenar de forma confiable en los dispositivos. Es necesario para aplicaciones web y las API web, que tienen la capacidad de almacenar el client_secret de manera segura en el lado del servidor. El secreto de cliente debe estar formateado con codificación URL antes de enviarse. Estas aplicaciones también pueden usar una autenticación basada en claves, mediante la firma de un JWT y su adición como el parámetro client_assertion.|
|code_verifier|opcional|Mismo parámetro `code_verifier` usado para obtener el parámetro authorization_code. Obligatorio si se usó PKCE en la solicitud de concesión de código de autorización. Para obtener más información, consulta la  [RFC de PKCE](https://tools.ietf.org/html/rfc7636).</br>Nota: Se aplica a AD FS 2019 y versiones posteriores|

### <a name="successful-response"></a>Respuesta correcta

Una respuesta de token correcta tendrá un aspecto similar al siguiente:

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
|access_token|Token de acceso solicitado. La aplicación puede usar este token para autenticarse en el recurso protegido (API web).|
|token_type|Indica el valor de tipo de token. El único tipo que admite AD FS es Bearer.
|expires_in|Tiempo de validez del token de acceso (en segundos).
|refresh_token|Token de actualización de OAuth 2.0. La aplicación puede usar este token para adquirir tokens de acceso adicionales una vez que el token de acceso actual expire. Los parámetros refresh_token son de larga duración y se pueden usar para conservar el acceso a los recursos durante largos períodos de tiempo.|
|refresh_token_expires_in|Tiempo de validez del token de actualización (en segundos).|
|id_token|Token web JSON (JWT). La aplicación puede descodificar los segmentos de este token para solicitar información sobre el usuario que ha iniciado sesión. La aplicación puede almacenar en caché los valores y mostrarlos, pero no debe depender de estos para ningún límite de seguridad o autorización.|

### <a name="use-the-access-token"></a>Uso del token de acceso

```
GET /v1.0/me/messages
Host: https://webapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
 ```

### <a name="refresh-token-grant-flow"></a>Flujo de concesión de tokens de actualización
 
Los parámetros access_token tienen una duración corta y deben actualizarse cuando expiren para seguir teniendo acceso a los recursos. Para hacerlo, puedes enviar otra solicitud POST al punto de conexión  `/token`  y proporcionar el parámetro refresh_token en lugar del código esta vez. Los tokens de actualización son válidos para todos los permisos para los que el cliente ha recibido el token de acceso.

Los tokens de actualización no tienen una duración especificada. Normalmente, las duraciones de este tipo de tokens son relativamente largas. Sin embargo, en algunos casos, los tokens de actualización expiran, se revocan o carecen de privilegios suficientes para la acción deseada. La aplicación necesita esperar y controlar correctamente los errores devueltos por el punto de conexión de emisión de tokens.

Aunque los tokens de actualización no se revocan cuando se usan para adquirir nuevos tokens de acceso, se espera que descartes el token de actualización anterior. La especificación OAuth 2.0 indica lo siguiente: "El servidor de autorización puede emitir un nuevo token de actualización, en cuyo caso el cliente debe descartar el token de actualización anterior y reemplazarlo por el nuevo. El servidor de autorización puede revocar el token de actualización anterior después de emitir uno nuevo para el cliente". AD FS emite el token de actualización cuando la vigencia del nuevo token de actualización es mayor que la vigencia anterior del token de actualización.  Para ver información adicional sobre la vigencia de los tokens de actualización de AD FS, visita [Configuración de inicio de sesión único de AD FS](../operations/ad-fs-single-sign-on-settings.md).

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
|client_id|necesarias|Identificador de la aplicación (cliente) que AD FS asignó a la aplicación.|
|grant_type|necesarias|Debe ser  `refresh_token`  para este segmento del flujo de código de autorización.|
|resource|opcional|Dirección URL de la API web.</br>Nota: si se usa la biblioteca de cliente de MSAL, el parámetro resource no se envía. En su lugar, el parámetro resource url se envía como parte del parámetro scope: `scope = [resource url]//[scope values e.g., openid]`</br>Si el parámetro resource no se pasa aquí ni en el parámetro scope, ADFS usará el recurso predeterminado urn:microsoft:userinfo. Las directivas de recursos userInfo, como las directivas MFA, de emisión o de autorización, no se pueden personalizar.|
|scope|opcional|Lista de ámbitos separados por espacios.|
|refresh_token|necesarias|Parámetro refresh_token que adquiriste en el segundo segmento del flujo.|
|client_secret|necesario para las aplicaciones web| Secreto de aplicación que creaste para tu aplicación en el portal de registro de aplicaciones. No se debe usar en una aplicación nativa porque los parámetros client_secret no se pueden almacenar de forma confiable en los dispositivos. Es necesario para las aplicaciones web y las API web, que tienen la capacidad de almacenar el parámetro client_secret de forma segura en el lado servidor. Estas aplicaciones también pueden usar una autenticación basada en claves, mediante la firma de un JWT y su adición como el parámetro client_assertion.|

### <a name="successful-response"></a>Respuesta correcta
Una respuesta de token correcta tendrá un aspecto similar al siguiente:

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
|access_token|Token de acceso solicitado. La aplicación puede usar este token para autenticarse en el recurso protegido, como una API web.|
|token_type|Indica el valor de tipo de token. El único tipo que admite AD FS es Bearer.|
|expires_in|Durante cuánto tiempo es válido el token de acceso (en segundos).|
|scope|Ámbitos en los que es válido el parámetro access_token.|
|refresh_token|Token de actualización de OAuth 2.0. La aplicación puede usar este token para adquirir tokens de acceso adicionales una vez que el token de acceso actual expire. Los parámetros refresh_token son de larga duración y se pueden usar para conservar el acceso a los recursos durante largos períodos de tiempo.|
|refresh_token_expires_in|Tiempo de validez del token de actualización (en segundos).|
|id_token|Token web JSON (JWT). La aplicación puede descodificar los segmentos de este token para solicitar información sobre el usuario que ha iniciado sesión. La aplicación puede almacenar en caché los valores y mostrarlos, pero no debe depender de estos para ningún límite de seguridad o autorización.|

## <a name="on-behalf-of-flow"></a>Flujo con derechos delegados

El flujo con derechos delegados de OAuth 2.0 sirve para el caso de uso en el que una aplicación invoca una API de servicio o web, que a su vez necesita llamar a otra API de servicio o web. La idea es propagar la identidad del usuario delegado y los permisos a través de la cadena de solicitudes. Para poder realizar solicitudes autenticadas en el servicio de bajada, el servicio de nivel intermedio debe proteger un token de acceso de AD FS en nombre del usuario.

### <a name="protocol-diagram"></a>Diagrama del protocolo
Supongamos que el usuario se ha autenticado en una aplicación mediante el flujo de concesión de código de autorización de OAuth 2.0 descrito anteriormente. En este punto, la aplicación tiene un token de acceso para la API A (token A) con las notificaciones y el consentimiento del usuario para acceder a la API web de nivel intermedio (API A). Asegúrate de que el cliente solicita el ámbito user_impersonation en el token. Ahora, la API A debe realizar una solicitud autenticada a la API web de bajada (API B).

Los pasos siguientes constituyen el flujo con derechos delegados y se explican con la ayuda del diagrama siguiente.

![Flujo con derechos delegados](media/adfs-scenarios-for-developers/obo.png)

  1. La aplicación cliente realiza una solicitud a la API A con el token A. Nota: Mientras configuras el flujo con permisos delegados en AD FS, asegúrate de que el ámbito `user_impersonation` esté seleccionado y de que el cliente solicite el ámbito `user_impersonation` en la solicitud.
  2. La API A se autentica en el punto de conexión de emisión de tokens de AD FS y solicita un token para acceder a la API B. Nota: Al configurar este flujo en AD FS, asegúrate de que la API A también se registre como una aplicación de servidor, cuyo valor de clientID coincida con el identificador de recurso de la API A.
  3. El punto de conexión de emisión de tokens de AD FS valida las credenciales de la API A con el token A y emite el token de acceso para la API B (token B).
  4. El token B se establece en el encabezado de autorización de la solicitud a la API B.
  5. La API B devuelve los datos del recurso protegido.

### <a name="service-to-service-access-token-request"></a>Solicitud de token de acceso de servicio a servicio

Para solicitar un token de acceso, realiza una solicitud HTTP POST al punto de conexión del token de AD FS con los parámetros siguientes.


### <a name="first-case-access-token-request-with-a-shared-secret"></a>Primer caso: solicitud de token de acceso con un secreto compartido

Cuando se utiliza un secreto compartido, una solicitud de token de acceso entre servicios contiene los parámetros siguientes:


|Parámetro|Obligatorio/opcional|Descripción|
|-----|-----|-----|
|grant_type|necesarias|Tipo de solicitud de token. Para una solicitud que utilice un JWT, el valor debe ser urn:ietf:params:oauth:grant-type:jwt-bearer.|
|client_id|necesarias|Identificador de cliente que se configura al registrar la primera API web como una aplicación de servidor (aplicación de nivel intermedio). Debe ser el mismo que el identificador de recurso usado en el primer segmento (es decir, la dirección URL de la primera API web).|
|client_secret|necesarias|Secreto de la aplicación que creaste durante el registro de la aplicación de servidor en AD FS.|
|assertion|requerido|Valor del token usado en la solicitud.|
|requested_token_use|requerido|Especifica cómo se debe procesar la solicitud. En el flujo con permisos delegados, el valor se debe establecer en on_behalf_of.|
|resource|necesarias|Identificador de recurso proporcionado al registrar la primera API web como la aplicación de servidor (aplicación de nivel intermedio). El identificador de recurso debe ser la dirección URL de la segunda API web que aplicación de nivel intermedio llamará en nombre del cliente.|
|scope|opcional|Lista separada por espacios de los ámbitos de la solicitud de token.|

#### <a name="example"></a>Ejemplo

La solicitud `HTTP POST` siguiente solicita un token de acceso y un token de actualización:

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

### <a name="second-case-access-token-request-with-a-certificate"></a>Segundo caso: solicitud de token de acceso con un certificado

Una solicitud de token de acceso entre servicios con un certificado contiene los parámetros siguientes:


|Parámetro|Obligatorio/opcional|Descripción|
|-----|-----|-----|
|grant_type|necesarias|Tipo de solicitud de token. Para una solicitud que utilice un JWT, el valor debe ser urn:ietf:params:oauth:grant-type:jwt-bearer. |
|client_id|necesarias|Identificador de cliente que se configura al registrar la primera API web como una aplicación de servidor (aplicación de nivel intermedio). Debe ser el mismo que el identificador de recurso usado en el primer segmento (es decir, la dirección URL de la primera API web).|
|client_assertion_type|necesarias|El valor debe ser urn:ietf:params:oauth:client-assertion-type:jwt-bearer.|
|client_assertion|necesarias|Aserción (token web JSON) que debes crear y firmar con el certificado que registraste como credenciales para la aplicación.|
|assertion|requerido|Valor del token usado en la solicitud.|
|requested_token_use|requerido|Especifica cómo se debe procesar la solicitud. En el flujo con permisos delegados, el valor se debe establecer en on_behalf_of.|
|resource|necesarias|Identificador de recurso proporcionado al registrar la primera API web como la aplicación de servidor (aplicación de nivel intermedio). El identificador de recurso debe ser la dirección URL de la segunda API web que aplicación de nivel intermedio llamará en nombre del cliente.|
|scope|opcional|Lista separada por espacios de los ámbitos de la solicitud de token.|


Ten en cuenta que los parámetros son casi iguales que en el caso de la solicitud con secreto compartido, salvo que el parámetro client_secret se reemplaza por dos parámetros: client_assertion_type y client_assertion.

#### <a name="example"></a>Ejemplo
La solicitud HTTP POST siguiente solicita un token de acceso para la API web con un certificado.

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

Una respuesta correcta es una respuesta de OAuth 2.0 de JSON con los parámetros siguientes.


|Parámetro|Descripción|
|-----|-----|
|token_type|Indica el valor de tipo de token. El único tipo que admite AD FS es Bearer. |
|scope|Ámbito de acceso concedido en el token.|
|expires_in|Tiempo de validez en segundos del token de acceso.|
|access_token|El token de acceso solicitado. El servicio de llamada puede usar este token para autenticarse en el servicio de recepción.|
|id_token|Token web JSON (JWT). La aplicación puede descodificar los segmentos de este token para solicitar información sobre el usuario que ha iniciado sesión. La aplicación puede almacenar en caché los valores y mostrarlos, pero no debe depender de estos para ningún límite de seguridad o autorización.|
|refresh_token|Token de actualización para el token de acceso solicitado. El servicio de llamada puede usar este token para solicitar otro token de acceso cuando expire el token de acceso actual.|
|Refresh_token_expires_in|Tiempo de validez en segundos del token de actualización.

### <a name="success-response-example"></a>Ejemplo de respuesta correcta

En el ejemplo siguiente se muestra una respuesta correcta a una solicitud de un token de acceso para la API web.

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


Usa el token de acceso para acceder al recurso protegido. Ahora el servicio de nivel intermedio puede usar el token adquirido anteriormente para realizar solicitudes autenticadas a la API web de bajada, mediante la definición del token en el encabezado Authorization.

#### <a name="example"></a>Ejemplo
```
GET /v1.0/me HTTP/1.1
Host: https://secondwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1tQ…
```

## <a name="client-credentials-grant-flow"></a>Flujo de concesión de credenciales de cliente

Puede usar la concesión de credenciales de cliente de OAuth 2.0 especificada en [RFC 6749](https://tools.ietf.org/html/rfc6749#section-4.4) para acceder a los recursos hospedados en web mediante la identidad de una aplicación. Este tipo de concesión se usa normalmente para las interacciones de servidor a servidor que se deben ejecutar en segundo plano, sin la interacción inmediata con un usuario. Estos tipos de aplicaciones se conocen a menudo como demonios o cuentas de servicio.

El flujo de concesión de credenciales de cliente de OAuth 2.0 permite que un servicio web (un cliente confidencial) use sus propias credenciales para autenticarse al llamar a otro servicio web, en lugar de realizar una suplantación de usuario. En este escenario, el cliente suele ser un servicio web de nivel intermedio, un servicio de demonio o un sitio web. Para un mayor nivel de seguridad, AD FS también permite al servicio de llamada usar un certificado (en lugar de un secreto compartido) como credencial.

### <a name="protocol-diagram"></a>Diagrama del protocolo

En el diagrama siguiente se muestra el flujo de concesión de credenciales de cliente.

![Credenciales de cliente](media/adfs-scenarios-for-developers/credentials.png)

### <a name="request-a-token"></a>Solicitar un token

Para obtener un token mediante la concesión de credenciales de cliente, envía una solicitud `POST` al punto de conexión /token de AD FS:

### <a name="first-case-access-token-request-with-a-shared-secret"></a>Primer caso: solicitud de token de acceso con un secreto compartido

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
|client_id|necesarias|Identificador de la aplicación (cliente) que AD FS asignó a la aplicación.|
|scope|opcional|Lista separada por espacios de los ámbitos que quieres que consienta el usuario.|
|client_secret|necesarias|Secreto de cliente que generaste para la aplicación en el portal de registro de aplicaciones. El secreto de cliente debe estar formateado con codificación URL antes de enviarse.|
|grant_type|necesarias|Se debe establecer en  `client_credentials`.|

### <a name="second-case-access-token-request-with-a-certificate"></a>Segundo caso: solicitud de token de acceso con un certificado

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
|client_assertion_type|necesarias|El valor debe estar establecido en urn:ietf:params:oauth:client-assertion-type:jwt-bearer.|
|client_assertion|necesarias|Aserción (token web JSON) que debes crear y firmar con el certificado que registraste como credenciales para la aplicación.|
|grant_type|necesarias|Se debe establecer en  `client_credentials`.|
|client_id|opcional|Identificador de la aplicación (cliente) que AD FS asignó a la aplicación. Forma parte de client_assertion, por lo que no es necesario pasarlo aquí.|
|scope|opcional|Lista separada por espacios de los ámbitos que quieres que consienta el usuario.|

### <a name="use-a-token"></a>Uso de un token

Ahora que has adquirido un token, úsalo para realizar solicitudes al recurso. Cuando el token expire, repite la solicitud al punto de conexión /token para adquirir un token de acceso nuevo.

```
GET /v1.0/me/messages
Host: https://webapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="resource-owner-password-credentials-grant-flow-not-recommended"></a>Flujo de concesión de credenciales de contraseña del propietario del recurso (no recomendado)

La concesión de credenciales de contraseña del propietario del recurso (ROPC) permite a una aplicación iniciar la sesión del usuario mediante el control directo de su contraseña. El flujo de ROPC requiere un alto grado de confianza y exposición del usuario, y solo debes usarlo cuando no puedas usar otros flujos más seguros.

### <a name="protocol-diagram"></a>Diagrama del protocolo

El diagrama siguiente muestra el flujo de ROPC.

![Flujo de ROPC](media/adfs-scenarios-for-developers/resource.png)

### <a name="authorization-request"></a>Solicitud de autorización
El flujo de ROPC es una solicitud única: envía la identificación del cliente y las credenciales del usuario al IDP y, a cambio, recibe los tokens. El cliente debe solicitar la dirección de correo electrónico (UPN) y la contraseña del usuario antes de hacerlo. Inmediatamente después de que una solicitud se realice correctamente, el cliente deberá liberar de manera segura las credenciales del usuario de la memoria. Nunca deben guardarse.

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
|client_id|necesarias|Id. de cliente|
|grant_type|necesarias|Se debe establecer en password.|
|nombreDeUsuario|necesarias|Dirección de correo electrónico del usuario.|
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
|expires_in|Número de segundos de validez del token de acceso incluido.|
|access_token|Se emite para los ámbitos solicitados.|
|id_token|Token web JSON (JWT). La aplicación puede descodificar los segmentos de este token para solicitar información sobre el usuario que ha iniciado sesión. La aplicación puede almacenar en caché los valores y mostrarlos, pero no debe depender de estos para ningún límite de seguridad o autorización.|
|refresh_token_expires_in|Número de segundos de validez del token de actualización incluido.|
|refresh_token|Se emite si el parámetro de ámbito original incluye offline_access.|

Puedes usar el token de actualización para adquirir nuevos tokens de acceso y tokens de actualización con el mismo flujo descrito en la sección sobre el flujo de concesión de código de autenticación anterior.

## <a name="device-code-flow"></a>Flujo de código de dispositivo

La concesión de código de dispositivo permite a los usuarios iniciar sesión en dispositivos con restricciones de entrada, como un televisor inteligente, un dispositivo IoT o una impresora. Para habilitar este flujo, el dispositivo exige al usuario visitar una página web en el explorador o en otro dispositivo para iniciar sesión. Una vez que el usuario inicia sesión, el dispositivo puede obtener los tokens de acceso y los tokens de actualización según sea necesario.

### <a name="protocol-diagram"></a>Diagrama del protocolo

El flujo de código de dispositivo completo es similar al del diagrama siguiente. Más adelante en este artículo se describe cada uno de los pasos.

![Flujo de código de dispositivo](media/adfs-scenarios-for-developers/device.png)

### <a name="device-authorization-request"></a>Solicitud de autorización de dispositivo
En primer lugar, el cliente debe consultar al servidor de autenticación el código de dispositivo y de usuario que se usa para iniciar la autenticación. El cliente recopila esta solicitud del punto de conexión /devicecode. En esta solicitud, el cliente también tiene que incluir los permisos que debe adquirir del usuario. Desde el momento en que se envía esta solicitud, el usuario solo tiene 15 minutos para iniciar sesión (el valor habitual de expires_in), por lo que solo debes realizar esta solicitud cuando el usuario haya indicado que está listo para iniciar sesión.

```
// Line breaks are for legibility only.

POST https://adfs.contoso.com/adfs/oauth2/devicecode
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
scope=openid
```


|Parámetro|Condición|Descripción|
|-----|-----|-----|
|client_id|necesarias|Identificador de la aplicación (cliente) que AD FS asignó a la aplicación.|
|scope|opcional|Lista de ámbitos separados por espacios.|

### <a name="device-authorization-response"></a>Respuesta de autorización de dispositivo
Una respuesta correcta será un objeto JSON que contenga la información necesaria para permitir que el usuario inicie sesión.


|Parámetro|Descripción|
|-----|-----|
|device_code|Cadena larga usada para comprobar la sesión entre el cliente y el servidor de autorización. El cliente utiliza este parámetro para solicitar el token de acceso al servidor de autorización.|
|user_code|Cadena corta que se muestra al usuario y que se usa para identificar la sesión en un dispositivo secundario.|
|verification_uri|URI al que debe acudir el usuario con el parámetro user_code para poder iniciar sesión.|
|verification_uri_complete|URI al que debe acudir el usuario con el parámetro user_code para poder iniciar sesión. Se rellenará previamente con user_code de forma que el usuario no tenga que escribirlo.|
|expires_in|Número de segundos antes de que expiren los códigos de device_code y user_code.|
|interval|Número de segundos que el cliente debe esperar entre las solicitudes de sondeo.|
|message|Cadena legible con instrucciones para el usuario. Para localizarla, se puede incluir un parámetro de consulta en la solicitud del formulario ?mkt=xx-XX y rellenar el código de referencia cultural del idioma adecuado.

### <a name="authenticating-the-user"></a>Autenticación del usuario
Después de recibir los valores de user_code y verification_uri, el cliente los muestra al usuario y le indica que inicie sesión con su teléfono móvil o el explorador del equipo. Además, el cliente puede usar un código QR o un mecanismo similar para mostrar el parámetro verfication_uri_complete, con lo que el parámetro user_code se escribirá automáticamente.
Mientras el usuario se autentica en verification_uri, el cliente debe sondear el punto de conexión /token del token solicitado mediante el valor de device_code.

```
POST https://adfs.contoso.com /adfs/oauth2/token
Content-Type: application/x-www-form-urlencoded

grant_type: urn:ietf:params:oauth:grant-type:device_code
client_id: 6731de76-14a6-49ae-97bc-6eba6914391e
device_code: GMMhmHCXhWEzkobqIHGG_EnNYYsAkukHspeYUk9E8
```


|Parámetro|necesarias|Descripción|
|-----|-----|-----|
|grant_type|necesarias|Debe ser urn:ietf:params:oauth:grant-type:device_code.|
|client_id|necesarias|Debe coincidir con el valor de client_id utilizado en la solicitud inicial.|
|code|necesarias|Valor de device_code devuelto en la solicitud de autorización de dispositivo.|

### <a name="successful-authentication-response"></a>Respuesta de autenticación correcta
Una respuesta de token correcta tendrá un aspecto similar al siguiente:


|Parámetro|Descripción|
|-----|-----|
|token_type|Siempre es Bearer.|
|scope|Si se devolvió un token de acceso, muestra los ámbitos para los que el token de acceso es válido.|
|expires_in|Número de segundos de validez del token de acceso incluido.|
|access_token|Se emite para los ámbitos solicitados.|
|id_token|Se emite si el parámetro de ámbito original incluye el ámbito openid.|
|refresh_token|Se emite si el parámetro de ámbito original incluye offline_access.|
|refresh_token_expires_in|Número de segundos de validez del token de actualización incluido.|


## <a name="related-content"></a>Contenidos relacionados
Consulta [Desarrollo de AD FS](../AD-FS-Development.md) para obtener una lista completa de los artículos de tutoriales, que te proporcionan instrucciones paso a paso sobre el uso de los flujos relacionados.
