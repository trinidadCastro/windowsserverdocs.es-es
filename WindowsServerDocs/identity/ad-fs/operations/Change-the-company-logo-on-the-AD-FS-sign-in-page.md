---
ms.assetid: f7f6bac2-1100-4b00-a248-4ca3eb3cdbe9
title: Cambiar el logotipo de la empresa en la página de inicio de sesión de AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/08/2017
ms.topic: article
ms.openlocfilehash: bc5bf9dcd0277980144d367e0bc539b555c05c83
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87956652"
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>Cambiar el logotipo de la empresa en la página de inicio de sesión de AD FS

## <a name="change-company-logo"></a>Cambio del logotipo de la compañía

Para cambiar el logotipo de la compañía que se muestra en la página de inicio \- de sesión, use el siguiente cmdlet de PowerShell de Windows PowerShell y la siguiente sintaxis.

![cambiar logotipo](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

> [!IMPORTANT]
> Recomendamos que las dimensiones del logotipo sean 260 × 35 a 96 ppp con un tamaño de archivo máximo de 10 kB.

```powershell
Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.png"}
```

> [!NOTE]
> El parámetro `TargetName` es obligatorio. El tema predeterminado que se publica con AD FS se denomina *default*.

## <a name="additional-references"></a>Referencias adicionales

[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)
