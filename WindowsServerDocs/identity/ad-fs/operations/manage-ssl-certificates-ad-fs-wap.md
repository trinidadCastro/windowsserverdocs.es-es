---
ms.assetid: a3f50046-5d48-43d3-b0f8-ac2346b15285
title: Administración de certificados SSL en AD FS y WAP en Windows Server 2016
description: Administración de certificados SSL en AD FS y WAP en Windows Server 2016
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/02/2017
ms.topic: article
ms.openlocfilehash: cc48e3efc783665921519272443e86620dcd4d4a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962479"
---
# <a name="managing-ssl-certificates-in-ad-fs-and-wap-in-windows-server-2016"></a>Administración de certificados SSL en AD FS y WAP en Windows Server 2016



En este artículo se describe cómo implementar un nuevo certificado SSL en los servidores AD FS y WAP.

>[!NOTE]
>El método recomendado para reemplazar el certificado SSL que se va a avanzar para una granja AD FS es usar Azure AD Connect.  Para obtener más información [, consulte Actualización del certificado SSL para una granja de servicios de Federación de Active Directory (AD FS) (AD FS)](/azure/active-directory/connect/active-directory-aadconnectfed-ssl-update)

## <a name="obtaining-your-ssl-certificates"></a>Obtención de los certificados SSL
En el caso de las granjas de AD FS de producción, se recomienda un certificado SSL de confianza pública. Esto suele obtenerse mediante el envío de una solicitud de firma de certificado (CSR) a un proveedor de certificados público de terceros. Hay varias maneras de generar el CSR, incluido desde un equipo con Windows 7 o una versión posterior. El proveedor debe tener documentación para esto.

