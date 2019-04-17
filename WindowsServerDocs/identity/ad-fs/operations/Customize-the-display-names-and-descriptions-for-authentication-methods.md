---
ms.assetid: 309d6358-777d-496a-856d-728246c7d9a1
title: "Personalizar los nombres para mostrar y descripciones para métodos de autenticación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 699622a8a075dd6c78ab1b536dce2abfee642e9e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="customize-the-display-names-and-descriptions-for-authentication-methods"></a>Personalizar los nombres para mostrar y descripciones para métodos de autenticación 

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Para personalizar los nombres para mostrar y descripciones de los métodos de autenticación que puedes usar la `Set-AdfsAuthenticationProviderWebContent`cmdlt de PowerShell.  Para poder usar este cmdlt, primero debes obtener el nombre del método de autenticación que deseas personalizar.  Esto puede hacerse con `Get-AdfsGlobalAuthenticationPolicy`.  En el siguiente ejemplo, vemos que, en nuestra página sign\, se muestra lo siguiente: "iniciar sesión con un certificado X.509".  Queremos simplificar esto para los usuarios.  
  
![Personalizar displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update1.PNG)  
  
Por lo tanto, primero se obtiene el nombre del método de autenticación y, a continuación, podemos editar el texto mostrado.  
  
 
    Get-AdfsGlobalAuthenticationPolicy  
      
    AdditionalAuthenticationProvider  : {}  
    DeviceAuthenticationEnabled   : False  
    PrimaryIntranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    PrimaryExtranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    WindowsIntegratedFallbackEnabled  : True  
      
    Set-AdfsAuthenticationProviderWebContent -Name CertificateAuthentication -DisplayName "Sign in with a certificate"  
  
  
![Personalizar displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update2.PNG)  
  
Ahora vamos a ver que el mensaje de la pantalla ha cambiado.  
  
![Personalizar displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update3.PNG)  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión del usuario FS de anuncios](AD-FS-user-sign-in-customization.md) 
