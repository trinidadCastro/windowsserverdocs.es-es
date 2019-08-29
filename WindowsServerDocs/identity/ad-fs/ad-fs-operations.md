---
ms.assetid: eefcc989-8763-45ee-8a64-3a97b4397160
title: Operaciones de AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 594e1605f44dad69ab7eee8b22e6a620ade02ad0
ms.sourcegitcommit: c307886e96622e9595700c94128103b84f5722ce
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70108739"
---
# <a name="ad-fs-operations"></a>Operaciones de AD FS



Este documento contiene una lista de todas las operaciones de documentación para AD FS. 

## <a name="service-configuration"></a>Configuración del servicio
- [Actualización de certificados SSL en AD FS y WAP 2016](../ad-fs/operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)
- [Herramienta de restauración rápida de AD FS](../ad-fs/operations/AD-FS-Rapid-Restore-Tool.md)
- [Configurar el enlace de nombre de host alternativo para la autenticación de certificados en AD FS](../ad-fs/operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
- [Adición de un almacén de atributos](../ad-fs/operations/Add-an-Attribute-Store.md)
- [Personalización de encabezados de respuesta de seguridad HTTP con AD FS 2019](../ad-fs/operations/customize-http-security-headers-ad-fs.md)
- [Delegar el acceso de Commandlet de Powershell de AD FS a los usuarios que no son administradores](../ad-fs/operations/delegate-ad-fs-pshell-access.md)
- [Ajustar SQL y la latencia de direcciones](../ad-fs/operations/adfs-sql-latency.md) 


## <a name="authentication-configuration"></a>Configuración de autenticación
### <a name="strong-authentication-mfa--password-less"></a>Autenticación sólida (MFA) & sin contraseña
- [Configurar proveedores de autenticación externos como principales en AD FS (2019 o posterior)](../ad-fs/operations/Additional-Authentication-Methods-AD-FS.md)
- [Configuración de AD FS (2016 o posterior) y Azure MFA](../ad-fs/operations/Configure-AD-FS-2016-and-Azure-MFA.md)
- [Configurar métodos de autenticación adicionales para AD FS](../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)

### <a name="lockout-protection"></a>Protección de bloqueo
- [Configurar la protección de bloqueo parcial de la Extranet de AD FS](../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md)
- [Configurar la protección de bloqueo inteligente de la Extranet de AD FS](../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)
- [Configurar IP prohibidas de la Extranet de AD FS](../ad-fs/operations/Configure-AD-FS-Banned-IP.md)

### <a name="policy-configuration"></a>Configuración de directiva
- [Configuración de directivas de autenticación](../ad-fs/operations/Configure-Authentication-Policies.md)
- [Configuración de identificador de inicio de sesión alternativo](../ad-fs/operations/Configuring-Alternate-Login-ID.md)
- [Configurar Azure AD comportamiento del inicio de sesión del sistema para trabajar con la Directiva de AD FS](../ad-fs/operations/AD-FS-Prompt-Login.md)

### <a name="kerberos--certificate-authentication"></a>Autenticación de certificados & de Kerberos
- [Habilitar notificaciones de AD DS & la autenticación compuesta de Kerberos en AD FS](../ad-fs/operations/AD-FS-Compound-Authentication-and-AD-DS-claims.md)
- [Configurar AD FS para la autenticación de certificados de usuario](../ad-fs/operations/Configure-User-Certificate-Authentication.md)
- [Configurar el enlace de nombre de host alternativo para la autenticación de certificados en AD FS](../ad-fs/operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)


### <a name="device"></a>Dispositivo
- [Controles de autenticación de dispositivos en AD FS](../ad-fs/operations/device-authentication-controls-in-AD-FS.md) 


## <a name="authorization-configuration"></a>Configuración de autorización
- [Configurar directivas de Access Control en AD FS](../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)
- [Configuración del acceso condicional basado en dispositivos a nivel local](../ad-fs/operations/Configure-Device-based-Conditional-Access-on-Premises.md)

## <a name="rpt--cpt-configuration"></a>RPT & configuración de CPT
- [Configuración de AD FS para autenticar a los usuarios almacenados en directorios LDAP](../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)
- [Configuración de regla de notificación](../ad-fs/operations/Configure-Claim-Rules.md) 
- [Creación de una confianza de proveedor de notificaciones](../ad-fs/operations/Create-a-Claims-Provider-Trust.md) 
- [Crear una relación de confianza para usuario autenticado que no sea para notificaciones](../ad-fs/operations/Create-a-Non-Claims-Aware-Relying-Party-Trust.md)
- [Creación de una relación de confianza para usuario autenticado](../ad-fs/operations/Create-a-Relying-Party-Trust.md)
- [Configurar AD FS para trabajar con un proveedor de Federación agregado (por ejemplo, infrecuente)](../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)

## <a name="sign-in-experience-configuration"></a>Configuración de la experiencia de inicio de sesión
- [Configurar los valores de inicio de sesión único de AD FS 2016](../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)
- [Configuración de AD FS inicio de sesión paginado](../ad-fs/operations/AD-FS-paginated-sign-in.md)
- [Configuración de la personalización del inicio de sesión de AD FS usuario](../ad-fs/operations/AD-FS-user-sign-in-customization.md)
- [Configuración de AD FS para enviar notificaciones de expiración de contraseña](../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)
- [Configuración de autenticación basada en formularios de intranet para dispositivos que no son compatibles con WIA](../ad-fs/operations/Configure-intranet-forms-based-authentication-for-devices-that-do-not-support-WIA.md)

## <a name="other"></a>Otros
- [Unirse a un área de trabajo desde cualquier dispositivo para SSO y autenticación de segundo factor sin problemas en todas las aplicaciones de la compañía](../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [Administración de riesgos con la autenticación multifactor adicional para aplicaciones confidenciales](../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
- [Administración de riesgos con control de acceso condicional](../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)
- [Configuración de un entorno de laboratorio de AD FS](../ad-fs/operations/Set-up-an-AD-FS-lab-environment.md)
- [Guía de tutorial: Administración de riesgos con Multi-Factor Authentication adicionales para aplicaciones confidenciales](../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
- [Guía de tutorial: administración de riesgos con control de acceso condicional](../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)
- [Tutorial: Workplace Join con un dispositivo Windows](../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)
- [Tutorial: Workplace Join con un dispositivo iOS](../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

  


