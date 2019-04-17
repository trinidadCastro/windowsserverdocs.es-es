---
title: "Símbolo del sistema de AD FS = inicio de sesión"
description: "Preguntas más frecuentes de AD FS de 2016"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/27/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8b98cdc27c9bd01d9855e98965f59ebe5257ed5c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="active-directory-federation-services-promptlogin-parameter-support"></a>Los servicios de federación de Active Directory pedir = compatibilidad con parámetros de inicio de sesión
El siguiente documento describe la compatibilidad nativa para el parámetro de inicio de sesión = símbolo del sistema que está disponible en AD FS.

## <a name="what-is-promptlogin"></a>¿Qué es pedir = inicio de sesión?  

Algunas aplicaciones de Office 365 (con autenticación moderna habilitada) envían el parámetro de inicio de sesión = pedir a Azure AD como parte de cada solicitud de autenticación.  De manera predeterminada, Azure AD traduce esto dos parámetros: <code><b>wauth</b>=urn:oasis:names:tc:SAML:1.0:am:password</code>, y <code><b>wfresh</b>=0</code>.

Esto puede provocar problemas de intranet corporativa y escenarios de autenticación multifactor en el que se desea un tipo de autenticación que no sea el nombre de usuario y contraseña.  

AD FS en Windows Server 2012 R2 con el paquete acumulativo de actualizaciones de julio de 2016 introdujo compatibilidad nativa con el parámetro de inicio de sesión = símbolo del sistema.  Esto significa, ahora tienes la opción de configuración de Azure AD para enviar este parámetro como-es el servicio de AD FS como parte de Azure AD y solicitudes de autenticación de Office 365.

### <a name="ad-fs-versions-that-support-promptlogin"></a>Las versiones de AD FS que admiten el símbolo del sistema = inicio de sesión
La siguiente es una lista de versiones de AD FS que admiten el parámetro de inicio de sesión = símbolo del sistema.

- Paquete acumulativo de AD FS en Windows Server 2012 R2 con la de julio de 2016

- AD FS en Windows Server 2016

## <a name="how-do-to-configure-your-azure-ad-tenant-to-send-promptlogin-to-ad-fs"></a>Cómo hacer para configurar el inquilino de Azure AD para enviar la solicitud = iniciar sesión en AD FS

Usa el módulo de PowerShell de Azure AD para configurar la configuración.

> [!NOTE]
> La funcionalidad de inicio de sesión de pedir = (habilitada por la propiedad PromptLoginBehavior) actualmente solo está disponible en la [' módulo de Powershell de Azure AD de versión 1.0'](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185), en el que los cmdlets tienen nombres que incluyen "Msol", como Set-MsolDomainFederationSettings.  No está actualmente disponible a través de ' versión 2.0' Azure AD módulo de PowerShell, cuyas cmdlets tienen nombres como "establecer-AzureAD\ *".

Para configurar el símbolo del sistema = comportamiento de inicio de sesión, la sintaxis de cmdlet siguiente:

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

 
 Los valores de propiedad PreferredAuthenticationProtocol, SupportsMfa y PromptLoginBehavior pueden encontrarse mediante la visualización de la salida del cmdlet: ![Get MsolDomainFederationSettings](media/AD-FS-Prompt-Login/GetMsol.png)
```powershell
    Get-MsolDomainFederationSettings -DomainName <your_domain_name> | fl *
 ```
> [!NOTE]
> De manera predeterminada, cuando se ejecuta Get-MsolDomainFederationSettings, no se muestran algunas propiedades en la consola.  Para ver estos parámetros se recomienda que uses el | Florida * para forzar la salida de todas las propiedades del objeto.


El siguiente es obtener más información sobre el parámetro PromptLoginBehavior y su configuración.
   
   - <b>TranslateToFreshPasswordAuth</b> significa el comportamiento de Azure AD predeterminados de enviar <b>wauth</b> y <b>wfresh</b> a AD FS en lugar de símbolo del sistema = inicio de sesión
   - <b>NativeSupport</b> significa que el parámetro de inicio de sesión = símbolo del sistema se envía como en AD FS
   - <b>Deshabilitado</b> significa nada se enviará a AD FS

