---
ms.assetid: a3f50046-5d48-43d3-b0f8-ac2346b15285
title: "Administración de certificados SSL en AD FS y WAP en Windows Server 2016"
description: "Administración de certificados SSL en AD FS y WAP en Windows Server 2016"
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/02/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5156b3ad357ab8edb5e08a89a459beaf9b8c9b1a
ms.sourcegitcommit: ca7dc3d56a33924ae5fe0e9ffb1274da6dc4e54d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2017
---
# <a name="managing-ssl-certificates-in-ad-fs-and-wap-in-windows-server-2016"></a>Administración de certificados SSL en AD FS y WAP en Windows Server 2016

>Se aplica a: Windows Server 2016

En este artículo se describe cómo implementar un nuevo certificado SSL en los servidores de AD FS y WAP.

>[!NOTE]
>La forma recomendada para reemplazar el certificado SSL en el futuro para una granja de AD FS es usar Azure AD Connect.  Para obtener más información, consulta [actualizar el certificado SSL para un conjunto de servicios de federación de Active Directory (AD FS)](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectfed-ssl-update)

## <a name="obtaining-your-ssl-certificates"></a>Obtener los certificados SSL
Para entornos de producción AD FS se recomienda un certificado SSL públicamente de confianza. Por lo general, esto se logra mediante el envío de una solicitud de firma de certificado (CSR) a un tercero, el proveedor de certificado público. Hay varias maneras para generar el CSR, incluso desde Windows 7 o posterior PC. Tu proveedor debe tener documentación para esto.

