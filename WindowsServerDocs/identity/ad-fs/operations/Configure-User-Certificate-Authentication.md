---
ms.assetid: 1ea2e1be-874f-4df3-bc9a-eb215002da91
title: Configurar la compatibilidad con AD FS para la autenticación de certificados de usuario
description: ''
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c69192a4223379b896a57eb04a38e37863c1366e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444309"
---
# <a name="configuring-ad-fs-for-user-certificate-authentication"></a>Configuración de AD FS para la autenticación de certificados de usuario


AD FS puede configurarse para x509 autenticación de certificado de usuario mediante uno de los modos que se describe en [en este artículo](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md). Esta funcionalidad puede usarse [con Azure Active Directory](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/) o por sí mismo habilitar los clientes y dispositivos aprovisionados con certificados de usuario para tener acceso a AD FS recursos desde la intranet o extranet.

## <a name="prerequisites"></a>Requisitos previos
- Asegúrese de que los certificados de usuario son de confianza de AD FS y WAP todos los servidores
- Asegúrese de que el certificado raíz de la cadena de confianza para los certificados de usuario en el almacén NTAuth en Active Directory
- Si usa AD FS en el modo de autenticación de certificado alternativo, asegúrese de que los servidores de AD FS y WAP tienen certificados SSL que contienen el nombre de host de AD FS el prefijo "certauth", por ejemplo "certauth.fs.contoso.com", y se permite que el tráfico a este nombre de host a través del firewall
- Si utiliza la autenticación de certificado desde la extranet, asegúrese de que al menos un AIA y al menos un CDP o OCSP ubicación de la lista especificada en los certificados son accesibles desde internet.
- Si va a configurar AD FS para la autenticación de certificados de Azure AD, asegúrese de que ha configurado el [configuración de Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities) y [reglas necesarias notificaciones de AD FS](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-ios#requirements) para el certificado del emisor y número de serie
- También para la autenticación de certificados de Azure AD, para los clientes de Exchange ActiveSync, el certificado de cliente debe tener la dirección de correo electrónico enrutable a los usuarios de Exchange en línea en el nombre de entidad de seguridad o el valor de nombre RFC822 del campo nombre alternativo del sujeto. (Azure Active Directory asigna el valor de RFC822 al atributo de dirección de Proxy en el directorio).

## <a name="configure-ad-fs-for-user-certificate-authentication"></a>Configurar AD FS para la autenticación de certificado de usuario  
- Habilitar la autenticación de certificado de usuario como una intranet o el método de autenticación de extranet de AD FS, mediante la consola de administración de AD FS o el cmdlet de PowerShell Set-AdfsGlobalAuthenticationPolicy
- Asegúrese de que toda la cadena de confianza, incluidos todos los certificados intermedios, está instalada en todos los servidores de AD FS y WAP. Los certificados intermedios deben instalarse en el almacén de entidades de certificación intermedias de equipo local en servidores de AD FS y WAP todas.
- Si desea utilizar notificaciones basadas en los campos de certificado y las extensiones además de EKU (tipo de notificación https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku), configurar notificación adicional pase a través de reglas en la confianza de proveedor de notificaciones de Active Directory.  Consulte a continuación para obtener una lista completa de las solicitudes de certificado disponibles.  
- [Opcional] Configurar entidades de certificación emisora permitidos para los certificados de cliente mediante las instrucciones en "Administración de emisores de confianza para la autenticación de cliente" en [en este artículo](https://technet.microsoft.com/library/dn786429(v=ws.11).aspx).

## <a name="configure-seamless-certificate-authentication-for-chrome-browser-on-windows-desktops"></a>Configurar la autenticación de certificados sin problemas para el explorador Chrome en equipos de escritorio de Windows
Cuando hay varios certificados de usuario (por ejemplo, los certificados de Wi-Fi) en el equipo que satisfacen los fines de autenticación del cliente, el Explorador de Chrome en el escritorio de Windows le solicitará al usuario seleccionar el certificado correcto. Esto puede resultar confuso para el usuario final. Para optimizar esta experiencia, puede establecer una directiva para Chrome para seleccionar automáticamente el certificado correcto para una mejor experiencia de usuario. Esta directiva se puede establecer manualmente mediante la realización de un cambio de registro o configura automáticamente a través de GPO (para establecer las claves del registro). Esto requiere que los certificados de cliente de usuario para la autenticación con AD FS para tener distintos emisores de otros casos de uso. 

Para obtener más información sobre cómo configurar esto para Chrome, consulte esta [vínculo](http://www.chromium.org/administrators/policy-list-3#AutoSelectCertificateForUrls).  


## <a name="troubleshooting"></a>Solución de problemas
- Si se producen errores de las solicitudes de autenticación de certificado con un HTTP 204 "ningún contenido de https:\//certauth.fs.contoso.com" respuesta, compruebe que la raíz y los certificados de CA intermedios están instalados, respectivamente, para la entidad de certificación raíz de confianza y certificado de CA intermedio se almacena en todos los servidores de federación.
- Si se producen errores en las solicitudes de autenticación de certificado por razones desconocidas, exporte el certificado de cliente a un archivo .cer y ejecute el comando 

`certutil -f -urlfetch -verify certificatefilename.cer`

Asegúrese de las CRL y resolver las ubicaciones de CRL delta.  Tenga en cuenta que se encuentran las ubicaciones de CRL delta en función del contenido de la CRL Base.

## <a name="reference-complete-list-of-user-certificate-claim-types-and-example-values"></a>Referencia: Ejemplos de valores y tipos de notificaciones de una lista completa de certificado de usuario

|                                         Tipo de notificación                                         |                              Valor de ejemplo                               |
|--------------------------------------------------------------------------------------------|--------------------------------------------------------------------------|
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/x509version         |                                    3                                     |
|     https://schemas.microsoft.com/2012/12/certificatecontext/field/signaturealgorithm      |                                sha256RSA                                 |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/issuer            |                 CN = entca, DC = domain, DC = contoso, DC = com                  |
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/issuername          |                 CN = entca, DC = domain, DC = contoso, DC = com                  |
|          https://schemas.microsoft.com/2012/12/certificatecontext/field/notbefore          |                           12/05/2016 20:50:18                            |
|          https://schemas.microsoft.com/2012/12/certificatecontext/field/notafter           |                           12/05/2017 20:50:18                            |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/subject           |   E =user@contoso.com, CN = usuario, CN = Users, DC = domain, DC = contoso, DC = com   |
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/subjectname         |   E =user@contoso.com, CN = usuario, CN = Users, DC = domain, DC = contoso, DC = com   |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/rawdata           |                {Los datos de certificado digital de codificada en Base64}                 |
|        https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage         |                             DigitalSignature                             |
|        https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage         |                             KeyEncipherment                              |
|  https://schemas.microsoft.com/2012/12/certificatecontext/extension/subjectkeyidentifier   |                 9D11941EC06FACCCCB1B116B56AA97F3987D620A                 |
| https://schemas.microsoft.com/2012/12/certificatecontext/extension/authoritykeyidentifier  |    KeyID = d6 13 e3 6b bc e5 d8 15 52 0a fd 36 6a d5 0b 51 f3 0b 25 7f     |
| https://schemas.microsoft.com/2012/12/certificatecontext/extension/certificatetemplatename |                                   Usuario                                   |
|           https://schemas.microsoft.com/2012/12/certificatecontext/extension/san           | Otro nombre de entidad de seguridad: nombre =user@contoso.com, Nombre RFC822 =user@contoso.com |
|           https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku           |                          1.3.6.1.4.1.311.10.3.4                          |

