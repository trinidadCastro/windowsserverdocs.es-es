---
title: Desconecte solo por OpenID conectar con AD FS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/17/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3af10ec139edbc72e75bf80f544ac5b4f1cf9222
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2018
---
#  <a name="single-log-out-for-openid-connect-with-ad-fs"></a>Desconecte solo por OpenID conectar con AD FS

## <a name="overview"></a>Introducción
Ampliar la compatibilidad de Oauth inicial de AD FS en Windows Server 2012 R2, AD FS 2016 introdujo la compatibilidad para conectar OpenId inicio de sesión. Con [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801), AD FS 2016 ahora admite fuera de registro única para escenarios de OpenId conectar. Este artículo proporciona una visión general de la salida de registro único escenario conectar OpenId y proporciona instrucciones sobre cómo usarla para sus aplicaciones OpenId conectar en AD FS.


## <a name="discovery-doc"></a>Documento de detección
Conectar OpenID usa un documento JSON que se denomina una "detección de documento" para proporcionar información detallada sobre la configuración.  Esto incluye el URI de la autenticación, token, userinfo y extremos del público.  El siguiente es un ejemplo de un documento de la detección.

```
{
"issuer":"https://fs.fabidentity.com/adfs",
"authorization_endpoint":"https://fs.fabidentity.com/adfs/oauth2/authorize/",
"token_endpoint":"https://fs.fabidentity.com/adfs/oauth2/token/",
"jwks_uri":"https://fs.fabidentity.com/adfs/discovery/keys",
"token_endpoint_auth_methods_supported":["client_secret_post","client_secret_basic","private_key_jwt","windows_client_authentication"],
"response_types_supported":["code","id_token","code id_token","id_token token","code token","code id_token token"],
"response_modes_supported":["query","fragment","form_post"],
"grant_types_supported":["authorization_code","refresh_token","client_credentials","urn:ietf:params:oauth:grant-type:jwt-bearer","implicit","password","srv_challenge"],
"subject_types_supported":["pairwise"],
"scopes_supported":["allatclaims","email","user_impersonation","logon_cert","aza","profile","vpn_cert","winhello_cert","openid"],
"id_token_signing_alg_values_supported":["RS256"],
"token_endpoint_auth_signing_alg_values_supported":["RS256"],
"access_token_issuer":"http://fs.fabidentity.com/adfs/services/trust",
"claims_supported":["aud","iss","iat","exp","auth_time","nonce","at_hash","c_hash","sub","upn","unique_name","pwd_url","pwd_exp","sid"],
"microsoft_multi_refresh_token":true,
"userinfo_endpoint":"https://fs.fabidentity.com/adfs/userinfo",
"capabilities":[],
"end_session_endpoint":"https://fs.fabidentity.com/adfs/oauth2/logout",
"as_access_token_token_binding_supported":true,
"as_refresh_token_token_binding_supported":true,
"resource_access_token_token_binding_supported":true,
"op_id_token_token_binding_supported":true,
"rp_id_token_token_binding_supported":true,
"frontchannel_logout_supported":true,
"frontchannel_logout_session_supported":true
} 
 
```



Los siguientes valores adicionales estarán disponibles en el documento del detección para indicar la compatibilidad con frontal cierre de sesión de canal:

- frontchannel_logout_supported: valor será 'true'
- frontchannel_logout_session_supported: valor será 'true'.
- end_session_endpoint: este es el URI que el cliente puede usar para iniciar el cierre de sesión en el servidor de cierre de sesión de OAuth.


## <a name="ad-fs-server-configuration"></a>Configuración del servidor de AD FS
La propiedad de AD FS EnableOAuthLogout se habilitará de forma predeterminada.  Esta propiedad indica el servidor de AD FS para buscar la dirección URL (LogoutURI) con el SID para iniciar el cierre de sesión en el cliente. Si no tienes [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) instalado, puedes usar el siguiente comando de PowerShell:

```PowerShell
Set-ADFSProperties -EnableOAuthLogout $true
```

