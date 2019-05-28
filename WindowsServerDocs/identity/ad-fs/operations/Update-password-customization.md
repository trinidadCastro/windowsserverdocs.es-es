---
ms.assetid: 7e804590-6d6c-4cca-ac14-02d4dff06cec
title: Personalización de la contraseña de actualización
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e67c04c98a53f4f1db36e6586fa77bcf181a8d5a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188970"
---
# <a name="update-password-customization"></a>Personalización de la contraseña de actualización 


En algunos casos, es posible que los usuarios no puedan conectarse a la red corporativa para cambiar la contraseña de su cuenta. Este factor puede causar problemas, especialmente a los empleados remotos que vivan lejos de la oficina corporativa más cercana. En esos casos concretos, la página de actualización de la contraseña se puede usar con solo conectarse a Internet.  
  
Puedes personalizar la página de actualización de la contraseña proporcionando tu propia descripción de la página.  
  
> Para habilitar la página de actualización de la contraseña, ve a Administración de AD FS, en Extremos. El extremo para la actualización de la contraseña se encuentra en la parte inferior, en Otros: /adfs/portal/updatepassword/. Después de habilitar el extremo, debes reiniciar el servicio AD FS. Hay que hacerlo de forma manual. Luego, puedes ir a https://<fqdn>/adfs/portal/updatepassword/ desde un dispositivo unido a un área de trabajo y deberá aparecer la página de actualización de la contraseña.  
  
![actualización](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom5.png)  
  
## <a name="customize-the-update-password-page-description"></a>Personalización de la descripción de la página de actualización de la contraseña  
Para personalizar la descripción de la página de actualización de contraseña, use el siguiente cmdlet de Windows PowerShell y la sintaxis.  
  

    Set-AdfsGlobalWebContent -UpdatePasswordPageDescriptionText "This is the Contoso Update Password page."  

## <a name="additional-references"></a>Referencias adicionales 
[AD FS Sign-personalización de usuario](AD-FS-user-sign-in-customization.md)  
