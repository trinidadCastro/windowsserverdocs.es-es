---
title: Cierre de sesión único de OpenID Connect con AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/17/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3af10ec139edbc72e75bf80f544ac5b4f1cf9222
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825776"
---
#  <a name="single-log-out-for-openid-connect-with-ad-fs"></a>Cierre de sesión único de OpenID Connect con AD FS

## <a name="overview"></a>Información general
A partir de la compatibilidad de Oauth inicial de AD FS en Windows Server 2012 R2, AD FS 2016 introdujo la compatibilidad con inicio de sesión OpenId Connect. Con [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801), AD FS 2016 ahora admite el cierre de sesión único para escenarios de OpenId Connect. Este artículo proporciona información general sobre el cierre de sesión único para el escenario de OpenId Connect y proporciona instrucciones sobre cómo se usa para las aplicaciones de OpenId Connect de AD FS.


## <a name="discovery-doc"></a>Documento de descubrimiento
OpenID Connect usa un documento JSON que se denomina un "documento de detección" para proporcionar detalles sobre la configuración.  Esto incluye a los URI de la autenticación, token, userinfo y puntos de conexión de público.  El siguiente es un ejemplo de lo documentos de descubrimiento.

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



Los siguientes valores adicionales estarán disponibles en el documento de detección para indicar la compatibilidad de cierre de sesión de canal frontal:

- frontchannel_logout_supported: valor será 'true'
- frontchannel_logout_session_supported: valor será 'true'.
- end_session_endpoint: este es el identificador URI que el cliente puede usar para iniciar el cierre de sesión en el servidor de cierre de sesión de OAuth.


## <a name="ad-fs-server-configuration"></a>Configuración del servidor de AD FS
La propiedad de AD FS EnableOAuthLogout se habilitará de forma predeterminada.  Esta propiedad indica al servidor de AD FS para buscar la dirección URL (LogoutURI) con el SID para iniciar el cierre de sesión en el cliente. Si no tienes [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) instalado, puede usar el siguiente comando de PowerShell:

```PowerShell
Set-ADFSProperties -EnableOAuthLogout $true
```

>[!NOTE]
> `EnableOAuthLogout` parámetro se marcará como obsoleto después de instalar [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801). `EnableOAUthLogout` siempre será verdadera y no tendrá ningún impacto en la funcionalidad de cierre de sesión.

>[!NOTE]
>se admite frontchannel_logout **sólo** después de la instalación de [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)

## <a name="client-configuration"></a>Configuración del cliente
Cliente debe implementar una dirección url que se "cierra la sesión' el inicio de sesión de usuario. Administrador puede configurar el LogoutUri en la configuración del cliente con los siguientes cmdlets de PowerShell. 


- `(Add | Set)-AdfsNativeApplication`
- `(Add | Set)-AdfsServerApplication`
- `(Add | Set)-AdfsClient`

```PowerShell
Set-AdfsClient -LogoutUri <url>
```

El `LogoutUri` es la dirección url usada por FS AF para "cerrar" el usuario. Para implementar el `LogoutUri`, las necesidades del cliente para asegurarse de que se borra el estado de autenticación del usuario en la aplicación, por ejemplo, quitar la autenticación de tokens que tiene. AD FS irá a esa dirección URL, con el SID como el parámetro de consulta, de señalización de confianza / aplicación al cerrar la sesión del usuario. 

![](media/ad-fs-logout-openid-connect/adfs_single_logout2.png)


1.  **Token de OAuth con el Id. de sesión**: AD FS incluye el Id. de sesión en el token de OAuth en el momento de la emisión de tokens de id_token. Esto se usará más adelante por AD FS para identificar las cookies SSO pertinentes se limpien para el usuario.
2.  **Usuario inicia el cierre de sesión en App1**: El usuario puede iniciar un cierre de sesión de cualquiera de las aplicaciones que ha iniciado sesión. En este escenario de ejemplo, un usuario inicia un cierre de sesión de App1.
3.  **Aplicación envía la solicitud de cierre de sesión a AD FS**: Después de que el usuario inicia el cierre de sesión, la aplicación envía una solicitud GET a end_session_endpoint de AD FS. La aplicación puede incluir opcionalmente id_token_hint como un parámetro a esta solicitud. Si id_token_hint está presente, AD FS usará junto con el Id. de sesión saber a qué URI el cliente debe redirigirse a después de cierre de sesión (post_logout_redirect_uri).  El post_logout_redirect_uri debe ser un uri válido registrado con AD FS mediante el parámetro RedirectUris.
4.  **AD FS envía cierre de sesión que ha iniciado la sesión de los clientes**: AD FS usa el valor de identificador de sesión para buscar al usuario inicia sesión en los clientes de pertinentes. Los clientes identificados se envían la solicitud en el LogoutUri registrado con AD FS para iniciar un cierre de sesión del cliente.

## <a name="faqs"></a>P+F
**P: ¿** No se ve los parámetros frontchannel_logout_supported y frontchannel_logout_session_supported en el documento de descubrimiento.</br>
**R:** Asegúrese de que tiene [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) instalado en todos los servidores de AD FS. Hacer referencia a un cierre de sesión único en Server 2016 con [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801).

**P: ¿** He configurado el cierre de sesión único como se indica, pero ha iniciado sesión sigue abierta en otros clientes.</br>
**R:** Asegúrese de que `LogoutUri` se establece para todos los clientes, donde es el usuario ha iniciado sesión. Asimismo, AD FS realiza un intento de mejor caso para enviar la solicitud de cierre de sesión en registrado `LogoutUri`. Cliente debe implementar la lógica para controlar la solicitud y tomar medidas al cierre de sesión el usuario de la aplicación.</br>

**P: ¿** ¿Si después de cerrar la sesión, uno de los clientes vuelve a AD FS con un token de actualización válido, AD FS emitirá un token de acceso?</br>
**R:** Sí. Es responsabilidad de la aplicación cliente para quitar artefactos todo autenticados después de que se recibió una solicitud de cierre de sesión en registrado `LogoutUri`.


## <a name="next-steps"></a>Pasos siguientes
[Desarrollo de AD FS](../../ad-fs/AD-FS-Development.md)  
