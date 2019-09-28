---
title: Solución de problemas de AD FS-Azure AD
description: En este documento se describe cómo solucionar varios aspectos de AD FS y Azure AD
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 293618b3fe2a24caff8fd6b52c5528cc699f93de
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407284"
---
# <a name="ad-fs-troubleshooting---azure-ad"></a>Solución de problemas de AD FS-Azure AD
Con el crecimiento de la nube, una gran cantidad de empresas se han trasladado a usar Azure AD para sus diversas aplicaciones y servicios.  La Federación con Azure AD se ha convertido en una práctica estándar con muchas organizaciones.  En este documento se tratan algunos de los aspectos de la solución de problemas que surgen con esta Federación.  Algunos de los temas del documento general de solución de problemas siguen perteneciendo a la Federación con Azure, por lo que este documento se centrará solo en detalles con Azure AD y AD FS interacción.

## <a name="redirection-to-ad-fs"></a>Redirección a AD FS
La redirección se produce cuando se inicia sesión en una aplicación como Office 365 y se "redirige" a las organizaciones AD FS servidores para iniciar sesión.

![](media/ad-fs-tshoot-azure/azure1.png)


### <a name="first-things-to-check"></a>Primeras cosas que se deben comprobar
Si no se está redireccionando, hay algunas cosas que desea comprobar

   1. Asegúrese de que el inquilino de Azure AD está habilitado para la Federación iniciando sesión en el Azure Portal y comprobando en Azure AD Connect.

![](media/ad-fs-tshoot-azure/azure2.png)

1. Asegúrese de que el dominio personalizado se comprueba haciendo clic en el dominio junto a la Federación en el Azure Portal.
   ![](media/ad-fs-tshoot-azure/azure3.png)

2. Por último, desea comprobar [DNS](ad-fs-tshoot-dns.md) y asegurarse de que los servidores de AD FS o los servidores WAP se resuelven desde Internet.  Compruebe que esto se resuelve y que se puede navegar a él.
3. También puede usar el cmdlt de PowerShell `Get-AzureADDomain` para obtener esta información también.

![](media/ad-fs-tshoot-azure/azure6.png)

### <a name="you-are-receiving-an-unknown-auth-method-error"></a>Recibe un error de método de autenticación desconocido
Puede encontrar un error de "método de autenticación desconocido" que indica que AuthnContext no se admite en el nivel de AD FS o STS cuando se le redirige desde Azure. 

Esto es muy habitual cuando Azure AD redirige al AD FS o STS mediante un parámetro que aplica un método de autenticación. 

Para aplicar un método de autenticación, use uno de los métodos siguientes:
- En el caso de WS-Federation, utilice una cadena de consulta WAUTH para forzar un método de autenticación preferido.

- Para SAML 2.0, use lo siguiente:
  ```
  <saml:AuthnContext>
  <saml:AuthnContextClassRef>
  urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport
  </saml:AuthnContextClassRef>
  </saml:AuthnContext>
  ```
  Cuando el método de autenticación aplicado se envía con un valor incorrecto, o si el método de autenticación no se admite en AD FS o STS, recibirá un mensaje de error antes de autenticarse.

|Método de autenticación deseado|URI de wauth|
|-----|-----|
|Autenticación de nombre de usuario y contraseña|urn: oasis: names: TC: SAML: 1.0: AM: password|
|Autenticación de cliente SSL|urn:ietf:rfc:2246|
|Autenticación integrada de Windows|urn: Federation: autenticación: Windows|

Clases de contexto de autenticación SAML admitidas

|Método de autenticación|URI de clase de contexto de autenticación|
|-----|-----| 
|Nombre de usuario y contraseña|urn: oasis: names: TC: SAML: 2.0: AC: classes: password|
|Transporte protegido por contraseña|urn: oasis: names: TC: SAML: 2.0: AC: classes: PasswordProtectedTransport|
|Cliente de seguridad de la capa de transporte (TLS)|urn: oasis: names: TC: SAML: 2.0: AC: classes: TLSClient
|Certificado X. 509|urn: oasis: names: TC: SAML: 2.0: AC: classes: X509
|Autenticación integrada de Windows|urn: Federation: autenticación: Windows|
|Kerberos|urn: oasis: names: TC: SAML: 2.0: AC: classes: Kerberos|

