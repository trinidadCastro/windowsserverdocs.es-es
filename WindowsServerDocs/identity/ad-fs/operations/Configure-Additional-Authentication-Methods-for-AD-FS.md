---
ms.assetid: ddfbbda3-30ca-4537-af12-667efc6f63ff
title: Configurar métodos de autenticación adicionales para AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/04/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 88c4d976b9808d254dc1681ce9eee3ca556824ab
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189791"
---
# <a name="configure-additional-authentication-methods-for-ad-fs"></a>Configurar métodos de autenticación adicionales para AD FS

Para habilitar la autenticación multifactor (MFA), debe seleccionar al menos un método de autenticación adicional. De forma predeterminada, en servicios de federación de Active Directory (AD FS) en Windows Server 2012 R2, puede seleccionar autenticación de certificado (en otras palabras, autenticación basada en tarjetas inteligentes) como método de autenticación adicional.

> [!NOTE]
> Si selecciona la autenticación de certificado, asegúrese de que los certificados de la tarjeta inteligente se hayan aprovisionado de manera segura y que tengan requisitos de PIN.

¿Sabía que Microsoft Azure proporciona una funcionalidad similar en la nube? Obtenga más información sobre las [soluciones de identidad de Microsoft Azure](http://aka.ms/m2w274).<br /><br />Crear una solución de identidad híbrida en Microsoft Azure:<br /> - [Obtenga información sobre Azure Multi-factor Authentication.](http://aka.ms/ey6o9r)<br /> - [Administración de identidades para entornos híbridos de bosque único mediante la autenticación en la nube.](http://aka.ms/g1jat8)<br /> - [Administración de riesgos con autenticación multifactor adicional para aplicaciones confidenciales.](http://aka.ms/kt1bbm)

## <a name="microsoft-and-third-party-additional-authentication-methods"></a>Métodos de autenticación adicionales de Microsoft y de otros fabricantes
También puede configurar y habilitar métodos de autenticación de terceros de AD FS en Windows Server 2012 R2 y Microsoft. Una vez instalado y registrado con AD FS, puede aplicar MFA como parte de la directiva de autenticación global o por-terceros de confianza.

A continuación hay una lista alfabética de Microsoft y proveedores de terceros con las ofertas de MFA disponibles actualmente para AD FS en Windows Server 2012 R2.

|Proveedor|Oferta|Vínculo para obtener más información|
|-|-|-| 
|aPersona|aPersona adaptable autenticación multifactor para el SSO de ADFS de Microsoft|[Adaptador de AD FS ASM aPersona](https://www.apersona.com/adfs)|
|Seguridad Duo|Adaptador MFA Duo para AD FS|[Duo autenticación para AD FS](https://duo.com/docs/adfs)|
|Gemalto|Identidad de Gemalto y servicios de seguridad|[http://www.gemalto.com/identity](http://www.gemalto.com/identity)|
|inWebo Technologies|Servicio de autenticación de la empresa inWebo|[Autenticación empresarial inWebo](http://www.inwebo.com)|
|Login People|Conector MFA API de Login People para AD FS 2012 R2 (versión beta pública)|[https://www.loginpeople.com](https://www.loginpeople.com)|
|Microsoft Corporation|Microsoft Azure MFA|[Guía de tutorial: Gestión de riesgos con autenticación multifactor adicional para aplicaciones confidenciales](https://technet.microsoft.com/library/dn280946.aspx) (vea el paso 3)|
Mideye | Proveedor de autenticación mideye para ADFS | [Autenticación en dos fases mideye con el servicio de federación de Active Directory de Microsoft](https://www.mideye.com/support/administrators/documentation/integration/microsoft-adfs/)|
|Okta | Okta MFA para servicios de federación de Active Directory | [Okta MFA para Active Directory Federation Services (ADFS)](https://help.okta.com/en/prod/Content/Topics/integrations/adfs-okta-int.htm)|
|Una sola identidad| Starling 2FA AD FS|[Starling 2FA adaptador de AD FS](https://www.oneidentity.com/products/starling-two-factor-authentication/)|
|Una sola identidad| Defender AD FS|[Adaptador de Defender AD FS](https://www.oneidentity.com/products/defender/)|
|Identidad de ping|Adaptador PingID MFA para AD FS|[Adaptador PingID MFA para AD FS](https://documentation.pingidentity.com/pingid/pingidAdminGuide/index.shtml#pid_c_PingIDforADFSSSO.html)|
|RSA, la división de seguridad de EMC|Agente de autenticación SecurID de RSA para los Servicios de federación de Active Directory de Microsoft|[Agente de autenticación de SecurID RSA para servicios de federación de Microsoft Active Directory](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)|
|SafeNet, Inc.|Agente del servicio de autenticación SafeNet (SAS) para ADFS|[Servicio de autenticación de SafeNet: guía de configuración del agente de ADFS](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)|
|SecureMFA|Proveedor de OTP SecureMFA| [Proveedores de autenticación multifactor ADFS](https://www.securemfa.com/)|
|Swisscom|Servicio de autenticación de Id. de móviles y servicios de firma|[Servicio de autenticación de Id. de móviles](http://swisscom.ch/mid)|
|Symantec|Servicio de validación y protección de ID (VIP) de Symantec|[Validación de Symantec y el servicio de protección de ID (VIP)](http://www.symantec.com/vip-authentication-service)|
|Trusona|Esenciales (sin contraseña MFA) y el ejecutivo (esenciales y corrección de identidad)| [Trusona Multi-factor Authentication](https://www.trusona.com/solution-overview/)|


## <a name="custom-authentication-method-for-ad-fs-in-windows-server-2012-r2"></a>Método de autenticación personalizado para ADFS en Windows Server 2012 R2
Ahora se dan instrucciones para crear su propio método de autenticación personalizado para ADFS en Windows Server 2012 R2. Para obtener más información, consulte [Creación de un método de autenticación personalizado para ADFS en Windows Server 2012 R2](https://go.microsoft.com/fwlink/?LinkID=511980).

## <a name="see-also"></a>Vea también
[Administración de riesgos con autenticación multifactor adicional para aplicaciones confidenciales](Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)


