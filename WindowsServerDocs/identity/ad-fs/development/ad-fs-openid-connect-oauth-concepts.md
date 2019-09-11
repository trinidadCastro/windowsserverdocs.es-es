---
title: AD FS de los conceptos de OpenID Connect/OAuth
description: Obtenga información sobre AD FS conceptos de autenticación moderna.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6a0a1da3dd5c92dff885478c1669bbda5ae07fe5
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867479"
---
# <a name="ad-fs-openid-connectoauth-concepts"></a>AD FS de los conceptos de OpenID Connect/OAuth
Se aplica a AD FS 2016 y versiones posteriores
 
## <a name="modern-authentication-actors"></a>Actores de autenticación moderna 

|Actor| Descripción|
|-----|-----|
|Usuario final|Esta es la entidad de seguridad (usuarios, aplicaciones, servicios y grupos) que necesita tener acceso al recurso.|  
|Cliente|Se trata de la aplicación Web, identificada por su identificador de cliente. El cliente es normalmente la entidad con la que el usuario final interactúa y solicita tokens del servidor de autorización.
|Servidor de autorización/proveedor de identidades (IdP)| Este es el servidor de AD FS. Es responsable de comprobar la identidad de las entidades de seguridad que existen en el directorio de una organización. Emite tokens de seguridad (token de acceso de portador, token de identificador y token de actualización) tras la autenticación correcta de esas entidades de seguridad.
|Servidor de recursos/proveedor de recursos/usuario de confianza| Aquí es donde residen el recurso o los datos. Confía en el servidor de autorización para autenticar y autorizar de forma segura al cliente y usa tokens de acceso de portador para asegurarse de que se puede conceder el acceso a un recurso.

En el siguiente diagrama se proporciona la relación más básica entre los actores:

![Actores de autenticación moderna](media/adfs-modern-auth-concepts/concept1.png)

## <a name="application-types"></a>Tipos de aplicación 
 

|Tipo de aplicación|Descripción|Rol|
|-----|-----|-----|
|Aplicación nativa|A veces denominado **cliente público**, se ha diseñado para ser una aplicación cliente que se ejecuta en un equipo o dispositivo y con el que interactúa el usuario.|Solicita tokens del servidor de autorización (AD FS) para el acceso de los usuarios a los recursos. Envía solicitudes HTTP a recursos protegidos, utilizando los tokens como encabezados HTTP.| 
|Aplicación de servidor (aplicación web)|Una aplicación web que se ejecuta en un servidor y a la que los usuarios suelen tener acceso a través de un explorador. Dado que es capaz de mantener su propia credencial o "secreto" de cliente, a veces se denomina **cliente confidencial**. |Solicita tokens del servidor de autorización (AD FS) para el acceso de los usuarios a los recursos. Antes de solicitar el token, el cliente (aplicación web) debe autenticarse con su secreto. | 
|Web API|El recurso final al que tiene acceso el usuario. Considérelos como la nueva representación de "usuarios de confianza".|Consume tokens de acceso de portador obtenidos por los clientes| 

## <a name="application-group"></a>Grupo de aplicaciones 
 
Cada cliente de OAuth (aplicación web o nativa) o recurso (API Web) configurado con AD FS debe estar asociado a un grupo de aplicaciones. Los clientes de un grupo de aplicaciones se pueden configurar para tener acceso a los recursos del mismo grupo. Un grupo de aplicación puede contener varios clientes y recursos.  

## <a name="security-tokens"></a>Tokens de seguridad 
 
La autenticación moderna usa los siguientes tipos de token: 
- **ID_token**: Un token de JWT emitido por el servidor de autorización (AD FS) y utilizado por el cliente. Las notificaciones del token de identificador contendrán información sobre el usuario para que el cliente pueda usarla.  
- **access_token**: Un token de JWT emitido por el servidor de autorización (AD FS) y destinado a ser utilizado por el recurso. La demanda de "AUD" o audiencia de este token debe coincidir con el identificador del recurso o de la API Web.  
- **refresh_token**: Este es el token emitido por AD FS para que el cliente lo use cuando necesite actualizar ID_token y access_token. El token es opaco para el cliente y solo lo puede consumir AD FS.  

## <a name="scopes"></a>Ámbitos 
 
Al registrar un recurso en AD FS, se pueden configurar ámbitos para permitir que AD FS realice acciones específicas. Además de configurar el ámbito, también se requiere que el valor de ámbito se envíe en la solicitud de AD FS para realizar la acción. Por ejemplo, el administrador debe configurar el ámbito como OpenID durante el registro de recursos y la aplicación (cliente) debe enviar el ámbito = OpenID en la solicitud de autenticación para que AD FS emita el token de identificador. A continuación se proporcionan detalles sobre los ámbitos disponibles en AD FS 
 
