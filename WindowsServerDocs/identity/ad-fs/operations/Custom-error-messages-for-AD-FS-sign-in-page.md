---
ms.assetid: 1df78c2a-5054-4b54-8310-c48ea62e6e0b
title: "Mensajes de error personalizados para la página de inicio de sesión de AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3a015c27f784d5b1f488f984450612820d4a06b1
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="custom-error-messages-for-ad-fs-sign-in-page"></a>Mensajes de error personalizados para la página de inicio de sesión de AD FS  

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Puedes configurar mensajes de error personalizados que se pueden adaptar a su organización. La siguiente ilustración muestra una descripción de la página de error personalizados y un mensaje de error genérica. Usa los siguientes cmdlets de Windows PowerShell para personalizar los mensajes de error.  
  
![error personalizada](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom3.png)  
  
## <a name="customize-the-error-page-description"></a>Personalizar la descripción de la página de error  
Para personalizar la descripción de la página de error, usa el siguiente cmdlet de PowerShell de Windows y la sintaxis.  
  

`Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" ` 

  
## <a name="customize-a-generic-error-message"></a>Personalizar un mensaje de error genérica  
Para personalizar el mensaje de error genérico, usa el siguiente cmdlet de PowerShell de Windows y la sintaxis.  
  
 
`Set-AdfsGlobalWebContent -ErrorPageGenericErrorMessage "This is a generic error message.  Contact Contoso IT for assistance." ` 

  
## <a name="customize-an-authorization-error-message"></a>Personalizar un mensaje de error de autorización  
Para personalizar el mensaje de error de autorización, usa el siguiente cmdlet de PowerShell de Windows y la sintaxis.  
  

    Set-AdfsGlobalWebContent -ErrorPageAuthorizationErrorMessage "You have received an Authorization error.  Contact Contoso IT for assistance."  

  
## <a name="customize-a-device-authentication-error-message"></a>Personalizar un mensaje de error de autenticación de dispositivo  
Para personalizar el mensaje de error de autenticación de dispositivo, usa el siguiente cmdlet de PowerShell de Windows y la sintaxis.  
  
 
`Set-AdfsGlobalWebContent -ErrorPageDeviceAuthenticationErrorMessage "Your device is not authorized.  Contact Contoso IT for assistance."`  
 
  
## <a name="customize-a-support-email-error-message"></a>Personalizar un mensaje de error de correo electrónico de soporte técnico  
Puedes configurar una dirección de correo electrónico de soporte técnico en AD FS. Si ha configurado, AD FS muestra automáticamente un vínculo para que los usuarios finales a los detalles del error de correo electrónico.  
  
Para personalizar el mensaje de error de correo electrónico de soporte técnico, usa el siguiente cmdlet de PowerShell de Windows y la sintaxis.  
  

    Set-AdfsGlobalWebContent -ErrorPageSupportEmail  "admin@contoso.com"  

  
## <a name="customize-a-relying-party-authorization-message"></a>Personalizar un mensaje de autorización de terceros usuario de confianza  
Puedes configurar un mensaje de error de autorización de usuario de confianza de las partes en AD FS.  
  
Para personalizar el mensaje de error de terceros confianza, usa el siguiente cmdlet de PowerShell de Windows y la sintaxis.  

    Set-AdfsRelyingPartyWebContent -Name fedpassive -ErrorPageAuthorizationErrorMessage "<p> You need to be a member of Security Auditors to access this site. Click <A href='http://accessrequest/'>here</A> for more information.</p>“  


## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión del usuario FS de anuncios](AD-FS-user-sign-in-customization.md)    
