---
ms.assetid: e863ab80-4e4c-48d3-bdaa-31815ef36bae
title: Configuración de AD FS para autenticar a los usuarios almacenados en directorios LDAP
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2053f0a93f33cdfdd85eec8cdbb6eca4ebad1ff0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444920"
---
# <a name="configure-ad-fs-to-authenticate-users-stored-in-ldap-directories"></a>Configuración de AD FS para autenticar a los usuarios almacenados en directorios LDAP

El siguiente tema describe la configuración necesaria para habilitar la infraestructura de AD FS autenticar a los usuarios cuyas identidades se almacenan en directorios de protocolo ligero de acceso a directorios (LDAP) compatibles con v3.

En muchas organizaciones, las soluciones de administración de identidades constan de una combinación de Active Directory, AD LDS o directorios LDAP de terceros. Con la adición de compatibilidad de AD FS para autenticar a los usuarios almacenados en directorios LDAP v3 compatible, puede beneficiarse de todo el nivel empresarial conjunto independientemente de dónde se almacenan las identidades de usuario de características de AD FS. AD FS admite cualquier directorio compatible con v3 LDAP.

> [!NOTE]
> Algunas de las características de AD FS incluye sesión único (SSO), autenticación de dispositivos, directivas de acceso condicional flexible, compatibilidad con trabajo-de-en cualquier lugar mediante la integración con el Proxy de aplicación Web y sin problemas la federación con Azure AD que a su vez permite a usted y sus usuarios usar la nube, incluidos Office 365 y otras aplicaciones SaaS.  Para obtener más información, consulte [Introducción a servicios de federación de Active Directory](../../ad-fs/AD-FS-2016-Overview.md).

En el orden de AD FS autenticar a los usuarios desde un directorio LDAP, debe conectarse este directorio LDAP para la granja de AD FS mediante la creación de un **confianza de proveedor de notificaciones local**.  Una confianza de proveedor de notificaciones local es un objeto de confianza que representa un directorio LDAP en la granja de AD FS. Una variable local de notificaciones de confianza de proveedor de objeto consta de una serie de identificadores, nombres y las reglas que identifican este directorio LDAP para el servicio de federación local.

Puede admitir varios directorios LDAP, cada uno con su propia configuración, en la misma granja de AD FS si agrega varios **confianzas de proveedores de notificaciones local**. Además, los bosques de AD DS que no confía en el bosque de AD FS reside en también se pueden modelar como relaciones de confianza para proveedor de notificaciones locales. Puede crear confianzas de proveedores de notificaciones local mediante Windows PowerShell.

Directorios LDAP (confianzas de proveedor de notificaciones local) pueden coexistir con directorios de AD (confianzas de proveedor de notificaciones) en el mismo servidor de AD FS, en la misma granja de AD FS, por lo tanto, es capaz de autenticar y autorizar el acceso para usuarios que forman una sola instancia de AD FS almacenado en ambos AD y que no son de AD directorios.

Se admite solo la autenticación basada en formularios para autenticar a los usuarios de directorios LDAP. No se admiten la autenticación de Windows integrada y basada en certificados para autenticar a los usuarios en directorios LDAP.

Todos los protocolos de autorización pasivo que son compatibles con AD FS, como SAML, WS-Federation y OAuth también son compatibles con las identidades que se almacenan en directorios LDAP.

También se admite el protocolo de autorización activo de WS-Trust para las identidades que se almacenan en directorios LDAP.

## <a name="configure-ad-fs-to-authenticate-users-stored-in-an-ldap-directory"></a>Configurar AD FS para autenticar a los usuarios almacenados en un directorio LDAP
Para configurar la granja de AD FS para autenticar a los usuarios desde un directorio LDAP, puede completar los pasos siguientes:

1. En primer lugar, configure una conexión a su directorio LDAP con el **New AdfsLdapServerConnection** cmdlet:

   ```
   $DirectoryCred = Get-Credential
   $vendorDirectory = New-AdfsLdapServerConnection -HostName dirserver -Port 50000 -SslMode None -AuthenticationMethod Basic -Credential $DirectoryCred
   ```

   > [!NOTE]
   > Se recomienda que cree un nuevo objeto de conexión para cada servidor LDAP que desea conectarse. AD FS puede conectarse a varios servidores de réplica LDAP y conmutar por error automáticamente en caso de un servidor LDAP específico está inactivo. Para este caso, puede crear uno AdfsLdapServerConnection para cada uno de estos servidores LDAP de réplica y, a continuación, agregar la matriz de objetos de conexión mediante-**LdapServerConnection** parámetro de la  **AdfsLocalClaimsProviderTrust agregar** cmdlet.

   **NOTA:** Podría producir un error al intentar usar Get-Credential y escriba un nombre completo y una contraseña que se utilizará para enlazar a una instancia LDAP porque del requisito de interfaz de usuario para formatos de entrada concretos, por ejemplo, DOMINIO\nombreDeUsuario o user@domain.tld. En su lugar, puede utilizar el cmdlet ConvertTo-SecureString como sigue (en el ejemplo siguiente se da por supuesto uid = admin, ou = sistema, como el DN de las credenciales que se utilizará para enlazar a la instancia LDAP):

   ```
   $ldapuser = ConvertTo-SecureString -string "uid=admin,ou=system" -asplaintext -force
   $DirectoryCred = Get-Credential -username $ldapuser -Message "Enter the credentials to bind to the LDAP instance:"
   ```

   A continuación, escriba la contraseña para el uid = admin y complete el resto de los pasos.

2. A continuación, puede realizar el paso opcional de asignación de atributos LDAP a las notificaciones de AD FS existentes mediante el **New AdfsLdapAttributeToClaimMapping** cmdlet. En el ejemplo siguiente, se asigna givenName, apellido, y CommonName LDAP atributos a las notificaciones de AD FS:

   ```
   #Map given name claim
   $GivenName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute givenName -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"
   # Map surname claim
   $Surname = New-AdfsLdapAttributeToClaimMapping -LdapAttribute sn -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname"
   # Map common name claim
   $CommonName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute cn -ClaimType "http://schemas.xmlsoap.org/claims/CommonName"
   ```

   Esta asignación se realiza con el fin de disponer de los atributos de la tienda LDAP como notificaciones en AD FS con el fin de crear reglas de control de acceso condicional en AD FS. También permite trabajar con esquemas personalizados en los almacenes de LDAP al proporcionar una manera fácil para asignar atributos LDAP a notificaciones de AD FS.

3. Por último, debe registrar el almacén LDAP con AD FS como una variable local de notificaciones de confianza de proveedor mediante el **agregar AdfsLocalClaimsProviderTrust** cmdlet:

   ```
   Add-AdfsLocalClaimsProviderTrust -Name "Vendors" -Identifier "urn:vendors" -Type Ldap

   # Connection info
   -LdapServerConnection $vendorDirectory 

   # How to locate user objects in directory
   -UserObjectClass inetOrgPerson -UserContainer "CN=VendorsContainer,CN=VendorsPartition" -LdapAuthenticationMethod Basic 

   # Claims for authenticated users
   -AnchorClaimLdapAttribute mail -AnchorClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" -LdapAttributeToClaimMapping @($GivenName, $Surname, $CommonName) 

   # General claims provider properties
   -AcceptanceTransformRules "c:[Type != ''] => issue(claim=c);" -Enabled $true 

   # Optional - supply user name suffix if you want to use Ws-Trust
   -OrganizationalAccountSuffix "vendors.contoso.com"
   ```

   En el ejemplo anterior, se crea una confianza de proveedor de notificaciones local denominada "Proveedores". Está especificando información de conexión de AD FS para conectarse con el directorio LDAP que representa esta relación de confianza de proveedor de notificaciones local mediante la asignación `$vendorDirectory` a la `-LdapServerConnection` parámetro. Tenga en cuenta que en el paso uno, se ha asignado `$vendorDirectory` una cadena de conexión que se usará al conectarse a su directorio LDAP específico. Por último, está especificando que el `$GivenName`, `$Surname`, y `$CommonName` son atributos LDAP (que se asignan a las notificaciones de AD FS) que se usará para el control de acceso condicional, incluidas las directivas de autenticación multifactor y de emisión reglas de autorización, así como para la emisión de notificaciones de AD FS emitidas en tokens de seguridad. Para usar activos protocolos como Ws-Trust con AD FS, debe especificar el parámetro OrganizationalAccountSuffix, que permite que AD FS eliminar la ambigüedad entre los confianzas de proveedores de notificaciones local al atender una solicitud de autorización active.

## <a name="see-also"></a>Vea también
[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md)


