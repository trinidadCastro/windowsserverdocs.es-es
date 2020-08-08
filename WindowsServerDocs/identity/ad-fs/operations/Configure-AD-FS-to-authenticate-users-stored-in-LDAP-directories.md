---
ms.assetid: e863ab80-4e4c-48d3-bdaa-31815ef36bae
title: Configuración de AD FS para autenticar a los usuarios almacenados en directorios LDAP
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 95dfeb427aa67bce56c3f87f2c356eff34aac6c4
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962509"
---
# <a name="configure-ad-fs-to-authenticate-users-stored-in-ldap-directories-in-windows-server-2016-or-later"></a>Configurar AD FS para autenticar a los usuarios almacenados en directorios LDAP en Windows Server 2016 o posterior

En el siguiente tema se describe la configuración necesaria para habilitar la infraestructura de AD FS con el fin de autenticar a los usuarios cuyas identidades estén almacenadas en directorios compatibles con el Protocolo ligero de acceso a directorios (LDAP) v3.

En muchas organizaciones, las soluciones de administración de identidades constan de una combinación de directorios LDAP de Active Directory, AD LDS o de terceros. Con la adición de AD FS compatibilidad con la autenticación de usuarios almacenados en directorios compatibles con LDAP v3, puede beneficiarse de todo el conjunto de características AD FS de categoría empresarial, independientemente de dónde se almacenen las identidades de usuario. AD FS admite cualquier directorio compatible con LDAP v3.

> [!NOTE]
> Algunas de las características AD FS incluyen el inicio de sesión único (SSO), la autenticación de dispositivos, las directivas de acceso condicional flexibles, la compatibilidad para trabajar desde cualquier lugar a través de la integración con el proxy de aplicación web y la Federación perfecta con Azure AD que, a su vez, le permite a usted y a los usuarios usar la nube, como Office 365 y otras aplicaciones SaaS.  Para obtener más información, vea [información general sobre servicios de Federación de Active Directory (AD FS)](../ad-fs-overview.md).

Para que AD FS autentique a los usuarios desde un directorio LDAP, debe conectar este directorio LDAP a su granja de AD FS mediante la creación de una **confianza de proveedor de notificaciones local**.  Una confianza de proveedor de notificaciones local es un objeto de confianza que representa un directorio LDAP en la granja de AD FS. Un objeto de confianza de proveedor de notificaciones local consta de una serie de identificadores, nombres y reglas que identifican este directorio LDAP para el servicio de Federación local.

Puede admitir varios directorios LDAP, cada uno con su propia configuración, dentro de la misma granja de AD FS mediante la adición de varias **confianzas de proveedor de notificaciones locales**. Además, AD DS bosques que no son de confianza para el bosque en el que AD FS vive también se pueden modelar como confianzas de proveedor de notificaciones locales. Puede crear confianzas de proveedor de notificaciones locales mediante Windows PowerShell.

Los directorios LDAP (confianzas de proveedores de notificaciones locales) pueden coexistir con directorios de AD (confianzas de proveedor de notificaciones) en el mismo servidor de AD FS, dentro de la misma granja de AD FS, por lo tanto, una única instancia de AD FS es capaz de autenticar y autorizar el acceso a los usuarios que se almacenan en directorios AD y no AD.

Solo se admite la autenticación basada en formularios para autenticar a los usuarios desde directorios LDAP. No se admite la autenticación de Windows integrada y basada en certificados para autenticar a los usuarios en directorios LDAP.

Todos los protocolos de autorización pasivos que son compatibles con AD FS, incluidos SAML, WS-Federation y OAuth, también se admiten para las identidades que se almacenan en directorios LDAP.

También se admite el protocolo de autorización activo de WS-Trust para las identidades que se almacenan en directorios LDAP.

## <a name="configure-ad-fs-to-authenticate-users-stored-in-an-ldap-directory"></a>Configurar AD FS para autenticar a los usuarios almacenados en un directorio LDAP
Para configurar la granja de AD FS para autenticar a los usuarios desde un directorio LDAP, puede completar los pasos siguientes:

1. En primer lugar, configure una conexión a su directorio LDAP con el cmdlet **New-AdfsLdapServerConnection** :

   ```
   $DirectoryCred = Get-Credential
   $vendorDirectory = New-AdfsLdapServerConnection -HostName dirserver -Port 50000 -SslMode None -AuthenticationMethod Basic -Credential $DirectoryCred
   ```

   > [!NOTE]
   > Se recomienda crear un nuevo objeto de conexión para cada servidor LDAP al que desee conectarse. AD FS puede conectarse a varios servidores LDAP de réplica y conmutar por error automáticamente en caso de que un servidor LDAP específico esté inactivo. En ese caso, puede crear un AdfsLdapServerConnection para cada uno de estos servidores LDAP de réplica y, a continuación, agregar la matriz de objetos de conexión mediante el parámetro-**LdapServerConnection** del cmdlet **Add-AdfsLocalClaimsProviderTrust** .

   **Nota:** El intento de usar Get-Credential y escribir el DN y la contraseña que se van a usar para enlazar a una instancia de LDAP podría producir un error debido a los requisitos de la interfaz de usuario para formatos de entrada específicos, por ejemplo, DOMINIO\nombreDeUsuario o user@domain.tld . En su lugar, puede usar el cmdlet ConvertTo-SecureString como se indica a continuación (en el ejemplo siguiente se supone que UID = admin, ou = System como el DN de las credenciales que se van a usar para enlazar a la instancia de LDAP):

   ```
   $ldapuser = ConvertTo-SecureString -string "uid=admin,ou=system" -asplaintext -force
   $DirectoryCred = Get-Credential -username $ldapuser -Message "Enter the credentials to bind to the LDAP instance:"
   ```

   A continuación, escriba la contraseña de UID = admin y complete el resto de los pasos.

2. Después, puede realizar el paso opcional de asignar atributos LDAP a las notificaciones de AD FS existentes con el cmdlet **New-AdfsLdapAttributeToClaimMapping** . En el ejemplo siguiente, se asignan los atributos de CommonName de givenName, apellidos y LDAP a las notificaciones de AD FS:

   ```
   #Map given name claim
   $GivenName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute givenName -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"
   # Map surname claim
   $Surname = New-AdfsLdapAttributeToClaimMapping -LdapAttribute sn -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname"
   # Map common name claim
   $CommonName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute cn -ClaimType "http://schemas.xmlsoap.org/claims/CommonName"
   ```

   Esta asignación se realiza para que los atributos del almacén LDAP estén disponibles como notificaciones en AD FS con el fin de crear reglas de control de acceso condicional en AD FS. También permite que AD FS trabajar con esquemas personalizados en almacenes LDAP al proporcionar una manera sencilla de asignar atributos LDAP a las notificaciones.

3. Por último, debe registrar el almacén LDAP con AD FS como una confianza de proveedor de notificaciones local mediante el cmdlet **Add-AdfsLocalClaimsProviderTrust** :

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

   En el ejemplo anterior, va a crear una confianza de proveedor de notificaciones local denominada "proveedores". Va a especificar la información de conexión para que AD FS se conecte al directorio LDAP que esta confianza del proveedor de notificaciones local representa mediante `$vendorDirectory` la asignación al `-LdapServerConnection` parámetro. Tenga en cuenta que, en el paso uno, ha asignado `$vendorDirectory` una cadena de conexión que se usará al conectarse a su directorio LDAP específico. Por último, está especificando que se `$GivenName` `$Surname` `$CommonName` usarán los atributos LDAP, y (asignados a las notificaciones de AD FS) para el control de acceso condicional, incluidas las directivas de autenticación multifactor y las reglas de autorización de emisión, así como para la emisión a través de notificaciones en los tokens de seguridad emitidos por AD FS. Con el fin de usar protocolos activos como WS-Trust con AD FS, debe especificar el parámetro OrganizationalAccountSuffix, que permite a AD FS eliminar la ambigüedad entre las confianzas de proveedor de notificaciones locales al atender una solicitud de autorización activa.

## <a name="see-also"></a>Consulte también
[Operaciones de AD FS](../ad-fs-operations.md)