- AZA: si se usan  [las extensiones de protocolo de OAuth 2,0 para los clientes de Broker](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapxbc/2f7d8875-0383-4058-956d-2fb216b44706)y si el parámetro de ámbito contiene el ámbito "AZA", el servidor emite un nuevo token de actualización principal y lo establece en el campo refresh_token de la respuesta, así como el establecimiento del parámetro refresh_token_expires_in el campo a la duración del nuevo token de actualización principal si se aplica uno. 
- OpenID: permite que la aplicación solicite el uso del Protocolo de autorización OpenID Connect. 
- logon_cert: el ámbito logon_cert permite a una aplicación solicitar certificados de inicio de sesión, que se pueden usar para iniciar sesión de forma interactiva en usuarios autenticados. El servidor de AD FS omite el parámetro access_token de la respuesta y, en su lugar, proporciona una cadena de certificados CMS codificada en base64 o una respuesta de PKI completa de CMC. Puede encontrar más información [aquí](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-oapx/32ce8878-7d33-4c02-818b-6c9164cc731e).
- user_impersonation: el ámbito user_impersonation es necesario para solicitar correctamente un token de acceso en nombre de AD FS. Para obtener más información sobre cómo usar este ámbito, consulte [creación de una aplicación de varios niveles con on-behalf-of (OBO) mediante OAuth con AD FS 2016](ad-fs-on-behalf-of-authentication-in-windows-server.md). 
- allatclaims: el ámbito allatclaims permite que la aplicación solicite notificaciones en el token de acceso que se van a agregar también en el token de identificador.   
- vpn_cert: el ámbito vpn_cert permite a una aplicación solicitar certificados VPN, que se pueden usar para establecer conexiones VPN mediante la autenticación EAP-TLS. Esto ya no se admite. 
- correo electrónico: permite que la aplicación solicite una solicitud de correo electrónico para el usuario que ha iniciado sesión.  
- Perfil: permite que la aplicación solicite notificaciones relacionadas con el perfil para el usuario de inicio de sesión.  

## <a name="claims"></a>Notificaciones 
 
Los tokens de seguridad (acceso y tokens de identificador) emitidos por AD FS contienen notificaciones o aserciones de información sobre el sujeto que se ha autenticado. Las aplicaciones pueden usar notificaciones para varias tareas, entre las que se incluyen: 
- Validar el token 
- Identificar el inquilino de directorio del sujeto 
- Mostrar información de usuario 
- Determinar la autorización del sujeto las notificaciones presentes en un token de seguridad determinado dependen del tipo de token, el tipo de credencial que se usa para autenticar al usuario y la configuración de la aplicación.  
 
## <a name="high-level-ad-fs-authentication-flow"></a>Flujo de autenticación AD FS de alto nivel 

![AD FS flujo de autenticación](media/adfs-modern-auth-concepts/adfsauthflow.png)


 1. AD FS recibe la solicitud de autenticación del cliente.  
 
 2. AD FS valida el identificador de cliente en la solicitud de autenticación con el identificador de cliente obtenido durante el registro del cliente y del recurso en AD FS. Si usa un cliente confidencial, AD FS también valida el secreto de cliente proporcionado en la solicitud de autenticación. AD FS validar también el URI de redirección del cliente. 
 
 3. AD FS identifica el recurso al que el cliente desea obtener acceso a través del parámetro de recurso pasado en la solicitud de autenticación. Si se usa la biblioteca de cliente de MSAL, no se envía el parámetro de recurso. En su lugar, la dirección URL del recurso se envía como parte del parámetro de ámbito: *Scope = [URL del recurso]//[valores de ámbito, por ejemplo, OpenID]* . 

    Si el recurso no se pasa mediante el parámetro de recurso o de ámbito, ADFS usará un recurso predeterminado urn: Microsoft: UserInfo cuyas directivas (por ejemplo, MFA, emisión o Directiva de autorización) no se pueden configurar. 
 
 4. A continuación AD FS valida si el cliente tiene los permisos para tener acceso al recurso. AD FS también valida si los ámbitos pasados en la solicitud de autenticación coinciden con los ámbitos configurados al registrar el recurso. Si el cliente no tiene los permisos o los ámbitos correctos no se envían en la solicitud de autenticación, el flujo de autenticación se termina.   
 
 5. Una vez validados los permisos y los ámbitos, AD FS autentica al usuario mediante el [método de autenticación](../operations/configure-authentication-policies.md)configurado.   

 6. Si se requiere un [método de autenticación adicional](../operations/configure-additional-authentication-methods-for-ad-fs.md) según la Directiva de recursos o la Directiva de autenticación global, AD FS desencadena la autenticación adicional. 

 7. AD FS usa [Azure MFA](../operations/configure-ad-fs-and-azure-mfa.md) o [MFA](../operations/additional-authentication-methods-ad-fs.md) de terceros para realizar la autenticación.   
 
 8. Una vez autenticado el usuario, AD FS aplica las [reglas de notificación](../deployment/configuring-claim-rules.md) (determina las notificaciones enviadas al recurso como parte de los tokens de seguridad) y las directivas de control de [acceso](../operations/ad-fs-client-access-policies.md) (determina que el usuario ha cumplido las condiciones necesarias para tener acceso al recurso).   

 9. A continuación, AD FS genera los tokens de acceso y actualización. 

 10. AD FS también genera el token de identificador. 
 
 11. Si el ámbito = allatclaims se incluye en la solicitud de autenticación, [se personaliza el token de identificador](custom-id-tokens-in-ad-fs.md) para incluir las notificaciones en el token de acceso en función de las reglas de notificación definidas. 
    
 12. Una vez que se han generado y personalizado los tokens necesarios, AD FS responde al cliente, incluidos los tokens. Solo si la solicitud de autenticación incluye el ámbito = OpenID, el token de identificador se incluye en la respuesta. El cliente siempre puede obtener el token de identificador después de la autenticación mediante el punto de conexión del token. 

