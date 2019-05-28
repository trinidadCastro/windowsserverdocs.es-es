---
ms.assetid: a3f50046-5d48-43d3-b0f8-ac2346b15285
title: Administrar certificados SSL en AD FS y WAP en Windows Server 2016
description: Administrar certificados SSL en AD FS y WAP en Windows Server 2016
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/02/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9bae831da9d247c423c2874a5928b7f811ef65dc
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188712"
---
# <a name="managing-ssl-certificates-in-ad-fs-and-wap-in-windows-server-2016"></a>Administrar certificados SSL en AD FS y WAP en Windows Server 2016



En este artículo se describe cómo implementar un nuevo certificado SSL en los servidores de AD FS y WAP.

>[!NOTE]
>Reemplace el certificado SSL en el futuro para una granja de AD FS de la manera recomendada es usar Azure AD Connect.  Para obtener más información, consulte [actualizar el certificado SSL para una granja de servidores de servicios de federación de Active Directory (AD FS)](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectfed-ssl-update)

## <a name="obtaining-your-ssl-certificates"></a>Obtener los certificados SSL
Para granjas de servidores de AD FS de producción se recomienda un certificado SSL de confianza pública. Normalmente, esto se logra mediante el envío de una solicitud de firma de certificado (CSR) a un tercero, el proveedor de certificados públicos. Hay varias maneras de generar el CSR, incluso desde un equipo PC superior o Windows 7. El proveedor debe tener documentación para esto.

