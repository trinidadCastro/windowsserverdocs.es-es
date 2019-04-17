---
ms.assetid: 7e804590-6d6c-4cca-ac14-02d4dff06cec
title: "Personalización de la contraseña de actualización"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4b06992bfb398b66988ad4882217a8a83738365e
ms.sourcegitcommit: 78d8839ccafa9530784cb9e38c3127ed2c215423
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2018
---
# <a name="update-password-customization"></a>Personalización de la contraseña de actualización 

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

En algunos casos, es posible que los usuarios no podrán conectarse a la red corporativa para cambiar su contraseña de cuenta. Este factor puede resultar problemático especialmente para empleados remotos que es posible que viven lejos de la oficina corporativa. Para estos casos específicos, la página de la contraseña de actualización puede usarse por sólo se conectan a Internet.  
  
Puedes personalizar la página de la contraseña de actualización al proporcionar la descripción de la página.  
  
> Para habilitar la página de actualización de la contraseña, ve a la administración de AD FS en extremos. El extremo para actualizar la contraseña se encuentra en la parte inferior en otros - / adfs/portal/updatepassword /. Una vez que has habilitado el punto de conexión, debes reiniciar el servicio de AD FS. Esto debe hacerse manualmente. A continuación, puedes navegar a https://<fqdn>/adfs/portal/updatepassword o en un área de trabajo Unidos a un dispositivo y deberías ver la página de contraseña de la actualización.  
  
![actualización](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom5.png)  
  
## <a name="customize-the-update-password-page-description"></a>Personalizar la descripción de la página de actualizar la contraseña  
Para personalizar la descripción de la página de actualización de contraseña, usa el siguiente cmdlet de PowerShell de Windows y la sintaxis.  
  

    Set-AdfsGlobalWebContent -UpdatePasswordPageDescriptionText "This is the Contoso Update Password page."  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión del usuario FS de anuncios](AD-FS-user-sign-in-customization.md)  
