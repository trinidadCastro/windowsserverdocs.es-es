---
ms.assetid: 7e804590-6d6c-4cca-ac14-02d4dff06cec
title: Actualización de la personalización de contraseñas
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e007d4449cb62e7888c30f5b5929e393d7b571ef
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407457"
---
# <a name="update-password-customization"></a>Actualización de la personalización de contraseñas 


En algunos casos, es posible que los usuarios no puedan conectarse a la red corporativa para cambiar la contraseña de su cuenta. Este factor puede causar problemas, especialmente a los empleados remotos que vivan lejos de la oficina corporativa más cercana. En esos casos concretos, la página de actualización de la contraseña se puede usar con solo conectarse a Internet.  
  
Puedes personalizar la página de actualización de la contraseña proporcionando tu propia descripción de la página.  
  
> Para habilitar la página de actualización de la contraseña, ve a Administración de AD FS, en Extremos. El extremo para la actualización de la contraseña se encuentra en la parte inferior, en Otros: /adfs/portal/updatepassword/. Después de habilitar el extremo, debes reiniciar el servicio AD FS. Hay que hacerlo de forma manual. Luego, puedes ir a https://<fqdn>/adfs/portal/updatepassword/ desde un dispositivo unido a un área de trabajo y deberá aparecer la página de actualización de la contraseña.  
  
![actualización](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom5.png)  
  
## <a name="customize-the-update-password-page-description"></a>Personalización de la descripción de la página de actualización de la contraseña  
Para personalizar la descripción de la página de actualización de la contraseña, use el siguiente cmdlet de Windows PowerShell y la siguiente sintaxis.  
  

    Set-AdfsGlobalWebContent -UpdatePasswordPageDescriptionText "This is the Contoso Update Password page."  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)  