- Asegúrese de que el certificado cumple los [requisitos del certificado SSL de Proxy de aplicación Web y AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="how-many-certificates-are-needed"></a>¿Cuántos certificados son necesarios
Se recomienda que use un certificado SSL común entre los servidores de todos los AD FS y Proxy de aplicación Web. Para conocer los requisitos detallados, consulte el documento [requisitos del certificado SSL de Proxy de aplicación Web y AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="ssl-certificate-requirements"></a>Requisitos de certificados SSL
Para conocer los requisitos incluidos de nomenclatura, raíz de confianza y las extensiones consulte el documento [requisitos del certificado SSL de Proxy de aplicación Web y AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

## <a name="replacing-the-ssl-certificate-for-ad-fs"></a>Sustitución del certificado SSL para AD FS
> [!NOTE]
> El certificado SSL de AD FS no es el mismo que el certificado de comunicaciones de servicio de AD FS se encuentra en el complemento de administración de AD FS. Para cambiar el certificado SSL de AD FS, necesitará usar PowerShell.

En primer lugar, determine qué modo de enlace de certificado que se ejecutan los servidores de AD FS: enlace de autenticación de certificado predeterminado o el modo de enlace de TLS de cliente alternativo.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-default-certificate-authentication-binding-mode"></a>Sustitución del certificado SSL para AD FS que se ejecuta de forma predeterminada en modo de enlace de autenticación de certificado
AD FS de forma predeterminada realiza la autenticación de certificados de dispositivo en el puerto 443 y autenticación de certificados de usuario en el puerto 49443 (o un puerto configurable que no es el 443).
En este modo, use el cmdlet de powershell Set-AdfsSslCertificate para administrar el certificado SSL.

Sigue los pasos siguientes:

1. En primer lugar, deberá obtener el nuevo certificado. Normalmente, esto se hace mediante el envío de una solicitud de firma de certificado (CSR) a un tercero, el proveedor de certificados públicos. Hay varias maneras de generar el CSR, incluso desde un equipo PC superior o Windows 7. El proveedor debe tener documentación para esto.

    * Asegúrese de que el certificado cumple los [requisitos del certificado SSL de Proxy de aplicación Web y AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. Una vez que obtenga la respuesta de su proveedor de certificados, impórtelo en el almacén del equipo Local en cada servidor de AD FS y Proxy de aplicación Web.

1. En el **principal** servidor AD FS, use el siguiente cmdlet para instalar el nuevo certificado SSL

```powershell
Set-AdfsSslCertificate -Thumbprint '<thumbprint of new cert>'
```

La huella digital del certificado puede encontrarse ejecutando este comando:

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Notas adicionales

* El cmdlet Set-AdfsSslCertificate es un cmdlet de varios nodo; Esto significa solo debe ejecutarse desde el servidor principal y se actualizarán todos los nodos de la granja de servidores. Esto es nuevo en Server 2016. En Server 2012 R2, debía ejecutar Set-AdfsSslCertificate en cada servidor.
* El cmdlet Set-AdfsSslCertificate tiene que ejecutarse solo en el servidor principal. El servidor principal debe estar ejecutando Server 2016 y debe elevarse el nivel de comportamiento de la granja de servidores a 2016.
* El cmdlet Set-AdfsSslCertificate usará comunicación remota de PowerShell para configurar los demás servidores de AD FS, asegúrese de que el puerto 5985 (TCP) esté abierto en los demás nodos.
* El cmdlet Set-AdfsSslCertificate le dará la adfssrv principal permisos de lectura a las claves privadas del certificado SSL. Esta entidad representa el servicio AD FS. No es necesario conceder el acceso de lectura de cuenta de servicio de AD FS para las claves privadas del certificado SSL.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-alternate-tls-binding-mode"></a>Sustitución del certificado SSL para AD FS que se ejecuta en modo de enlace TLS alternativo
Cuando se configura en el cliente alternativo el modo de enlace TLS, AD FS realiza autenticación de certificados de dispositivo en el puerto 443 y la autenticación de certificados de usuario en el puerto 443, en un nombre de host diferente. El nombre de host del certificado de usuario es el AD FS hostname precede "certauth", por ejemplo, "certauth.fs.contoso.com".
En este modo, use el cmdlet de powershell Set-AdfsAlternateTlsClientBinding para administrar el certificado SSL. Esto va a administrar no solo el enlace de TLS de cliente alternativos, pero todos los otros enlaces en los que AD FS establece también el certificado SSL.

Sigue los pasos siguientes:

1. En primer lugar, deberá obtener el nuevo certificado. Normalmente, esto se hace mediante el envío de una solicitud de firma de certificado (CSR) a un tercero, el proveedor de certificados públicos. Hay varias maneras de generar el CSR, incluso desde un equipo PC superior o Windows 7. El proveedor debe tener documentación para esto.

    * Asegúrese de que el certificado cumple los [requisitos del certificado SSL de Proxy de aplicación Web y AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. Una vez que obtenga la respuesta de su proveedor de certificados, impórtelo en el almacén del equipo Local en cada servidor de AD FS y Proxy de aplicación Web.

1. En el **principal** servidor AD FS, use el siguiente cmdlet para instalar el nuevo certificado SSL

```powershell
Set-AdfsAlternateTlsClientBinding -Thumbprint '<thumbprint of new cert>'
```

La huella digital del certificado puede encontrarse ejecutando este comando:

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Notas adicionales

* El cmdlet Set-AdfsAlternateTlsClientBinding es un cmdlet de varios nodo; Esto significa solo debe ejecutarse desde el servidor principal y se actualizarán todos los nodos de la granja de servidores.
* El cmdlet Set-AdfsAlternateTlsClientBinding tiene que ejecutarse solo en el servidor principal. El servidor principal debe estar ejecutando Server 2016 y debe elevarse el nivel de comportamiento de la granja de servidores a 2016.
* El cmdlet Set-AdfsAlternateTlsClientBinding usará comunicación remota de PowerShell para configurar los demás servidores de AD FS, asegúrese de que el puerto 5985 (TCP) esté abierto en los demás nodos.
* El cmdlet Set-AdfsAlternateTlsClientBinding le dará la adfssrv principal permisos de lectura a las claves privadas del certificado SSL. Esta entidad representa el servicio AD FS. No es necesario conceder el acceso de lectura de cuenta de servicio de AD FS para las claves privadas del certificado SSL.

## <a name="replacing-the-ssl-certificate-for-the-web-application-proxy"></a>Sustitución del certificado SSL para el Proxy de aplicación Web
Para configurar el enlace de autenticación de certificado predeterminado o el modo de enlace de TLS de cliente alternativo en el WAP podemos usar el cmdlet Set-WebApplicationProxySslCertificate.
Para reemplazar el certificado SSL de Proxy de aplicación Web, en **cada** servidor Proxy de aplicación Web use el siguiente cmdlet para instalar el nuevo certificado SSL:

```powershell
Set-WebApplicationProxySslCertificate '<thumbprint of new cert>'
```

Si se produce un error en el cmdlet anterior porque ya ha expirado el certificado antiguo, vuelva a configurar al proxy mediante los siguientes cmdlets:

```powershell
$cred = Get-Credential
```

Escriba las credenciales de un usuario de dominio que sea administrador local en el servidor de AD FS

```powershell
Install-WebApplicationProxy -FederationServiceTrustCredential $cred -CertificateThumbprint '<thumbprint of new cert>' -FederationServiceName 'fs.contoso.com'
```

## <a name="additional-references"></a>Referencias adicionales  
* [Compatibilidad de AD FS con el enlace de nombre de host alternativo para la autenticación de certificado](../operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [AD FS y la propiedad KeySpec información de certificado](../technical-reference/AD-FS-and-KeySpec-Property.md)
