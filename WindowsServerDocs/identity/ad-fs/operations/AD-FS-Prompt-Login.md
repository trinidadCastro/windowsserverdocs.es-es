---
title: AD FS prompt = login
description: Preguntas más frecuentes sobre AD FS 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/27/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cc116701f9d03e6ca2e6d086ac7a93b0df595366
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866179"
---
# <a name="active-directory-federation-services-promptlogin-parameter-support"></a>Servicios de federación de Active Directory (AD FS) prompt = compatibilidad con parámetros de inicio de sesión

En el documento siguiente se describe la compatibilidad nativa para el parámetro prompt = login que está disponible en AD FS.

## <a name="what-is-promptlogin"></a>¿Qué es prompt = login?

Cuando las aplicaciones necesitan solicitar autenticación nueva desde Azure ad, lo que significa que necesitan Azure ad para volver a autenticar al usuario incluso si el usuario ya se ha autenticado, pueden enviar el `prompt=login` parámetro a Azure ad como parte de la autenticación. Solicite.

Cuando esta solicitud es para un usuario federado, Azure AD necesita informar al IdP, como AD FS, que la solicitud es para la autenticación nueva.

De forma predeterminada, Azure ad se traduce `prompt=login` en `wfresh=0` y `wauth=http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password` al enviar este tipo de solicitudes de autenticación al IDP federado.

Estos parámetros significan:

- `wfresh=0`: hacer la autenticación nueva
- `wauth=http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`: usar el nombre de usuario/contraseña para la solicitud de autenticación nueva

Esto puede producir problemas con escenarios de la intranet corporativa y de multi-factor Authentication en los que se desea un tipo de autenticación distinto del nombre `wauth` de usuario y la contraseña, tal y como solicita el parámetro.  

AD FS en Windows Server 2012 R2 con el paquete acumulativo de actualizaciones de julio de 2016 presentó `prompt=login` compatibilidad nativa con el parámetro. Esto significa que ahora Azure AD puede enviar este parámetro tal cual a AD FS servicio como parte de las solicitudes de autenticación Azure AD y Office 365.

## <a name="ad-fs-versions-that-support-promptlogin"></a>AD FS versiones que admiten prompt = login

A continuación se muestra una lista de las versiones de AD FS `prompt=login` que admiten el parámetro.

- AD FS en Windows Server 2012 R2 con el paquete acumulativo de actualizaciones de julio de 2016
- AD FS en Windows Server 2016

## <a name="how-to-configure-a-federated-domain-to-send-promptlogin-to-ad-fs"></a>Configuración de un dominio federado para enviar prompt = inicio de sesión en AD FS

Use el módulo de PowerShell de Azure AD para configurar el valor.

> [!NOTE]
> La `prompt=login` funcionalidad (habilitada por `PromptLoginBehavior` la propiedad) solo está disponible actualmente en la [versión 1,0 del módulo Azure ad PowerShell](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185), en el que los cmdlets tienen nombres que incluyen "msol", como set-MsolDomainFederationSettings.  Actualmente no está disponible a través de la versión 2,0 del módulo de PowerShell de Azure AD, cuyos cmdlets tienen nombres como "set-\*AzureAD".

1. En primer lugar, obtenga los `PreferredAuthenticationProtocol`valores `SupportsMfa`actuales de `PromptLoginBehavior` , y para el dominio federado mediante la ejecución del siguiente comando de PowerShell:

```powershell
    Get-MsolDomainFederationSettings -DomainName <your_domain_name> | Format-List *
```

> [!NOTE]
> La salida de `Get-MsolDomainFederationSettings` de forma predeterminada no muestra ciertas propiedades en la consola. Para ver todas las propiedades que se deben canalizar (`|`) en su salida a `Format-List *` para forzar la salida de todas las propiedades del objeto.

![Get-MsolDomainFederationSettings](media/AD-FS-Prompt-Login/GetMsol.png)

> [!NOTE]
> Si la `PreferredAuthenticationMethod` propiedad está vacía (`$null`) significa el comportamiento predeterminado de `TranslateToFreshPasswordAuth`.

2. Configure el valor deseado `PromptLoginBehavior` de ejecutando el comando siguiente:

```powershell
    Set-MsolDomainFederationSettings –DomainName <your_domain_name> -PreferredAuthenticationProtocol <current_value_from_step1> -SupportsMfa <current_value_from_step1> -PromptLoginBehavior <TranslateToFreshPasswordAuth|NativeSupport|Disabled>
```

A continuación se muestran los valores `PromptLoginBehavior` posibles del parámetro y su significado:

- **TranslateToFreshPasswordAuth**: indica el comportamiento predeterminado Azure ad de traducir `prompt=login` a `wauth=http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password` y `wfresh=0`.
- **NativeSupport**: significa que el `prompt=login` parámetro se enviará tal cual para AD FS. Este es el valor recomendado si AD FS está en Windows Server 2012 R2 con el paquete acumulativo de actualizaciones del 2016 de julio o superior.
- **Deshabilitado**: significa `wfresh=0` que solo se envía a AD FS.
