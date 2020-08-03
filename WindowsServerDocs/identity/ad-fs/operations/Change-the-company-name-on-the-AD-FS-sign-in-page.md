---
ms.assetid: 28043fc4-a34d-4710-ac3b-5c9d4d6a895c
title: Cambiar el nombre de la empresa en la página de inicio de sesión de AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3b6489ae115fb236e19214bceb291cd8f6dfacba
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2020
ms.locfileid: "87519814"
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
