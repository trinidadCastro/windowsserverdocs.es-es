---
ms.assetid: 7e804590-6d6c-4cca-ac14-02d4dff06cec
title: Actualización de la personalización de contraseñas
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: c47649e5a60de646de331a6c723c18c3b041bca6
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87967012"
---
# <a name="update-password-customization"></a>Actualización de la personalización de contraseñas

En algunos casos, es posible que los usuarios no puedan conectarse a la red corporativa para cambiar la contraseña de su cuenta. Este factor puede causar problemas, especialmente a los empleados remotos que vivan lejos de la oficina corporativa más cercana. En esos casos concretos, la página de actualización de la contraseña se puede usar con solo conectarse a Internet.

Puedes personalizar la página de actualización de la contraseña proporcionando tu propia descripción de la página.

Para habilitar la página de actualización de la contraseña, ve a Administración de AD FS, en Extremos. El extremo para la actualización de la contraseña se encuentra en la parte inferior, en Otros: /adfs/portal/updatepassword/. Después de habilitar el extremo, debes reiniciar el servicio AD FS. Hay que hacerlo de forma manual. Si espera usar la página web actualizar contraseña externamente y, al usar el proxy de aplicación Web, en la misma opción debe habilitarla en el proxy (habilitar en el proxy). Después, puede ir a `https://<fqdn>/adfs/portal/updatepassword/` en un dispositivo unido al área de trabajo y debería ver la página actualizar contraseña.

![update](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom5.png)

## <a name="customize-the-update-password-page-description"></a>Personalización de la descripción de la página de actualización de la contraseña

Para personalizar la descripción de la página de actualización de la contraseña, use el siguiente cmdlet de Windows PowerShell y la siguiente sintaxis.

```powershell
Set-AdfsGlobalWebContent -UpdatePasswordPageDescriptionText "This is the Contoso Update Password page."
```

## <a name="additional-references"></a>Referencias adicionales

[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)
