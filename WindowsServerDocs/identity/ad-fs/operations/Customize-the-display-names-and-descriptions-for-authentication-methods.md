---
ms.assetid: 309d6358-777d-496a-856d-728246c7d9a1
title: Personalizar los nombres para mostrar y descripciones para los métodos de autenticación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b9702873d42e0a72e510ac022d8d7fb04b45dab9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189167"
---
# <a name="customize-the-display-names-and-descriptions-for-authentication-methods"></a>Personalizar los nombres para mostrar y descripciones para los métodos de autenticación 


Para personalizar los nombres para mostrar y las descripciones de los métodos de autenticación se puede usar el cmdlt de PowerShell `Set-AdfsAuthenticationProviderWebContent` .  Para poder usar este cmdlt, primero debe obtener el nombre del método de autenticación que desea personalizar.  Esto puede hacerse mediante `Get-AdfsGlobalAuthenticationPolicy`.  En el siguiente ejemplo vemos que, en nuestra sesión\-en la página, se muestra lo siguiente:  “Sign in using an X.509 certificate”.  Queremos simplificar este proceso para los usuarios.  
  
![Personalizar displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update1.PNG)  
  
Para ello, obtenemos el nombre del método de autenticación y después editamos el texto mostrado.  
  
 
    Get-AdfsGlobalAuthenticationPolicy  
      
    AdditionalAuthenticationProvider  : {}  
    DeviceAuthenticationEnabled   : False  
    PrimaryIntranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    PrimaryExtranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    WindowsIntegratedFallbackEnabled  : True  
      
    Set-AdfsAuthenticationProviderWebContent -Name CertificateAuthentication -DisplayName "Sign in with a certificate"  
  
  
![Personalizar displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update2.PNG)  
  
Ahora vemos que el mensaje de la pantalla ha cambiado.  
  
![Personalizar displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update3.PNG)  

## <a name="additional-references"></a>Referencias adicionales 
[AD FS Sign-personalización de usuario](AD-FS-user-sign-in-customization.md) 
