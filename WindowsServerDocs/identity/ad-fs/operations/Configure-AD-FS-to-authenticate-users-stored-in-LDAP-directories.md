---
ms.assetid: e863ab80-4e4c-48d3-bdaa-31815ef36bae
title: Configurar AD FS para autenticar usuarios almacenados en directorios LDAP
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 05f8b8991e664a84c3f2b3200de4068af8d1476a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="configure-ad-fs-to-authenticate-users-stored-in-ldap-directories"></a>Configurar AD FS para autenticar usuarios almacenados en directorios LDAP

>Se aplica a: Windows Server 2016

Este tema describe la configuración necesaria para habilitar la infraestructura de AD FS autenticar usuarios cuyas identidades se almacenan en directorios compatibles con v3 de protocolo ligero de acceso a directorios (LDAP).

En muchas organizaciones, soluciones de administración de identidades constan de una combinación de Active Directory, AD LDS o directorios LDAP de terceros. Con la adición de compatibilidad de AD FS para autenticar a los usuarios almacenados en directorios compatibles con v3 LDAP, puede beneficiarse de la empresarial completa conjunto independientemente de dónde se almacenan las identidades de usuario de características de AD FS. AD FS admite cualquier directorio compatible con v3 LDAP.

> [!NOTE]
> Algunas de las características de AD FS incluyen el inicio de sesión único (SSO), autenticación de dispositivo, directivas de acceso condicional flexible, la compatibilidad con el trabajo-de-en cualquier lugar mediante la integración con el Proxy de aplicación Web y federación transparente con Azure AD que a su vez permite y a los usuarios usar la nube, como Office 365 y otras aplicaciones SaaS.  Para obtener más información, consulta [introducción de servicios de federación de Active Directory ](../../ad-fs/AD-FS-2016-Overview.md).

En el orden de AD FS autenticar usuarios desde un directorio LDAP, debes conectar este directorio LDAP al conjunto de AD FS mediante la creación de un **reclamaciones local proveedor confianza **.  Una confianza de proveedor de reclamaciones local es un objeto de confianza que representa un directorio LDAP en el conjunto de AD FS. Las notificaciones de una variable local objeto consta de una variedad de identificadores, nombres y las reglas que identifican este directorio LDAP al servicio de federación local de confianza de proveedor.

Puedes admitir varios directorios LDAP, cada uno con su propia configuración, en el mismo conjunto de AD FS si agregas varios **confianzas de proveedor de notificaciones locales **. Además, también pueden modelar bosques de AD DS que no son de confianza para el bosque de AD FS vive en como confianzas de proveedor de notificaciones locales. Puedes crear confianzas de proveedor de notificaciones locales mediante Windows PowerShell.

Directorios LDAP (confianzas de proveedor de notificaciones locales) pueden coexistir con directorios de anuncios (reclamaciones confianzas de proveedor) en el mismo servidor de AD FS dentro del mismo conjunto de AD FS, por lo tanto, una sola instancia de AD FS es capaz de autenticar y autorizar el acceso a los usuarios que se almacenan en los anuncios y directorios distinta de AD.

Se admite únicamente la autenticación basada en formularios para autenticar a los usuarios desde directorios LDAP. No se admiten la autenticación de Windows integrado y basada en certificados para autenticar usuarios en directorios LDAP.

Todos los protocolos de autorización pasivo que son compatibles con AD FS, incluida SAML, WS-federación y OAuth son también compatibles con identidades que están almacenadas en directorios LDAP.

También se admite el protocolo de autorización activo de WS-Trust para identidades que están almacenadas en directorios LDAP.

## <a name="configure-ad-fs-to-authenticate-users-stored-in-an-ldap-directory"></a>Configurar AD FS para autenticar a los usuarios almacenados en un directorio LDAP
Para configurar el conjunto de AD FS para autenticar a los usuarios desde un directorio LDAP, puede completar los pasos siguientes:

