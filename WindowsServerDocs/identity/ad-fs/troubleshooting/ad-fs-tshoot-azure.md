---
title: Solución de AD FS - Azure AD
description: Este documento describe cómo solucionar diversos aspectos de AD FS y Azure AD
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6f85c447ac0816c46e07145dbe9a491a29e17c0f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846476"
---
# <a name="ad-fs-troubleshooting---azure-ad"></a>Solución de AD FS - Azure AD
Con el crecimiento de la nube, muchas empresas han migrado a usar Azure AD para sus aplicaciones y servicios distintos.  Federación con Azure AD se ha convertido en una práctica estándar con muchas organizaciones.  Este documento trata algunos de los aspectos de solución de problemas que surgen con esta federación.  Algunos de los temas en el documento de solución de problemas general se refieren todavía para federar con Azure, por lo que este documento se centrará en aspectos específicos solo con Azure AD y la interacción de AD FS.

## <a name="redirection-to-ad-fs"></a>Redirección a AD FS
Redirección produce cuando que inicie sesión en una aplicación, como Office 365 y está "redirigir" a las organizaciones de servidores de AD FS para iniciar sesión.

![](media/ad-fs-tshoot-azure/azure1.png)


### <a name="first-things-to-check"></a>Lo primero que comprobar
Si el redireccionamiento no se está produciendo que hay algunas cosas que desea comprobar

   1. Asegúrese de que el inquilino de Azure AD está habilitado para la federación de inicio de sesión en el portal de Azure y comprobando en Azure AD Connect.

![](media/ad-fs-tshoot-azure/azure2.png)

   2.  Asegúrese de que se debe comprobar el dominio personalizado, haga clic en el dominio al lado de la federación en Azure portal.
![](media/ad-fs-tshoot-azure/azure3.png)

   3. Por último, en el que desea comprobar [DNS](ad-fs-tshoot-dns.md) y asegúrese de que sus servidores de AD FS o servidores WAP se resuelven desde internet.  Compruebe que se resuelve y que puede navegar a él.
   4. También puede usar el cmdlet de PowerShell `Get-AzureADDomain` para obtener esta información también.

![](media/ad-fs-tshoot-azure/azure6.png)

### <a name="you-are-receiving-an-unknown-auth-method-error"></a>Ha recibido un error de método de autenticación desconocido
Puede encontrar un error "Método de autenticación desconocido" que indica que no se admite AuthnContext en el nivel de AD FS o STS cuando se le redirigirá de Azure. 

Esto es muy habitual cuando Azure AD redirige a AD FS o STS con un parámetro que se aplica un método de autenticación. 

Para aplicar un método de autenticación, utilice uno de los métodos siguientes:
- Para WS-Federation, utilice una cadena de consulta WAUTH para forzar un método de autenticación preferido.

- Para SAML2.0, use lo siguiente:
```
<saml:AuthnContext>
<saml:AuthnContextClassRef>
urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport
</saml:AuthnContextClassRef>
</saml:AuthnContext>
```
Cuando el método de autenticación aplicada se envía con un valor incorrecto, o si ese método de autenticación no es compatible con AD FS o STS, recibe un mensaje de error antes de que se autentica.

|Método de autenticación deseada|wauth URI|
|-----|-----|
|Autenticación de nombre de usuario y contraseña|urn:oasis:names:tc:SAML:1.0:am:password|
|Autenticación de cliente SSL|urn:ietf:rfc:2246|
|Autenticación integrada de Windows|urn:federation:authentication:windows|

Clases de contexto de autenticación SAML admitidas

|Método de autenticación|Clase de contexto de autenticación URI|
|-----|-----| 
|Nombre de usuario y contraseña|urn:oasis:names:tc:SAML:2.0:ac:classes:Password|
|Transporte protegido con contraseña|urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport|
|Cliente del transporte de capa (TLS)|urn:oasis:names:tc:SAML:2.0:ac:classes:TLSClient
|Certificado X.509|urn:oasis:names:tc:SAML:2.0:ac:classes:X509
|Autenticación integrada de Windows|urn:federation:authentication:windows|
|Kerberos|urn:oasis:names:tc:SAML:2.0:ac:classes:Kerberos|

Para asegurarse de que se admite el método de autenticación en el nivel de AD FS, compruebe lo siguiente.

#### <a name="ad-fs-20"></a>AD FS 2.0 

