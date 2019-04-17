---
ms.assetid: ddfbbda3-30ca-4537-af12-667efc6f63ff
title: "Configurar los métodos de autenticación adicional de AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 65baa6f94e1efa1fa337eab477b18f947cf0bb2d
ms.sourcegitcommit: 36d7b1dfd7da8e9f303d007a628e76149de000f2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/21/2018
---
# <a name="configure-additional-authentication-methods-for-ad-fs"></a>Configurar los métodos de autenticación adicional de AD FS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Para habilitar la autenticación multifactor (AMF), debes seleccionar al menos un método de autenticación adicional. De manera predeterminada, en servicios de federación de Active Directory (AD FS) en Windows Server 2012 R2, puedes seleccionar la autenticación de certificado (en otras palabras, la autenticación con tarjeta inteligente) como un método de autenticación adicional.

> [!NOTE]
> Si seleccionas la autenticación de certificado, asegúrate de que los certificados de tarjeta inteligente se haya aprovisionado de forma segura y tienen requisitos de pin.

¿Sabías que Microsoft Azure proporciona una funcionalidad similar en la nube? Obtén más información sobre [soluciones de identidad de Microsoft Azure ](http://aka.ms/m2w274).<br /><br />Crear una solución de identidad híbrida en Microsoft Azure:<br /> - [Obtén información sobre Azure la autenticación multifactor.](http://aka.ms/ey6o9r)<br /> - [Administrar las identidades de los entornos de bosque único híbrida mediante la autenticación de nube.](http://aka.ms/g1jat8)<br /> - [Administrar el riesgo con la autenticación multifactor adicional para aplicaciones con información confidencial.](http://aka.ms/kt1bbm)|

## <a name="microsoft-and-third-party-additional-authentication-methods"></a>Métodos de autenticación adicionales de Microsoft y terceros
También puedes configurar y habilitar Microsoft y métodos de autenticación de terceros de AD FS en Windows Server 2012 R2. Una vez instalado y registrado con AD FS, puedes aplicar AMF como parte de la directiva de autenticación global o por confiar de terceros.

A continuación hay una lista alfabética de Microsoft y los proveedores terceros con ofertas de MFA de AD FS en Windows Server 2012 R2.

||||
|-|-|-|
|**Proveedor**|**Oferta**|**Vínculo para obtener más información**|
|Gemalto|Servicios de seguridad y la identidad Gemalto|[http://www.gemalto.com/identity](http://www.gemalto.com/identity)|
|inWebo tecnologías|inWebo el servicio de autenticación de empresa|[inWebo autenticación de empresa](http://www.inwebo.com)|
|Contactos de inicio de sesión|Conector de la API de MFA personas de inicio de sesión para AD FS 2012 R2 (versión beta)|[https://www.loginpeople.com](https://www.loginpeople.com)|
|Microsoft Corporation.|MFA de Microsoft Azure|[Guía paso a paso: Administrar el riesgo con la autenticación multifactor adicional para aplicaciones con información confidencial](https://technet.microsoft.com/library/dn280946.aspx) (consulta el paso 3)|
|RSA, la división de seguridad de EMC|Agente de autenticación de RSA SecurID para servicios de federación de Microsoft Active Directory|[Agente de autenticación de RSA SecurID para servicios de federación de Microsoft Active Directory](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)|
|SafeNet, Inc.|Agente del servicio (SAS) de autenticación de SafeNet de AD FS|[Servicio de autenticación de SafeNet: Guía de configuración del agente de AD FS](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)|
|Swisscom|Servicio de autenticación de Id. de dispositivos móviles y servicios de firma|[Servicio de autenticación de Id. de móvil](http://swisscom.ch/mid)|
|Symantec|Servicio de protección de identificador (VIP) y la validación de Symantec|[Servicio de protección de identificador (VIP) y la validación de Symantec](http://www.symantec.com/vip-authentication-service)|

## <a name="custom-authentication-method-for-ad-fs-in-windows-server-2012-r2"></a>Método de autenticación personalizado de AD FS en Windows Server 2012 R2
Ahora, proporcionamos instrucciones para crear tu propio método de autenticación personalizado de AD FS en Windows Server 2012 R2. Para obtener más información, consulta [crear un método de autenticación personalizada para AD FS en Windows Server 2012 R2](https://go.microsoft.com/fwlink/?LinkID=511980).

## <a name="see-also"></a>Consulta también
[Administrar el riesgo con la autenticación multifactor adicional para aplicaciones con información confidencial](Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)