1.  En primer lugar, configurar una conexión al directorio LDAP con los **nueva AdfsLdapServerConnection** cmdlet:

    ```
    $DirectoryCred = Get-Credential
    $vendorDirectory = New-AdfsLdapServerConnection -HostName dirserver -Port 50000 -SslMode None -AuthenticationMethod Basic -Credential $DirectoryCred
    ```

    > [!NOTE]
    > Se recomienda que crees un nuevo objeto de conexión para cada servidor LDAP que desee conectarse. AD FS puede conectarse a varios servidores LDAP de réplica y conmutar automáticamente en caso de un servidor LDAP específico está inactivo. Para este caso, puedes crear un AdfsLdapServerConnection para cada uno de estos servidores LDAP de réplica y, a continuación, agrega la matriz de objetos de conexión mediante la -**LdapServerConnection** parámetro de la **agregar AdfsLocalClaimsProviderTrust** cmdlet.

    **Nota:** podría producirá un error al intentar usar Get-Credential y escribe un nombre completo y la contraseña que se usará para enlazar a una instancia LDAP porque el del requisito de la interfaz de usuario para formatos de entrada específicos, por ejemplo, DOMINIO\nombre de usuario o user@domain.tld. En su lugar, puede usar el cmdlet ConvertTo-SecureString manera (el siguiente ejemplo se da por hecho uid = admin, unidad organizativa = sistema como el nombre completo de las credenciales para usarse para enlazar a la instancia LDAP):

    ```
    $ldapuser = ConvertTo-SecureString -string "uid=admin,ou=system" -asplaintext -force
    $DirectoryCred = Get-Credential -username $ldapuser -Message "Enter the credentials to bind to the LDAP instance:"
    ```

    A continuación, escriba la contraseña para el uid = admin y completar el resto de los pasos.

2.  A continuación, puedes realizar el paso opcional de asignación de atributos LDAP a las notificaciones de AD FS existentes mediante la **nueva AdfsLdapAttributeToClaimMapping** cmdlet. En el ejemplo siguiente, que asignar givenName, apellido, y CommonName LDAP atributos a las notificaciones de AD FS:

    ```
    #Map given name claim
    $GivenName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute givenName -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"
    # Map surname claim
    $Surname = New-AdfsLdapAttributeToClaimMapping -LdapAttribute sn -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname"
    # Map common name claim
    $CommonName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute cn -ClaimType "http://schemas.xmlsoap.org/claims/CommonName"
    ```

    Esta asignación se realiza con el fin de que esté disponible como notificaciones de AD FS para crear reglas de control de acceso condicional en ADFS los atributos de la tienda LDAP. También permite AD FS trabajar con esquemas personalizados en las tiendas LDAP proporcionando una manera sencilla de asignar atributos LDAP reclamaciones.

3.  Por último, debes registrar la tienda LDAP con AD FS como un lugareño dice proveedor confianza usando el **agregar AdfsLocalClaimsProviderTrust** cmdlet:

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

    En el ejemplo anterior, se crea una confianza de proveedor de reclamaciones local denominada "Proveedores". Especifica la información de conexión de AD FS conectar con el directorio LDAP esta confianza de proveedor de notificaciones locales se representa mediante la asignación de `$vendorDirectory`a la `-LdapServerConnection`parámetro. Ten en cuenta que en el paso 1, que hayas asignado `$vendorDirectory`una cadena de conexión que se usará al conectarse a tu directorio LDAP específico. Por último, se especifica que la `$GivenName`, `$Surname`, y `$CommonName`son atributos LDAP (que asignan a las notificaciones de AD FS) que se usará para el control de acceso condicional, incluidas las directivas de la autenticación multifactor y reglas de autorización de emisión, así como para emisión a través de notificaciones de tokens de seguridad emitido por FS de AD. Para usar activas protocolos como Ws-Trust con AD FS, debes especificar el parámetro OrganizationalAccountSuffix, lo que permite AD FS eliminar la ambigüedad entre confianzas de proveedor local notificaciones cuando el mantenimiento de una solicitud de autorización activo.

## <a name="see-also"></a>Consulta también
[AD FS operaciones](../../ad-fs/AD-FS-2016-Operations.md)