## <a name="types-of-libraries"></a>Tipos de bibliotecas 
  
Se usan dos tipos de bibliotecas con AD FS: 
- **Bibliotecas de cliente**: Los clientes nativos y las aplicaciones de servidor usan bibliotecas de cliente para adquirir tokens de acceso para llamar a un recurso, como una API Web. La biblioteca de autenticación de Microsoft (MSAL) es la biblioteca de cliente más reciente y recomendada al usar AD FS 2019. Biblioteca de autenticación de Active Directory (ADAL) se recomienda para AD FS 2016.  

- **Bibliotecas de middleware de servidor**: Web Apps usa bibliotecas de middleware de servidor para el inicio de sesión de usuario. Las API Web usan las bibliotecas de middleware de servidor para validar los tokens enviados por clientes nativos o por otros servidores. OWIN (Open web interface para .NET) es la biblioteca de middleware recomendada. 

## <a name="customize-id-token-additional-claims-in-id-token"></a>Personalizar el token de identificador (notificaciones adicionales en el token de identificador)
 
En algunos escenarios es posible que la aplicación web (cliente) necesite notificaciones adicionales en un token de identificador para ayudar en la funcionalidad. Esto se puede lograr mediante una de las siguientes opciones. 

**Opción 1:** Debe usarse cuando se usa un cliente público y la aplicación web no tiene un recurso al que se está intentando obtener acceso. La opción requiere 
1.  response_mode establecido como form_post 
2.  El identificador del usuario de confianza (identificador de la API Web) es el mismo que el identificador de cliente

![AD FS la opción de token de personalización 1](media/adfs-modern-auth-concepts/option1.png)

**Opción 2:** Debe usarse cuando la aplicación web tiene un recurso al que está intentando acceder y debe pasar notificaciones adicionales mediante el token de identificador. Se pueden usar clientes públicos y confidenciales. La opción requiere 
1.  response_mode establecido como form_post 
2.  KB4019472 está instalado en los servidores de AD FS 
3.  Ámbito allatclaims asignado al par cliente-RP. Puede asignar el ámbito mediante el cmdlet Grant-ADFSApplicationPermission (usar Set-AdfsApplicationPermission si ya se concedió una vez) de PowerShell como se indica en el ejemplo siguiente: 

    ``` powershell
    Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
    ```

![AD FS la opción de token de personalización 2](media/adfs-modern-auth-concepts/option2.png)

Para comprender mejor cómo configurar una aplicación web en ADFS para adquirir un token de identificador personalizado, consulte [Personalización de notificaciones que se emitirán en ID_token al usar OpenID Connect o OAuth con AD FS 2016 o posterior](Custom-Id-Tokens-in-AD-FS.md).

## <a name="single-log-out"></a>Cierre de sesión único

El cierre de sesión único hace que finalice todas las sesiones de cliente con el identificador de sesión. AD FS 2016 y versiones posteriores admiten el cierre de sesión único para OpenID Connect/OAuth. Para obtener más información [, consulte inicio de sesión único para OpenID Connect con AD FS](ad-fs-logout-openid-connect.md).


## <a name="ad-fs-endpoints"></a>AD FS puntos de conexión

|AD FS extremo|Descripción|
|-----|-----|
|/Authorize|AD FS devuelve un código de autorización que se puede usar para obtener el token de acceso|
|/token|AD FS devuelve un token de acceso que se puede usar para tener acceso al recurso (API Web)|
|/userinfo|AD FS devuelve notificaciones sobre el usuario autenticado|
|/devicecode|AD FS devuelve el código del dispositivo y el código de usuario|
|/logout|AD FS cierra la sesión del usuario|
|/keys|AD FS claves públicas usadas para firmar las respuestas|
|/.well-known/openid-configuration|AD FS devuelve los metadatos de OAuth/OpenID Connect|
