---
title: Símbolo del sistema de AD FS = inicio de sesión
description: Preguntas más frecuentes para AD FS 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/27/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6a4b6cfe98064181824e210be9031a0f67cb4b75
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824636"
---
# <a name="active-directory-federation-services-promptlogin-parameter-support"></a>Símbolo del sistema de servicios de federación de Active Directory = compatibilidad con parámetros de inicio de sesión
El siguiente documento describe la compatibilidad nativa para el parámetro de inicio de sesión = símbolo del sistema que está disponible en AD FS.

## <a name="what-is-promptlogin"></a>¿Qué es el símbolo del sistema = inicio de sesión?  

Algunas aplicaciones de Office 365 (con autenticación moderna habilitada) envían el parámetro de inicio de sesión = pedir datos a Azure AD como parte de cada solicitud de autenticación.  De forma predeterminada, Azure AD lo traduce en dos parámetros: <code> <b> wauth </b> =urn:oasis:names:tc:SAML:1.0:am:password </code>, y <code> <b> wfresh </b> =0 </code> .

Esto puede causar problemas con la intranet corporativa y escenarios de autenticación multifactor en el que se desea un tipo de autenticación que no sea el nombre de usuario y contraseña.  

AD FS en Windows Server 2012 R2 con el paquete acumulativo de actualizaciones de julio de 2016 introdujo la compatibilidad nativa para el parámetro de inicio de sesión de símbolo del sistema =.  Esto significa que, ahora tiene la opción de configuración de Azure AD para enviar este parámetro como-está a su servicio de AD FS como parte de Azure AD y las solicitudes de autenticación de Office 365.

### <a name="ad-fs-versions-that-support-promptlogin"></a>Las versiones de AD FS que admiten el símbolo del sistema = inicio de sesión
La siguiente es una lista de versiones de AD FS que admite el parámetro de inicio de sesión de símbolo del sistema =.

- Paquete acumulativo de actualizaciones de AD FS en Windows Server 2012 R2 con la de julio de 2016

- AD FS en Windows Server 2016

## <a name="how-do-to-configure-your-azure-ad-tenant-to-send-promptlogin-to-ad-fs"></a>Cómo hacer para configurar el inquilino de Azure AD para enviar mensajes = inicio de sesión a AD FS

Usar el módulo de Azure AD PowerShell para configurar la opción.

> [!NOTE]
> La funcionalidad de inicio de sesión = prompt (habilitada por la propiedad PromptLoginBehavior) actualmente solo está disponible en el ["módulo de Azure AD Powershell versión 1.0'](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185), en el que los cmdlets tienen nombres que incluyan"Msol", tales como Set-MsolDomainFederationSettings.  No está actualmente disponible a través de ' versión 2.0' módulo Azure AD PowerShell, cuyos cmdlets tienen nombres como "Set-AzureAD\*".

Para configurar el símbolo del sistema = el comportamiento de inicio de sesión, la sintaxis de cmdlet siguiente:

Ejemplo 1:
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -PreferredAuthenticationProtocol <your current protocol setting> 
```

Ejemplo 2:
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -SupportsMfa <$True|$False>
```

Ejemplo 3:
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -PromptLoginBehavior <TranslateToFreshPasswordAuth|NativeSupport|Disabled>
```

 
 Los valores de propiedad PreferredAuthenticationProtocol SupportsMfa y PromptLoginBehavior se pueden encontrar consultando el resultado del cmdlet: ![Get-MsolDomainFederationSettings](media/AD-FS-Prompt-Login/GetMsol.png)
```powershell
    Get-MsolDomainFederationSettings -DomainName <your_domain_name> | fl *
 ```
> [!NOTE]
> De forma predeterminada, al ejecutar Get-MsolDomainFederationSettings, algunas propiedades no se muestran en la consola.  Para ver estos parámetros, se recomienda que utilice el | fl * para forzar la salida de todas las propiedades del objeto.


El siguiente es para obtener más información sobre el parámetro PromptLoginBehavior y su configuración.
   
   - <b>TranslateToFreshPasswordAuth</b> significa que el comportamiento de Azure AD predeterminado de enviar <b>wauth</b> y <b>wfresh</b> a AD FS en lugar de símbolo del sistema = inicio de sesión
   - <b>NativeSupport</b> significa que el parámetro de inicio de sesión de símbolo del sistema = se enviarán tal cual a AD FS
   - <b>Deshabilitado</b> significa que nada se enviará a AD FS