- Asegúrate de que el certificado cumple el [AD FS y SSL de Proxy de aplicación Web de requisitos de certificados](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="how-many-certificates-are-needed"></a>Se necesitan cuántas certificados
Se recomienda que uses un certificado SSL comunes en los servidores de todos los AD FS y Proxy de aplicación Web. Para requisitos detallados, consulta el documento [AD FS y SSL de Proxy de aplicación Web de requisitos de certificados](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="ssl-certificate-requirements"></a>Requisitos de certificados SSL
Para establecer los requisitos incluidos nombres raíz de confianza y extensiones de consulta el documento [AD FS y SSL de Proxy de aplicación Web de requisitos de certificados](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

## <a name="replacing-the-ssl-certificate-for-ad-fs"></a>Reemplazar el certificado SSL de AD FS
> [!NOTE]
> El certificado SSL de AD FS no es lo mismo que el certificado de comunicaciones de servicio de AD FS se encuentra en el complemento de administración de AD FS. Para cambiar el certificado SSL de AD FS, tendrás que usar PowerShell.

En primer lugar, determina qué certificado modo de enlace que se ejecutan los servidores de AD FS: enlace de autenticación de certificado predeterminado o el modo de enlace TLS cliente alternativo.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-default-certificate-authentication-binding-mode"></a>Reemplazar el certificado SSL de AD FS ejecutando de forma predeterminada en modo de enlace de autenticación de certificado
AD FS de manera predeterminada realiza la autenticación de certificado de dispositivo en el puerto 443 y autenticación de certificado de usuario en el puerto 49443 (o un puerto configurable que no es 443).
En este modo, usa el cmdlet de powershell conjunto AdfsSslCertificate para administrar el certificado SSL.

Sigue los pasos siguientes:

1. En primer lugar, debes obtener el nuevo certificado. Esto se suele hacer mediante el envío de una solicitud de firma de certificado (CSR) a un tercero, el proveedor de certificado público. Hay varias maneras para generar el CSR, incluso desde Windows 7 o posterior PC. Tu proveedor debe tener documentación para esto.

    * Asegúrate de que el certificado cumple el [AD FS y SSL de Proxy de aplicación Web de requisitos de certificados](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. Cuando tengas la respuesta del proveedor de certificado, importarlo en el almacén de equipo Local en cada servidor de AD FS y Proxy de aplicación Web.

1. En la **principal** servidor de AD FS, usa el cmdlet siguiente para instalar el nuevo certificado SSL

```powershell
Set-AdfsSslCertificate -Thumbprint '<thumbprint of new cert>'
```

La huella digital de certificado se puede encontrar al ejecutar este comando:

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Notas adicionales

* El cmdlet Set-AdfsSslCertificate es un cmdlet de varios nodos; Esto significa que solo tiene que ejecutar desde el principal y se actualizarán todos los nodos de la batería. Esto es nuevo en el servidor de 2016. En Server 2012 R2 debía ejecutar conjunto AdfsSslCertificate en cada servidor.
* El cmdlet Set-AdfsSslCertificate debe ejecutarse solo en el servidor principal. El servidor principal tiene que estar ejecutándose Server 2016 y el nivel de comportamiento de granja debe emitirse a 2016.
* El cmdlet Set-AdfsSslCertificate a usar PowerShell remoto para configurar el resto de los servidores de AD FS, asegúrate de que el puerto 5985 (TCP) esté abierto en los otros nodos.
* El cmdlet Set-AdfsSslCertificate concede los adfssrv principal permisos de lectura a las claves privadas del certificado SSL. Esta entidad de seguridad representa el servicio de AD FS. No es necesario conceder el acceso de lectura de cuenta de servicio de AD FS a las claves privadas del certificado SSL.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-alternate-tls-binding-mode"></a>Reemplazar el certificado SSL de AD FS que se ejecuta en modo de enlace TLS alternativo
Cuando configurado en el cliente alternativo el modo de enlace TLS, AD FS realiza la autenticación de certificado de dispositivo en el puerto 443 y autenticación de certificado de usuario en el puerto 443, así como en un nombre de host diferente. El nombre de host del certificado de usuario es el AD FS hostname antepone con "certauth", por ejemplo, "certauth.fs.contoso.com".
En este modo, usa el cmdlet de powershell conjunto AdfsAlternateTlsClientBinding para administrar el certificado SSL. Administrará no solo el enlace de TLS cliente alternativo pero resto de los enlaces en el que AD FS establece también el certificado SSL.

Sigue los pasos siguientes:

1. En primer lugar, debes obtener el nuevo certificado. Esto se suele hacer mediante el envío de una solicitud de firma de certificado (CSR) a un tercero, el proveedor de certificado público. Hay varias maneras para generar el CSR, incluso desde Windows 7 o posterior PC. Tu proveedor debe tener documentación para esto.

    * Asegúrate de que el certificado cumple el [AD FS y SSL de Proxy de aplicación Web de requisitos de certificados](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. Cuando tengas la respuesta del proveedor de certificado, importarlo en el almacén de equipo Local en cada servidor de AD FS y Proxy de aplicación Web.

1. En la **principal** servidor de AD FS, usa el cmdlet siguiente para instalar el nuevo certificado SSL

```powershell
Set-AdfsAlternateTlsClientBinding -Thumbprint '<thumbprint of new cert>'
```

La huella digital de certificado se puede encontrar al ejecutar este comando:

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Notas adicionales

* El cmdlet Set-AdfsAlternateTlsClientBinding es un cmdlet de varios nodos; Esto significa que solo tiene que ejecutar desde el principal y se actualizarán todos los nodos de la batería.
* El cmdlet Set-AdfsAlternateTlsClientBinding debe ejecutarse solo en el servidor principal. El servidor principal tiene que estar ejecutándose Server 2016 y el nivel de comportamiento de granja debe emitirse a 2016.
* El cmdlet Set-AdfsAlternateTlsClientBinding a usar PowerShell remoto para configurar el resto de los servidores de AD FS, asegúrate de que el puerto 5985 (TCP) esté abierto en los otros nodos.
* El cmdlet Set-AdfsAlternateTlsClientBinding concede los adfssrv principal permisos de lectura a las claves privadas del certificado SSL. Esta entidad de seguridad representa el servicio de AD FS. No es necesario conceder el acceso de lectura de cuenta de servicio de AD FS a las claves privadas del certificado SSL.

## <a name="replacing-the-ssl-certificate-for-the-web-application-proxy"></a>Reemplazar el certificado SSL para el Proxy de aplicación Web
Para configurar el enlace de autenticación de certificado predeterminado o el modo de enlace TLS cliente alternativo en el WAP podemos usar el cmdlet Set-WebApplicationProxySslCertificate.
Para reemplazar el certificado SSL de Proxy de aplicación Web, en **cada** servidor Proxy de aplicación Web usa el cmdlet siguiente para instalar el certificado SSL nuevo:

```powershell
Set-WebApplicationProxySslCertificate '<thumbprint of new cert>'
```

Si se produce un error en el cmdlet anterior porque el antiguo certificado ya ha expirado, volver a configurar al servidor proxy con los siguientes cmdlets:

```powershell
$cred = Get-Credential
```

Escribe las credenciales de un usuario de dominio que es un administrador local en el servidor de AD FS

```powershell
Install-WebApplicationProxy -FederationServiceTrustCredential $cred -CertificateThumbprint '<thumbprint of new cert>' -FederationServiceName 'fs.contoso.com'
```

## <a name="additional-references"></a>Referencias adicionales  
* [AD FS soporte técnico para el enlace de nombre de host alternativo para la autenticación de certificado](../operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [AD FS y propiedad KeySpec información de certificado](../technical-reference/AD-FS-and-KeySpec-Property.md)
