---
ms.assetid: 1ea2e1be-874f-4df3-bc9a-eb215002da91
title: "Configurar la compatibilidad con AD FS para la autenticación de certificado de usuario"
description: 
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.service: active-directory
ms.technology: identity-adfs
ms.openlocfilehash: 9941d4dd997e857874aceddc920ec7f9d8944a81
ms.sourcegitcommit: d351cdbb0bf2533d6db35626ebbc4924b3834309
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2018
---
# <a name="configuring-ad-fs-for-user-certificate-authentication"></a>Configurar AD FS para la autenticación de certificado de usuario

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

AD FS puede configurarse para x509 autenticación de certificado de usuario mediante uno de los modos se describe en [este artículo](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md). Esta funcionalidad puede usarse [con Azure Active Directory](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/) o por sí solo permitir que los clientes y dispositivos aprovisionado con certificados de usuario para acceder a AD FS de recursos de la intranet o en la extranet.

## <a name="prerequisites"></a>Requisitos previos
- Asegúrate de que los certificados de usuario son de confianza todos los AD FS WAP servidores y
- Asegúrate de que el certificado raíz de la cadena de confianza para los certificados de usuario está en el almacén NTAuth en Active Directory
- Si utiliza AD FS en modo de autenticación de certificados alternativos, asegúrate de que los servidores de AD FS y WAP certificados SSL que contienen el nombre de host de AD FS el prefijo "certauth", por ejemplo "certauth.fs.contoso.com" y que se permite el tráfico a este nombre de host a través del firewall
- Si mediante la autenticación de certificado desde la extranet, asegúrate de que al menos un AIA y al menos un CDP o OCSP ubicación desde la lista de los certificados son accesibles desde internet.
- Si estás configurando AD FS para la autenticación de certificado de Azure AD, asegúrate de que has configurado el [configuración de Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities) y la [AD FS reclamar reglas necesarias](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-certificate-based-authentication-ios#requirements) para certificados de emisor y número de serie
- También para la autenticación de certificado de Azure AD, para los clientes de Exchange ActiveSync, el certificado de cliente debe tener la dirección de correo electrónico pueden enrutar los usuarios en Exchange en línea en el nombre de entidad de seguridad o el valor de nombre RFC822 del campo de nombre alternativo del sujeto. (Azure Active Directory asigna el valor RFC822 al atributo de dirección de Proxy en el directorio).

## <a name="configure-ad-fs-for-user-certificate-authentication"></a>Configurar AD FS para la autenticación de certificado de usuario  
- Habilitar la autenticación de certificado de usuario como una intranet o un método de autenticación extranet en AD FS, con la consola de administración de AD FS o el cmdlet de PowerShell Set-AdfsGlobalAuthenticationPolicy
- Asegúrate de que toda la cadena de confianza, incluidos los certificados intermedios, está instalada en todos los servidores de AD FS y WAP. Los certificados intermedios deberían estar instalados en el almacén de entidades de certificación intermedio de equipo local en los servidores de todos los AD FS y WAP.
- Si deseas usar notificaciones en función de los campos de un certificado y extensiones además EKU (tipo de notificación https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku), configurar Reclamación adicionales pasada a través de reglas en la confianza de proveedor de reclamaciones de Active Directory.  Consulta a continuación para obtener una lista completa de una reclamación de certificado disponible.  
- [Opcional] Configurar permitidas entidades de certificación emisora de certificados de cliente mediante las instrucciones en "Administración de emisores de confianza para la autenticación de cliente" en [este artículo](https://technet.microsoft.com/en-us/library/dn786429(v=ws.11).aspx).

## <a name="configure-seamless-certificate-authentication-for-chrome-browser-on-windows-desktops"></a>Configurar la autenticación de certificado perfecto para el explorador Chrome en equipos de escritorio de Windows
Cuando hay varios certificados de usuario (por ejemplo, los certificados de Wi-Fi) en el equipo que cumplan con los fines de autenticación de cliente, el explorador Chrome en escritorio de Windows solicitará al usuario seleccionar el certificado correcto. Esto puede resultar confuso para el usuario final. Para optimizar esta experiencia, puedes establecer una directiva de Chrome a seleccionar el certificado correcto para mejorar la experiencia del usuario automáticamente. Esta directiva se puede establecer manualmente al hacer un cambio en el registro o configura automáticamente a través de GPO (para establecer las claves del registro). Esto requiere los certificados de cliente del usuario para la autenticación de AD FS tener diferentes emisores de otros casos de uso. 

Para obtener más información sobre cómo configurar esto cromo, hacer referencia a este [vínculo](http://www.chromium.org/administrators/policy-list-3#AutoSelectCertificateForUrls).  


## <a name="troubleshooting"></a>Solución de problemas
- Si se produce un error en las solicitudes de autenticación de certificado con una respuesta de HTTP 204 "No contenido desde https://certauth.fs.contoso.com", comprueba que la raíz y los certificados de CA intermedios están instalados, respectivamente, a la raíz de confianza CA y CA intermedia almacenes en todos los servidores de federación de certificados.
- Si las solicitudes de autenticación de certificado se producen errores por motivos desconocidos, exportar el certificado de cliente a un archivo .cer y ejecuta el comando 

`certutil -f -urlfetch -verify certificatefilename.cer`

Asegúrate de cualquier CRL y delta se resuelven CRL ubicaciones.  Ten en cuenta que se encuentran ubicaciones de diferencia CRL en función del contenido de la CRL de Base.

## <a name="reference-complete-list-of-user-certificate-claim-types-and-example-values"></a>Referencia: Lista completa de certificado de usuario reclamar tipos y los valores de ejemplo

|Tipo de notificación|Valor de ejemplo
|-----|-----
|https://schemas.microsoft.com/2012/12/certificatecontext/field/x509version | 3
|https://schemas.microsoft.com/2012/12/certificatecontext/field/signaturealgorithm | sha256RSA
|https://schemas.microsoft.com/2012/12/certificatecontext/field/issuer | CN = entca, DC = dominio, DC = contoso, DC = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/issuername | CN = entca, DC = dominio, DC = contoso, DC = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/notbefore | 12/05/2016 20:50:18
|https://schemas.microsoft.com/2012/12/certificatecontext/field/notafter | 12/05/2017 20:50:18
|https://schemas.microsoft.com/2012/12/certificatecontext/field/subject | E =user@contoso.com, CN = user, CN = usuarios, DC = dominio, DC = contoso, DC = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/subjectname | E =user@contoso.com, CN = user, CN = usuarios, DC = dominio, DC = contoso, DC = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/rawdata | {Datos del certificado digital codificada con Base64}
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage | DigitalSignature
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage | KeyEncipherment
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/subjectkeyidentifier | 9D11941EC06FACCCCB1B116B56AA97F3987D620A
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/authoritykeyidentifier | ID = d6 13 e3 6b bc e5 d8 15 52 0a fd 36 6a d5 0b 51 f3 0b 25 7f
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/certificatetemplatename | Usuario
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/san | Otro nombre de entidad de seguridad: nombre =user@contoso.com, Nombre RFC822 =user@contoso.com
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku | 1.3.6.1.4.1.311.10.3.4


