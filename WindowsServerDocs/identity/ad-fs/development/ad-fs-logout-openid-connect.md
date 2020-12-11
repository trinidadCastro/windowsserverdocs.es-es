---
description: 'Más información sobre: el cierre de sesión único para OpenID Connect con AD FS'
title: Cierre de sesión único de OpenID Connect con AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/17/2017
ms.topic: article
ms.openlocfilehash: 4b94ddda582c5bb51cf8b6fe987e039ee3e55957
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047023"
---
#  <a name="single-log-out-for-openid-connect-with-ad-fs"></a>Cierre de sesión único de OpenID Connect con AD FS

## <a name="overview"></a>Información general
Basándose en la compatibilidad inicial de OAuth en AD FS en Windows Server 2012 R2, AD FS 2016 presentó la compatibilidad con el inicio de sesión de OpenId Connect. Con [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801), AD FS 2016 ahora admite el cierre de sesión único para escenarios de OpenID Connect. En este artículo se proporciona información general sobre el cierre de sesión único para el escenario de OpenId Connect y se proporcionan instrucciones sobre cómo usarlo para las aplicaciones de OpenId Connect en AD FS.


## <a name="discovery-doc"></a>Documento de detección
OpenID Connect usa un documento JSON denominado "documento de descubrimiento" para proporcionar detalles acerca de la configuración.  Esto incluye los URI de la autenticación, el token, la UserInfo y los puntos de conexión públicos.  El siguiente es un ejemplo del documento de detección.

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



Los siguientes valores adicionales estarán disponibles en el documento de detección para indicar la compatibilidad con el cierre de sesión del canal delantero:

- frontchannel_logout_supported: el valor será ' true '
- frontchannel_logout_session_supported: el valor será ' true '.
- end_session_endpoint: este es el URI de cierre de sesión de OAuth que el cliente puede usar para iniciar el cierre de sesión en el servidor.


## <a name="ad-fs-server-configuration"></a>Configuración del servidor de AD FS
La propiedad AD FS EnableOAuthLogout se habilitará de forma predeterminada.  Esta propiedad indica al servidor de AD FS que busque la dirección URL (LogoutURI) con el SID para iniciar el cierre de sesión en el cliente.
Si no tiene [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) instalado, puede usar el siguiente comando de PowerShell:

```PowerShell
Set-ADFSProperties -EnableOAuthLogout $true
```

>[!NOTE]
> `EnableOAuthLogout` el parámetro se marcará como obsoleto después de instalar [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801). `EnableOAUthLogout` siempre será true y no afectará a la funcionalidad de cierre de sesión.

>[!NOTE]
>frontchannel_logout **solo** se admite después de instalación de [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)

## <a name="client-configuration"></a>Configuración de cliente
El cliente debe implementar una dirección URL que "cierre la sesión" del usuario que ha iniciado sesión. El administrador puede configurar LogoutUri en la configuración del cliente mediante los siguientes cmdlets de PowerShell.


- `(Add | Set)-AdfsNativeApplication`
- `(Add | Set)-AdfsServerApplication`
- `(Add | Set)-AdfsClient`

```PowerShell
Set-AdfsClient -LogoutUri <url>
```

`LogoutUri`Es la dirección URL que usa AF FS para "cerrar la sesión" del usuario. Para implementar `LogoutUri` , el cliente debe asegurarse de que borra el estado de autenticación del usuario en la aplicación, por ejemplo, quitando los tokens de autenticación que tiene. AD FS examinará la dirección URL, con el SID como parámetro de consulta, señalando al usuario de confianza o a la aplicación para que cierre la sesión del usuario.

![Diagrama de usuario de cierre de sesión de ADFS](media/ad-fs-logout-openid-connect/adfs_single_logout2.png)

1.  **Token de OAuth con ID. de sesión**: AD FS incluye el identificador de sesión en el token de OAuth en el momento de la emisión de tokens de ID_token. Lo usará más adelante AD FS para identificar las cookies de SSO pertinentes que se limpiarán para el usuario.
2.  El **usuario inicia el cierre de sesión en app1**: el usuario puede iniciar un cierre de sesión de cualquiera de las aplicaciones conectadas. En este escenario de ejemplo, un usuario inicia un cierre de sesión de app1.
3.  La **aplicación envía una solicitud de cierre de sesión a AD FS**: una vez que el usuario inicia el cierre de sesión, la aplicación envía una solicitud GET a end_session_endpoint de AD FS. Opcionalmente, la aplicación puede incluir id_token_hint como un parámetro para esta solicitud. Si id_token_hint está presente, AD FS lo utilizará junto con el identificador de sesión para averiguar a qué URI debe redirigirse el cliente después del cierre de sesión (post_logout_redirect_uri).  El post_logout_redirect_uri debe ser un URI válido registrado con AD FS mediante el parámetro RedirectUris.
4.  **AD FS envía el cierre de sesión a los clientes que han iniciado** sesión: AD FS usa el valor de identificador de sesión para encontrar los clientes relevantes en los que el usuario ha iniciado sesión. Los clientes identificados envían la solicitud en el LogoutUri registrado con AD FS para iniciar un cierre de sesión en el lado cliente.

## <a name="faqs"></a>Preguntas más frecuentes
**P:** No veo los parámetros frontchannel_logout_supported y frontchannel_logout_session_supported en el documento de detección.</br>
**R:** Asegúrese de que ha instalado [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) en todos los servidores AD FS. Consulte el cierre de sesión único en el servidor 2016 con [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801).

**P:** He configurado el cierre de sesión único como se indicó, pero el usuario permanece registrado en otros clientes.</br>
**R:** Asegúrese de que `LogoutUri` está configurado para todos los clientes en los que el usuario ha iniciado sesión. Además, AD FS realiza el mejor intento de enviar la solicitud de cierre de sesión en el registro `LogoutUri` . El cliente debe implementar la lógica para controlar la solicitud y tomar medidas para cerrar la sesión del usuario de la aplicación.</br>

**P:** Si después del cierre de sesión, uno de los clientes vuelve a AD FS con un token de actualización válido, ¿AD FS emitir un token de acceso?</br>
**R:** Sí. Es responsabilidad de la aplicación cliente quitar todos los artefactos autenticados después de que se haya recibido una solicitud de cierre de sesión en el registro `LogoutUri` .


## <a name="next-steps"></a>Pasos siguientes
[Desarrollo de AD FS](../../ad-fs/AD-FS-Development.md)
