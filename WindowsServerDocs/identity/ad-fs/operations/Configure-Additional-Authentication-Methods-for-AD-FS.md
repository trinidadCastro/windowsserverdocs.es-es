---
description: Más información acerca de cómo configurar métodos de autenticación adicionales para AD FS
ms.assetid: ddfbbda3-30ca-4537-af12-667efc6f63ff
title: Configurar métodos de autenticación adicionales para AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/26/2019
ms.topic: article
ms.openlocfilehash: c18f5c932c20e74de402942c75ac987873c538cf
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97046684"
---
# <a name="configure-additional-authentication-methods-for-ad-fs"></a>Configurar métodos de autenticación adicionales para AD FS

Para habilitar la autenticación multifactor (MFA), debe seleccionar al menos un método de autenticación adicional. De forma predeterminada, en Servicios de federación de Active Directory (AD FS) (AD FS) en Windows Server 2012 R2, puede seleccionar autenticación de certificado (es decir, autenticación basada en tarjeta inteligente) como método de autenticación adicional.

> [!NOTE]
> Si selecciona la autenticación de certificado, asegúrese de que los certificados de la tarjeta inteligente se hayan aprovisionado de manera segura y que tengan requisitos de PIN.

¿Sabía que Microsoft Azure proporciona una funcionalidad similar en la nube? Obtenga más información sobre las [soluciones de identidad de Microsoft Azure](https://aka.ms/m2w274).<p>Crear una solución de identidad híbrida de Microsoft Azure:<br /> - [Más información sobre Azure Multi-Factor Authentication.](/azure/active-directory/authentication/concept-mfa-howitworks)<br /> - [Administrar identidades para entornos híbridos de bosque único mediante la autenticación en la nube.](/previous-versions/windows/it-pro/solutions-guidance/dn550986(v=ws.11))<br /> - [Administrar los riesgos con Multi-Factor Authentication adicionales para las aplicaciones confidenciales.](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280946(v=ws.11))

## <a name="microsoft-and-third-party-additional-authentication-methods"></a>Métodos de autenticación adicionales de Microsoft y de otros fabricantes
También puede configurar y habilitar métodos de autenticación de Microsoft y de terceros en AD FS en Windows Server 2012 R2. Una vez instalado y registrado con AD FS, puede aplicar MFA como parte de la Directiva de autenticación global o por entidad de confianza.

A continuación hay una lista alfabética de Microsoft y proveedores de terceros con las ofertas de MFA disponibles actualmente para AD FS en Windows Server 2012 R2.

|Proveedor|Oferta|Vínculo para obtener más información|
|-|-|-|
|aPersona|aPersona Adaptive Multi-Factor Authentication para Microsoft ADFS SSO|[Adaptador ADFS de aPersona ASM](https://www.apersona.com/adfs)|
|Cyphercor Inc.|Multi-Factor Authentication LoginTC para AD FS|[Conector de AD FS de LoginTC](https://www.logintc.com/docs/connectors/adfs.html)|
|Duo Security|Duo adaptador MFA para AD FS|[Duo autenticación para AD FS](https://duo.com/docs/adfs)|
|Futurae|Conjunto de autenticación Futurae para AD FS|[Autenticación segura Futurae](https://futurae.com)|
|Gemalto|Identidad de Gemalto y servicios de seguridad|[http://www.gemalto.com/identity](http://www.gemalto.com/identity)|
|inWebo Technologies|Servicio de autenticación de la empresa inWebo|[Autenticación empresarial inWebo](http://www.inwebo.com)|
|Login People|Conector MFA API de Login People para AD FS 2012 R2 (versión beta pública)|[https://www.loginpeople.com](https://www.loginpeople.com)|
|Microsoft Corporation|Microsoft Azure MFA|[Guía de tutorial: administración de riesgos con autenticación multifactor adicional para aplicaciones confidenciales](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280946(v=ws.11)) (consulte el paso 3)|
Mideye | Proveedor de autenticación Mideye para ADFS | [Mideye la autenticación en dos fases con Microsoft Active Directory Servicio de federación](https://www.mideye.com/support/administrators/documentation/integration/microsoft-adfs/)|
|Okta | Okta MFA para Servicios de federación de Active Directory (AD FS) | [Okta MFA para Servicios de federación de Active Directory (AD FS) (ADFS)](https://help.okta.com/en/prod/Content/Topics/integrations/adfs-okta-int.htm)|
|One Identity| Starling 2FA AD FS|[Adaptador de AD FS Starling 2FA](https://www.oneidentity.com/products/starling-two-factor-authentication/)|
|One Identity| AD FS de defender|[Adaptador de AD FS de defender](https://www.oneidentity.com/products/defender/)|
|Identidad de ping|Adaptador de PingID MFA para AD FS|[Adaptador de PingID MFA para AD FS](https://documentation.pingidentity.com/pingid/pingidAdminGuide/index.shtml#pid_c_PingIDforADFSSSO.html)|
|RSA, la división de seguridad de EMC|Agente de autenticación SecurID de RSA para los Servicios de federación de Active Directory de Microsoft|[Agente de autenticación SecurID de RSA para los Servicios de federación de Active Directory de Microsoft](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)|
|SafeNet, Inc.|Agente del servicio de autenticación SafeNet (SAS) para ADFS|[Servicio de autenticación de SafeNet: guía de configuración del agente de AD FS](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)|
|SecureMFA|Proveedor de OTP de SecureMFA| [Proveedores de autenticación multifactor de ADFS](https://www.securemfa.com/)|
|Swisscom|Servicio de autenticación de Id. de móviles y servicios de firma|[Servicio de autenticación de Id. de móviles](http://swisscom.ch/mid)|
|Symantec|Servicio de validación y protección de ID (VIP) de Symantec|[Servicio de validación y protección de ID (VIP) de Symantec](http://www.symantec.com/vip-authentication-service)|
|Trusona|Essential (MFA con contraseña) y Executive (prueba de la información esencial y de identidad)| [Trusona multi-factor Authentication](https://www.trusona.com/solution-overview/)|


## <a name="custom-authentication-method-for-ad-fs-in-windows-server-2012-r2"></a>Método de autenticación personalizado para ADFS en Windows Server 2012 R2
Ahora se dan instrucciones para crear su propio método de autenticación personalizado para ADFS en Windows Server 2012 R2. Para obtener más información, consulte [Creación de un método de autenticación personalizado para ADFS en Windows Server 2012 R2](https://go.microsoft.com/fwlink/?LinkID=511980).

## <a name="see-also"></a>Consulte también
[Administración de riesgos con la autenticación multifactor adicional para aplicaciones confidenciales](Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
