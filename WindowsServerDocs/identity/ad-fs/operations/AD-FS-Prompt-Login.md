---
title: AD FS prompt = login
description: Preguntas más frecuentes sobre AD FS 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/27/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: cb91bb61adf97fee6f157ca44eb657e20670a1e7
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75948683"
---
# <a name="active-directory-federation-services-promptlogin-parameter-support"></a>Servicios de federación de Active Directory (AD FS) prompt = compatibilidad con parámetros de inicio de sesión

En el documento siguiente se describe la compatibilidad nativa para el parámetro prompt = login que está disponible en AD FS.

## <a name="what-is-promptlogin"></a>¿Qué es prompt = login?

Cuando las aplicaciones necesitan solicitar autenticación nueva desde Azure AD, lo que significa que necesitan Azure AD para volver a autenticar al usuario incluso si el usuario ya se ha autenticado, pueden enviar el parámetro `prompt=login` a Azure AD como parte de la solicitud de autenticación.

Cuando esta solicitud es para un usuario federado, Azure AD necesita informar al IdP, como AD FS, que la solicitud es para la autenticación nueva.

De forma predeterminada, Azure AD traduce `prompt=login` a `wfresh=0` y `wauth=https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password` al enviar este tipo de solicitudes de autenticación al IdP federado.

Estos parámetros significan:

- `wfresh=0`: realizar la autenticación nueva
- `wauth=https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`: usar el nombre de usuario/contraseña para la solicitud de autenticación nueva

Esto puede producir problemas con escenarios de la intranet corporativa y de multi-factor Authentication en los que se desea un tipo de autenticación distinto del nombre de usuario y la contraseña, tal y como solicita el parámetro `wauth`.  

AD FS en Windows Server 2012 R2 con el paquete acumulativo de actualizaciones de julio de 2016 presentó compatibilidad nativa con el parámetro `prompt=login`. Esto significa que ahora Azure AD puede enviar este parámetro tal cual a AD FS servicio como parte de las solicitudes de autenticación Azure AD y Office 365.

## <a name="ad-fs-versions-that-support-promptlogin"></a>AD FS versiones que admiten prompt = login

A continuación se muestra una lista de las versiones de AD FS que admiten el parámetro `prompt=login`.

- AD FS en Windows Server 2012 R2 con el paquete acumulativo de actualizaciones de julio de 2016
- AD FS en Windows Server 2016

## <a name="how-to-configure-a-federated-domain-to-send-promptlogin-to-ad-fs"></a>Configuración de un dominio federado para enviar prompt = inicio de sesión en AD FS

Use el módulo de PowerShell de Azure AD para configurar el valor.

> [!NOTE]
> La capacidad de `prompt=login` (habilitada por la propiedad `PromptLoginBehavior`) solo está disponible en la [versión 1,0 del módulo de Azure ad PowerShell](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185), en la que los cmdlets tienen nombres que incluyen "msol", como set-MsolDomainFederationSettings.  Actualmente no está disponible a través de la versión 2,0 del módulo de PowerShell de Azure AD, cuyos cmdlets tienen nombres como "Set-AzureAD\*".

1. En primer lugar, obtenga los valores actuales de `PreferredAuthenticationProtocol`, `SupportsMfa`y `PromptLoginBehavior` para el dominio federado mediante la ejecución del siguiente comando de PowerShell:

```powershell
    Get-MsolDomainFederationSettings -DomainName <your_domain_name> | Format-List *
```

> [!NOTE]
> La salida de `Get-MsolDomainFederationSettings` de forma predeterminada no muestra ciertas propiedades en la consola. Para ver todas las propiedades, debe canalizar (`|`) su salida a `Format-List *` para forzar la salida de todas las propiedades del objeto.

![Get-MsolDomainFederationSettings](media/AD-FS-Prompt-Login/GetMsol.png)

> [!NOTE]
> Si la propiedad `PreferredAuthenticationMethod` está vacía (`$null`) significa el comportamiento predeterminado de `TranslateToFreshPasswordAuth`.

2. Configure el valor deseado de `PromptLoginBehavior` ejecutando el comando siguiente:

```powershell
    Set-MsolDomainFederationSettings –DomainName <your_domain_name> -PreferredAuthenticationProtocol <current_value_from_step1> -SupportsMfa <current_value_from_step1> -PromptLoginBehavior <TranslateToFreshPasswordAuth|NativeSupport|Disabled>
```

A continuación se muestran los posibles valores de `PromptLoginBehavior` parámetro y su significado:

- **TranslateToFreshPasswordAuth**: indica el comportamiento predeterminado Azure ad de traducir `prompt=login` a `wauth=https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password` y `wfresh=0`.
- **NativeSupport**: significa que el parámetro `prompt=login` se enviará tal y como se va a AD FS. Este es el valor recomendado si AD FS está en Windows Server 2012 R2 con el paquete acumulativo de actualizaciones del 2016 de julio o superior.
- **Deshabilitado**: significa que solo `wfresh=0` se envía a AD FS.
