---
ms.assetid: 309d6358-777d-496a-856d-728246c7d9a1
title: Personalizar los nombres para mostrar y las descripciones de los métodos de autenticación
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b599d5b1d9d66758f70b7e5ab8cab8f53c475788
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2020
ms.locfileid: "87519744"
---
# <a name="customize-the-display-names-and-descriptions-for-authentication-methods"></a>Personalizar los nombres para mostrar y las descripciones de los métodos de autenticación

Para personalizar los nombres para mostrar y las descripciones de los métodos de autenticación se puede usar el cmdlt de PowerShell `Set-AdfsAuthenticationProviderWebContent` .  Para poder usar este cmdlt, primero debe obtener el nombre del método de autenticación que desea personalizar.  Esto puede hacerse mediante `Get-AdfsGlobalAuthenticationPolicy`.  En el ejemplo siguiente, vemos que, en nuestra \- Página de inicio de sesión, se muestra lo siguiente: "iniciar sesión con un certificado X. 509".  Queremos simplificar este proceso para los usuarios.

![personalizar DisplayName](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update1.PNG)

Para ello, obtenemos el nombre del método de autenticación y después editamos el texto mostrado.

```powershell
Get-AdfsGlobalAuthenticationPolicy

AdditionalAuthenticationProvider  : {}
DeviceAuthenticationEnabled   : False
PrimaryIntranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}
PrimaryExtranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}
WindowsIntegratedFallbackEnabled  : True

Set-AdfsAuthenticationProviderWebContent -Name CertificateAuthentication -DisplayName "Sign in with a certificate"
 ```

![personalizar DisplayName](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update2.PNG)

Ahora vemos que el mensaje de la pantalla ha cambiado.

![personalizar DisplayName](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update3.PNG)

## <a name="additional-references"></a>Referencias adicionales

[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)
