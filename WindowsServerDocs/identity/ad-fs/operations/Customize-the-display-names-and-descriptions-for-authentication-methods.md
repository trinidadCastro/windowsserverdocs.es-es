---
ms.assetid: 309d6358-777d-496a-856d-728246c7d9a1
title: Personalizar los nombres para mostrar y las descripciones de los métodos de autenticación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: cc0da10858ca6924a516fbf825206e376294209d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407563"
---
# <a name="customize-the-display-names-and-descriptions-for-authentication-methods"></a>Personalizar los nombres para mostrar y las descripciones de los métodos de autenticación 


Para personalizar los nombres para mostrar y las descripciones de los métodos de autenticación se puede usar el cmdlt de PowerShell `Set-AdfsAuthenticationProviderWebContent` .  Para poder usar este cmdlt, primero debe obtener el nombre del método de autenticación que desea personalizar.  Esto puede hacerse mediante `Get-AdfsGlobalAuthenticationPolicy`.  En el ejemplo siguiente, vemos que, en nuestra\-de firma en la página, se muestra lo siguiente: "iniciar sesión con un certificado X. 509".  Queremos simplificar este proceso para los usuarios.  
  
![personalizar DisplayName](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update1.PNG)  
  
Para ello, obtenemos el nombre del método de autenticación y después editamos el texto mostrado.  
  
 
    Get-AdfsGlobalAuthenticationPolicy  
      
    AdditionalAuthenticationProvider  : {}  
    DeviceAuthenticationEnabled   : False  
    PrimaryIntranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    PrimaryExtranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    WindowsIntegratedFallbackEnabled  : True  
      
    Set-AdfsAuthenticationProviderWebContent -Name CertificateAuthentication -DisplayName "Sign in with a certificate"  
  
  
![personalizar DisplayName](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update2.PNG)  
  
Ahora vemos que el mensaje de la pantalla ha cambiado.  
  
![personalizar DisplayName](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update3.PNG)  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md) 
