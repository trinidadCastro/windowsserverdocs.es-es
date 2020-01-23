---
ms.assetid: ddfbbda3-30ca-4537-af12-667efc6f63ff
title: Configurar métodos de autenticación adicionales para AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/26/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a7cfc80a1e137a4184afa0e8b33a488f1e35dcfa
ms.sourcegitcommit: 840d1d8851f68936db3934c80796fb8722d3c64a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519457"
---
# <a name="configure-additional-authentication-methods-for-ad-fs"></a>Configurar métodos de autenticación adicionales para AD FS

Para habilitar la autenticación multifactor (MFA), debe seleccionar al menos un método de autenticación adicional. De forma predeterminada, en Servicios de federación de Active Directory (AD FS) (AD FS) en Windows Server 2012 R2, puede seleccionar autenticación de certificado (es decir, autenticación basada en tarjeta inteligente) como método de autenticación adicional.

> [!NOTE]
> Si selecciona la autenticación de certificado, asegúrese de que los certificados de la tarjeta inteligente se hayan aprovisionado de manera segura y que tengan requisitos de PIN.

¿Sabía que Microsoft Azure proporciona una funcionalidad similar en la nube? Obtenga más información sobre las [soluciones de identidad de Microsoft Azure](https://aka.ms/m2w274).<br /><br />Crear una solución de identidad híbrida de Microsoft Azure:<br /> - [más información sobre Azure multi-factor Authentication.](https://aka.ms/ey6o9r)<br /> - [administrar identidades para entornos híbridos de bosque único mediante la autenticación en la nube.](https://aka.ms/g1jat8)<br /> - [administrar los riesgos con multi-factor Authentication adicionales para las aplicaciones confidenciales.](https://aka.ms/kt1bbm)

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
|inWebo Technologies|Servicio de autenticación de la empresa inWebo|[Autenticación de inWebo Enterprise](http://www.inwebo.com)|
|Login People|Conector MFA API de Login People para AD FS 2012 R2 (versión beta pública)|[https://www.loginpeople.com](https://www.loginpeople.com)|
|Microsoft Corporation|Microsoft Azure MFA|[Guía de tutorial: administración de riesgos con autenticación multifactor adicional para aplicaciones confidenciales](https://technet.microsoft.com/library/dn280946.aspx) (consulte el paso 3)|
Mideye | Proveedor de autenticación Mideye para ADFS | [Mideye la autenticación en dos fases con Microsoft Active Directory Servicio de federación](https://www.mideye.com/support/administrators/documentation/integration/microsoft-adfs/)|
|Okta | Okta MFA para Servicios de federación de Active Directory (AD FS) | [Okta MFA para Servicios de federación de Active Directory (AD FS) (ADFS)](https://help.okta.com/en/prod/Content/Topics/integrations/adfs-okta-int.htm)|
|One Identity| Starling 2FA AD FS|[Adaptador de AD FS Starling 2FA](https://www.oneidentity.com/products/starling-two-factor-authentication/)|
|One Identity| AD FS de defender|[Adaptador de AD FS de defender](https://www.oneidentity.com/products/defender/)|
|Ping Identity|Adaptador de PingID MFA para AD FS|[Adaptador de PingID MFA para AD FS](https://documentation.pingidentity.com/pingid/pingidAdminGuide/index.shtml#pid_c_PingIDforADFSSSO.html)|
|RSA, la división de seguridad de EMC|Agente de autenticación SecurID de RSA para los Servicios de federación de Active Directory de Microsoft|[Agente de autenticación SecurID de RSA para Microsoft Servicios de federación de Active Directory (AD FS)](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)|
|SafeNet, Inc.|Agente del servicio de autenticación SafeNet (SAS) para ADFS|[Servicio de autenticación de SafeNet: Guía de configuración del agente de AD FS](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)|
|SecureMFA|Proveedor de OTP de SecureMFA| [Proveedores de autenticación multifactor de ADFS](https://www.securemfa.com/)|
|Swisscom|Servicio de autenticación de Id. de móviles y servicios de firma|[Servicio de autenticación de ID. móvil](http://swisscom.ch/mid)|
|Symantec|Servicio de validación y protección de ID (VIP) de Symantec|[Servicio de validación y protección de ID (VIP) de Symantec](http://www.symantec.com/vip-authentication-service)|
|Trusona|Essential (MFA con contraseña) y Executive (prueba de la información esencial y de identidad)| [Trusona multi-factor Authentication](https://www.trusona.com/solution-overview/)|


## <a name="custom-authentication-method-for-ad-fs-in-windows-server-2012-r2"></a>Método de autenticación personalizado para ADFS en Windows Server 2012 R2
Ahora se dan instrucciones para crear su propio método de autenticación personalizado para ADFS en Windows Server 2012 R2. Para obtener más información, consulte [Creación de un método de autenticación personalizado para ADFS en Windows Server 2012 R2](https://go.microsoft.com/fwlink/?LinkID=511980).

## <a name="see-also"></a>Consulta también
[Administración de riesgos con la autenticación multifactor adicional para aplicaciones confidenciales](Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)


