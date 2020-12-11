---
description: Más información acerca de cómo cambiar el nombre de la empresa en la página de inicio de sesión de AD FS
ms.assetid: 28043fc4-a34d-4710-ac3b-5c9d4d6a895c
title: Cambiar el nombre de la empresa en la página de inicio de sesión de AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 907043517d56790cd4f6de49d67de5ed3905f0ad
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97040253"
---
# <a name="change-the-company-name-on-the-ad-fs-sign-in-page"></a>Cambiar el nombre de la empresa en la página de inicio de sesión de AD FS

Para cambiar el nombre de la empresa que se muestra en la página de inicio \- de sesión, use el siguiente cmdlet de Windows PowerShell y la siguiente sintaxis. Este valor se establece de forma predeterminada con el valor del nombre para mostrar del Servicio de federación que introdujiste durante la configuración.

![cambiar nombre](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png)

```powershell
    Set-AdfsGlobalWebContent –CompanyName "Contoso Corp"
```

> [!NOTE]
> También puede usar el entorno de scripting integrado de Windows PowerShell \( ISE \) para cambiar el nombre de la empresa. Mediante el Windows PowerShell ISE, puede mostrar el contenido en un \- entorno compatible con Unicode. Para obtener más información, consulte [Introducción a Windows PowerShell ISE](/previous-versions/mt707506(v=msdn.10)).

## <a name="additional-references"></a>Referencias adicionales

- [Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)
