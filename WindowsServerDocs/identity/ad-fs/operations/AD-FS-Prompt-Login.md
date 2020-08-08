---
title: AD FS prompt = login
description: Preguntas más frecuentes sobre AD FS 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/27/2017
ms.topic: article
ms.custom: it-pro
ms.openlocfilehash: 0af0d71ad2b373f755653898ab0697b0308faa4b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940308"
---
# <a name="active-directory-federation-services-promptlogin-parameter-support"></a>Compatibilidad con el parámetro prompt=login de Servicios de federación de Active Directory

En el documento siguiente se describe la compatibilidad nativa para el parámetro prompt = login que está disponible en AD FS.

## <a name="what-is-promptlogin"></a>¿Qué es prompt = login?

Cuando las aplicaciones necesitan solicitar autenticación nueva desde Azure AD, lo que significa que necesitan Azure AD para volver a autenticar al usuario incluso si el usuario ya se ha autenticado, pueden enviar el `prompt=login` parámetro a Azure ad como parte de la solicitud de autenticación.

Cuando esta solicitud es para un usuario federado, Azure AD necesita informar al IdP, como AD FS, que la solicitud es para la autenticación nueva.

De forma predeterminada, Azure AD se traduce `prompt=login` en `wfresh=0` y `wauth=https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password` al enviar este tipo de solicitudes de autenticación al IDP federado.

Estos parámetros significan:

- `wfresh=0`: hacer la autenticación nueva
- `wauth=https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`: usar el nombre de usuario/contraseña para la solicitud de autenticación nueva

Esto puede producir problemas con escenarios de la intranet corporativa y de multi-factor Authentication en los que se desea un tipo de autenticación distinto del nombre de usuario y la contraseña, tal y como solicita el `wauth` parámetro.

AD FS en Windows Server 2012 R2 con el paquete acumulativo de actualizaciones de julio de 2016 presentó compatibilidad nativa con el `prompt=login` parámetro. Esto significa que ahora Azure AD puede enviar este parámetro tal cual a AD FS servicio como parte de las solicitudes de autenticación Azure AD y Office 365.

## <a name="ad-fs-versions-that-support-promptlogin"></a>AD FS versiones que admiten prompt = login

A continuación se muestra una lista de las versiones de AD FS que admiten el `prompt=login` parámetro.

- AD FS en Windows Server 2012 R2 con el paquete acumulativo de actualizaciones de julio de 2016
- AD FS en Windows Server 2016

## <a name="how-to-configure-a-federated-domain-to-send-promptlogin-to-ad-fs"></a>Configuración de un dominio federado para enviar prompt = inicio de sesión en AD FS

Use el módulo de PowerShell de Azure AD para configurar el valor.

> [!NOTE]
> La `prompt=login` funcionalidad (habilitada por la `PromptLoginBehavior` propiedad) solo está disponible actualmente en la [versión 1,0 del módulo Azure ad PowerShell](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185), en el que los cmdlets tienen nombres que incluyen "msol", como set-MsolDomainFederationSettings.  Actualmente no está disponible a través de la versión 2,0 del módulo de PowerShell de Azure AD, cuyos cmdlets tienen nombres como "Set-AzureAD \* ".

1. En primer lugar, obtenga los valores actuales de `PreferredAuthenticationProtocol` , `SupportsMfa` y `PromptLoginBehavior` para el dominio federado mediante la ejecución del siguiente comando de PowerShell:

```powershell
    Get-MsolDomainFederationSettings -DomainName <your_domain_name> | Format-List *
```

> [!NOTE]
> La salida de de `Get-MsolDomainFederationSettings` forma predeterminada no muestra ciertas propiedades en la consola. Para ver todas las propiedades que se deben canalizar ( `|` ) en su salida a `Format-List *` para forzar la salida de todas las propiedades del objeto.

![Get-MsolDomainFederationSettings](media/AD-FS-Prompt-Login/GetMsol.png)

> [!NOTE]
> Si el valor de la propiedad `PromptLoginBehavior` está vacío ( `$null` ), se utiliza el comportamiento de `TranslateToFreshPasswordAuth` .

2. Configure el valor deseado de `PromptLoginBehavior` ejecutando el comando siguiente:

```powershell
    Set-MsolDomainFederationSettings –DomainName <your_domain_name> -PreferredAuthenticationProtocol <current_value_from_step1> -SupportsMfa <current_value_from_step1> -PromptLoginBehavior <TranslateToFreshPasswordAuth|NativeSupport|Disabled>
```

A continuación se muestran los valores posibles del `PromptLoginBehavior` parámetro y su significado:

- **TranslateToFreshPasswordAuth**: indica el comportamiento predeterminado Azure ad de traducir `prompt=login` a `wauth=https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password` y `wfresh=0` .
- **NativeSupport**: significa que el `prompt=login` parámetro se enviará tal cual para AD FS. Este es el valor recomendado si AD FS está en Windows Server 2012 R2 con el paquete acumulativo de actualizaciones del 2016 de julio o superior.
- **Deshabilitado**: significa que solo `wfresh=0` se envía a AD FS.