En **/adfs/ls/web.config**, asegúrese de que la entrada para el tipo de autenticación está presente.

```
<microsoft.identityServer.web>
<localAuthenticationTypes>
<add name="Forms" page="FormsSignIn.aspx" />
<add name="Integrated" page="auth/integrated/" />
<add name="TlsClient" page="auth/sslclient/" />
<add name="Basic" page="auth/basic/" />
</localAuthenticationTypes>
```

#### <a name="ad-fs-2012-r2"></a>AD FS 2012 R2

En **administración de AD FS**, haga clic en **las directivas de autenticación** en AD FS un complemento.

En el **autenticación principal** sección, haga clic en Editar junto a la configuración Global. Puede también haga clic en las directivas de autenticación y, a continuación, seleccionar Editar autenticación principal Global. O bien, en el panel Acciones, seleccione Editar autenticación principal Global.

En la ventana Editar directiva de autenticación Global, en la ficha principal, puede configurar opciones como parte de la directiva de autenticación global. Por ejemplo, para la autenticación principal, puede seleccionar los métodos de autenticación disponibles en la Intranet y de Extranet.

** Asegurarse que esté seleccionada la casilla de verificación del método de autenticación necesarios. 

#### <a name="ad-fs-2016"></a>AD FS 2016

En **administración de AD FS**, haga clic en **servicio** y **métodos de autenticación** en AD FS un complemento.

En el **autenticación principal** sección, haga clic en Editar.

En el **editar métodos de autenticación** ventana, en la ficha principal, puede configurar opciones como parte de la directiva de autenticación.

![](media/ad-fs-tshoot-azure/azure4.png)

## <a name="tokens-issued-by-ad-fs"></a>Tokens emitidos por AD FS

### <a name="azure-ad-throws-error-after-token-issuance"></a>Azure AD produce error después de la emisión de tokens
Después de que AD FS emita un token, Azure AD puede producir un error. En esta situación, compruebe lo siguiente:
- Las notificaciones emitidas por AD FS en el símbolo (token) deben coincidir con los atributos correspondientes del usuario en Azure AD.
- el token de Azure AD debe contener las siguientes notificaciones:
    - WSFED: 
        - UPN: El valor de esta notificación debe coincidir con el UPN de los usuarios de Azure AD.
        - ImmutableID: El valor de esta notificación debe coincidir con el valor de sourceAnchor o ImmutableID del usuario en Azure AD.

Para obtener el valor del atributo de usuario de Azure AD, ejecute la siguiente línea de comandos: `Get-AzureADUser –UserPrincipalName <UPN>`

![](media/ad-fs-tshoot-azure/azure5.png)

   - SAML 2.0:
       - IDPEmail: El valor de esta notificación debe coincidir con el nombre principal de usuario de los usuarios de Azure AD.
       - NAMEID: El valor de esta notificación debe coincidir con el valor de sourceAnchor o ImmutableID del usuario en Azure AD.

Para obtener más información, consulte [utilizar un proveedor de identidades de SAML 2.0 para implementar el inicio de sesión único en](https://technet.microsoft.com/library/dn641269.aspx).

### <a name="token-signing-certificate-mismatch-between-ad-fs-and-azure-ad"></a>Discrepancia de certificado firma de tokens entre AD FS y Azure AD.

AD FS usa el certificado de firma de tokens para firmar el token que se envía al usuario o la aplicación. La confianza entre el AD FS y Azure AD es una confianza federada que se basa en este certificado de firma de tokens.

Sin embargo, si se cambia el certificado de firma de tokens en el lado de AD FS debido a la sustitución del certificado automático o mediante alguna intervención, los detalles del nuevo certificado deben actualizarse en el lado de Azure AD para el dominio federado. Cuando el certificado de firma de tokens principal en la instancia de AD FS es diferente de anuncios de Azure, Azure AD no confía en el token emitido por AD FS. Por lo tanto, no se permite al usuario federado para iniciar sesión.

Para corregir esto, puede usar los pasos se describen en [renovar certificados de federación para Office 365 y Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs).

## <a name="other-common-things-to-check"></a>Otras comprobaciones habituales
La siguiente es una lista rápida de tareas para comprobar si tiene problemas con la interacción de AD FS y Azure AD.
- obsoletas o almacenados en caché de credenciales en el Administrador de credenciales de Windows
- Algoritmo Hash seguro que está configurada en la confianza de Office 365 se establece en SHA1

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)