- Asegúrese de que el certificado cumple los [requisitos de certificado SSL de AD FS y proxy de aplicación web](../overview/ad-fs-requirements.md#BKMK_1)

### <a name="how-many-certificates-are-needed"></a>Cuántos certificados se necesitan
Se recomienda usar un certificado SSL común en todos los servidores de AD FS y del proxy de aplicación Web. Para conocer los requisitos detallados, consulte los [requisitos de certificado SSL de AD FS y proxy de aplicación web](../overview/ad-fs-requirements.md#BKMK_1)

### <a name="ssl-certificate-requirements"></a>Requisitos del certificado SSL
En cuanto a los requisitos, como la asignación de nombres, la raíz de confianza y las extensiones, consulte los requisitos de los [certificados SSL del proxy de aplicación web y el AD FS](../overview/ad-fs-requirements.md#BKMK_1) documento

## <a name="replacing-the-ssl-certificate-for-ad-fs"></a>Reemplazar el certificado SSL para AD FS
> [!NOTE]
> El certificado SSL de AD FS no es el mismo que el certificado de comunicaciones del servicio AD FS que se encuentra en el complemento Administración de AD FS. Para cambiar el certificado SSL AD FS, debe usar PowerShell.

En primer lugar, determine el modo de enlace de certificados que ejecutan los servidores de AD FS: el enlace de autenticación de certificado predeterminado o el modo de enlace TLS de cliente alternativo.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-default-certificate-authentication-binding-mode"></a>Reemplazar el certificado SSL para AD FS que se ejecuta en el modo de enlace de autenticación de certificado predeterminado
De forma predeterminada, AD FS realiza la autenticación de certificados de dispositivo en el puerto 443 y la autenticación de certificado de usuario en el puerto 49443 (o un puerto configurable que no sea 443).
En este modo, use el cmdlet de PowerShell Set-AdfsSslCertificate para administrar el certificado SSL.

Para hacerlo, siga estos pasos:

1. En primer lugar, tendrá que obtener el nuevo certificado. Esto se suele hacer mediante el envío de una solicitud de firma de certificado (CSR) a un proveedor de certificados público de terceros. Hay varias maneras de generar el CSR, incluido desde un equipo con Windows 7 o una versión posterior. El proveedor debe tener documentación para esto.

    * Asegúrese de que el certificado cumple los [requisitos de certificado SSL de AD FS y proxy de aplicación web](../overview/ad-fs-requirements.md#BKMK_1)

1. Una vez que obtenga la respuesta del proveedor de certificados, impórtela en el almacén del equipo local en cada AD FS y en el servidor proxy de aplicación Web.

1. En el servidor de AD FS **principal** , use el siguiente cmdlet para instalar el nuevo certificado SSL.

```powershell
Set-AdfsSslCertificate -Thumbprint '<thumbprint of new cert>'
```

La huella digital del certificado se puede encontrar ejecutando este comando:

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Notas adicionales

* El cmdlet Set-AdfsSslCertificate es un cmdlet de varios nodos. Esto significa que solo tiene que ejecutarse desde el servidor principal y se actualizarán todos los nodos de la granja. Esto es nuevo en el servidor 2016. En el servidor 2012 R2, tenía que ejecutar Set-AdfsSslCertificate en cada servidor.
* El cmdlet Set-AdfsSslCertificate solo debe ejecutarse en el servidor principal. El servidor principal debe ejecutar el servidor 2016 y el nivel de comportamiento de la granja debe elevarse a 2016.
* El cmdlet Set-AdfsSslCertificate usará la comunicación remota de PowerShell para configurar el resto de servidores de AD FS, asegúrese de que el puerto 5985 (TCP) está abierto en los otros nodos.
* El cmdlet Set-AdfsSslCertificate concederá los permisos de lectura de la entidad de seguridad de adfssrv a las claves privadas del certificado SSL. Esta entidad de seguridad representa el servicio AD FS. No es necesario conceder a la cuenta de servicio de AD FS acceso de lectura a las claves privadas del certificado SSL.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-alternate-tls-binding-mode"></a>Reemplazar el certificado SSL para AD FS que se ejecuta en modo de enlace TLS alternativo
Cuando se configura en el modo de enlace TLS de cliente alternativo, AD FS realiza la autenticación de certificados de dispositivo en el puerto 443 y la autenticación de certificado de usuario en el puerto 443 también en un nombre de host diferente. El nombre de host del certificado de usuario es el AD FS nombre de host precedido de "certauth", por ejemplo, "certauth.fs.contoso.com".
En este modo, use el cmdlet de PowerShell Set-AdfsAlternateTlsClientBinding para administrar el certificado SSL. Esto no solo administrará el enlace TLS de cliente alternativo, sino también todos los demás enlaces en los que AD FS establece el certificado SSL.

Para hacerlo, siga estos pasos:

1. En primer lugar, tendrá que obtener el nuevo certificado. Esto se suele hacer mediante el envío de una solicitud de firma de certificado (CSR) a un proveedor de certificados público de terceros. Hay varias maneras de generar el CSR, incluido desde un equipo con Windows 7 o una versión posterior. El proveedor debe tener documentación para esto.

    * Asegúrese de que el certificado cumple los [requisitos de certificado SSL de AD FS y proxy de aplicación web](../overview/ad-fs-requirements.md#BKMK_1)

1. Una vez que obtenga la respuesta del proveedor de certificados, impórtela en el almacén del equipo local en cada AD FS y en el servidor proxy de aplicación Web.

1. En el servidor de AD FS **principal** , use el siguiente cmdlet para instalar el nuevo certificado SSL.

```powershell
Set-AdfsAlternateTlsClientBinding -Thumbprint '<thumbprint of new cert>'
```

La huella digital del certificado se puede encontrar ejecutando este comando:

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Notas adicionales

* El cmdlet Set-AdfsAlternateTlsClientBinding es un cmdlet de varios nodos. Esto significa que solo tiene que ejecutarse desde el servidor principal y se actualizarán todos los nodos de la granja.
* El cmdlet Set-AdfsAlternateTlsClientBinding solo debe ejecutarse en el servidor principal. El servidor principal debe ejecutar el servidor 2016 y el nivel de comportamiento de la granja debe elevarse a 2016.
* El cmdlet Set-AdfsAlternateTlsClientBinding usará la comunicación remota de PowerShell para configurar el resto de servidores de AD FS, asegúrese de que el puerto 5985 (TCP) está abierto en los otros nodos.
* El cmdlet Set-AdfsAlternateTlsClientBinding concederá los permisos de lectura de la entidad de seguridad de adfssrv a las claves privadas del certificado SSL. Esta entidad de seguridad representa el servicio AD FS. No es necesario conceder a la cuenta de servicio de AD FS acceso de lectura a las claves privadas del certificado SSL.

## <a name="replacing-the-ssl-certificate-for-the-web-application-proxy"></a>Reemplazar el certificado SSL para el proxy de aplicación Web
Para configurar el enlace de autenticación de certificado predeterminado o el modo de enlace TLS de cliente alternativo en el WAP, se puede usar el cmdlet Set-WebApplicationProxySslCertificate.
Para reemplazar el certificado SSL del proxy de aplicación Web, en **cada** servidor proxy de aplicación web use el siguiente cmdlet para instalar el nuevo certificado SSL:

```powershell
Set-WebApplicationProxySslCertificate -Thumbprint '<thumbprint of new cert>'
```

Si se produce un error en el cmdlet anterior porque el certificado anterior ya expiró, vuelva a configurar el proxy con los siguientes cmdlets:

```powershell
$cred = Get-Credential
```

Escriba las credenciales de un usuario de dominio que sea administrador local en el servidor de AD FS

```powershell
Install-WebApplicationProxy -FederationServiceTrustCredential $cred -CertificateThumbprint '<thumbprint of new cert>' -FederationServiceName 'fs.contoso.com'
```

## <a name="additional-references"></a>Referencias adicionales
* [Compatibilidad de AD FS con el enlace de nombre de host alternativo para la autenticación de certificado](../operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [Información de la propiedad de AD FS e especificación de certificados](../technical-reference/AD-FS-and-KeySpec-Property.md)