Para asegurarse de que se admite el método de autenticación en el nivel de AD FS, compruebe lo siguiente.

#### <a name="ad-fs-20"></a>AD FS 2,0 

En **/ADFS/LS/Web.config**, asegúrese de que la entrada para el tipo de autenticación está presente.

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

En **Administración de AD FS**, haga clic en **directivas de autenticación** en el complemento AD FS.

En la sección **autenticación principal** , haga clic en Editar junto a configuración global. También puede hacer clic con el botón derecho en directivas de autenticación y seleccionar Editar autenticación principal global. O bien, en el panel acciones, seleccione Editar autenticación principal global.

En la ventana Editar Directiva de autenticación global, en la pestaña principal, puede configurar las opciones como parte de la Directiva de autenticación global. Por ejemplo, para la autenticación principal, puede seleccionar métodos de autenticación disponibles en extranet y intranet.

\* * Asegúrese de que la casilla de verificación método de autenticación necesario está activada. 

#### <a name="ad-fs-2016"></a>AD FS 2016

En **Administración de AD FS**, haga clic en métodos de **servicio** y **autenticación** en el complemento AD FS.

En la sección **autenticación principal** , haga clic en Editar.

En la ventana **Editar métodos de autenticación** , en la pestaña principal, puede configurar las opciones como parte de la Directiva de autenticación.

![](media/ad-fs-tshoot-azure/azure4.png)

## <a name="tokens-issued-by-ad-fs"></a>Tokens emitidos por AD FS

### <a name="azure-ad-throws-error-after-token-issuance"></a>Azure AD produce un error después de la emisión del token
Una vez que AD FS emite un token, Azure AD puede producir un error. En esta situación, compruebe los siguientes problemas:
- Las notificaciones emitidas por AD FS en token deben coincidir con los atributos respectivos del usuario en Azure AD.
- el token de Azure AD debe contener las siguientes notificaciones requeridas:
    - WSFED 
        - UPN El valor de esta demanda debe coincidir con el UPN de los usuarios en Azure AD.
        - ImmutableID El valor de esta demanda debe coincidir con el sourceAnchor o ImmutableID del usuario en Azure AD.

Para obtener el valor de atributo de usuario en Azure AD, ejecute la siguiente línea de comandos: `Get-AzureADUser –UserPrincipalName <UPN>`

![](media/ad-fs-tshoot-azure/azure5.png)

   - SAML 2,0:
       - IDPEmail El valor de esta demanda debe coincidir con el nombre principal de usuario de los usuarios de Azure AD.
       - NAMEID El valor de esta demanda debe coincidir con el sourceAnchor o ImmutableID del usuario en Azure AD.

Para obtener más información, consulte [uso de un proveedor de identidad SAML 2,0 para implementar el inicio de sesión único](https://technet.microsoft.com/library/dn641269.aspx).

### <a name="token-signing-certificate-mismatch-between-ad-fs-and-azure-ad"></a>Los certificados de firma de tokens no coinciden entre AD FS y Azure AD.

AD FS usa el certificado de firma de tokens para firmar el token que se envía al usuario o a la aplicación. La confianza entre el AD FS y el Azure AD es una confianza federada que se basa en este certificado de firma de tokens.

Sin embargo, si se cambia el certificado de firma de tokens del lado AD FS debido a la sustitución automática del certificado o por alguna intervención, los detalles del nuevo certificado deben actualizarse en el Azure AD lado del dominio federado. Cuando el certificado de firma de tokens principal del AD FS es diferente de los anuncios de Azure, el token que emite AD FS no es de confianza para Azure AD. Por lo tanto, no se permite que el usuario federado inicie sesión.

Para solucionarlo, puede usar los pasos que se describen en [renovar certificados de Federación para Office 365 y Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs).

## <a name="other-common-things-to-check"></a>Otros aspectos comunes que se deben comprobar
A continuación se muestra una lista rápida de aspectos que se deben comprobar si tiene problemas con AD FS y Azure AD interacción.
- credenciales obsoletas o almacenadas en caché en el administrador de credenciales de Windows
- El algoritmo hash seguro que está configurado en la relación de confianza para usuario autenticado para Office 365 está establecido en SHA1

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)