>[!NOTE]
> `EnableOAuthLogout` Parámetro se marcarán como obsoleta después de instalar [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801). `EnableOAUthLogout` Siempre será true y no tendrá ningún efecto en la funcionalidad de cierre de sesión.

>[!NOTE]
>Se admite frontchannel_logout **solo** después de la instalación de [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)

## <a name="client-configuration"></a>Configuración del cliente
Cliente necesita implementar una dirección url que 'cierra' el inicio de sesión de usuario. Administrador puede configurar la LogoutUri en la configuración de cliente con los siguientes cmdlets de PowerShell. 


- `(Add | Set)-AdfsNativeApplication`
- `(Add | Set)-AdfsServerApplication`
- `(Add | Set)-AdfsClient`

```PowerShell
Set-AdfsClient -LogoutUri <url>
```

La `LogoutUri`es la dirección url AF FS utilizado para "cerrar sesión" al usuario. Para implementar el `LogoutUri`, las necesidades del cliente para garantizar que se borra el estado de autenticación del usuario en la aplicación, por ejemplo, colocar la autenticación tokens que tiene. AD FS irá a esa dirección URL, con el SID como el parámetro de consulta, el usuario de confianza de señalización o aplicación para cerrar la sesión del usuario. 

![](media/ad-fs-logout-openid-connect/adfs_single_logout2.png)


1.  **Token de OAuth con el identificador de sesión**: AD FS incluye un identificador de sesión en el token de OAuth en el momento de emisión de token id_token. Esto se usará más tarde por AD FS para identificar las cookies SSO relevantes limpiar para que el usuario.
2.  **Usuario inicia el cierre de sesión en App1**: el usuario puede iniciar un cierre de sesión desde cualquiera de las aplicaciones de la sesión iniciada. En este escenario de ejemplo, un usuario inicia un cierre de sesión de App1.
3.  **Aplicación envía la solicitud de cierre de sesión a AD FS**: después de que el usuario inicia el cierre de sesión, la aplicación envía una solicitud GET a end_session_endpoint de AD FS. La aplicación puede incluir opcionalmente id_token_hint como un parámetro a esta solicitud. Si id_token_hint está presente, AD FS usarlo junto con el identificador de sesión determinar a qué URI el cliente de ser redirigido a tras el cierre de sesión (post_logout_redirect_uri).  El post_logout_redirect_uri debe ser un uri válido registrado con AD FS con el parámetro RedirectUris.
4.  **AD FS envía cierre de sesión a los clientes que hayan iniciado sesión**: usa AD FS el valor de identificador de sesión para buscar los clientes relevantes que el usuario ha iniciado sesión con. Los clientes identificados se envían solicitud en la LogoutUri registrado con AD FS para iniciar un cierre de sesión del cliente.

## <a name="faqs"></a>Preguntas más frecuentes
**P: ¿** no ven los parámetros frontchannel_logout_supported y frontchannel_logout_session_supported en el documento de detección.</br>
**R:** Asegúrate de que tienes [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) instalado en todos los servidores de AD FS. Consulta a solo registro de servidor 2016 con [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801).

**P: ¿** he configurado tal como se indica el cierre de sesión único, pero permanece usuario inició sesión en otros clientes.</br>
**R:** asegurarse de que `LogoutUri`se establece para todos los clientes donde es el usuario ha iniciado sesión. Además, AD FS hace un mejor intento de enviar la solicitud de cierre de sesión en el registrados `LogoutUri`. Cliente debe implementar la lógica para controlar la solicitud y realizar alguna acción para cierre de sesión del usuario respecto de la aplicación.</br>

**P: ¿** si después de cierre de sesión, uno de los clientes vuelve a AD FS con un token de actualización válido, AD FS emitirá un token de acceso?</br>
**R:** sí. Es responsabilidad de la aplicación cliente colocar artefactos autenticados todo después de que se recibió una solicitud de cierre de sesión en el registrados `LogoutUri`.


## <a name="next-steps"></a>Pasos siguientes
[AD FS desarrollo](../../ad-fs/AD-FS-Development.md)  
