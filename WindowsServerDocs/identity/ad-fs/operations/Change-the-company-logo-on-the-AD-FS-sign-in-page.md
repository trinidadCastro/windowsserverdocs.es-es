---
description: Más información acerca de cómo cambiar el logotipo de la empresa en la página de inicio de sesión de AD FS
ms.assetid: f7f6bac2-1100-4b00-a248-4ca3eb3cdbe9
title: Cambiar el logotipo de la empresa en la página de inicio de sesión de AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/08/2017
ms.topic: article
ms.openlocfilehash: a04f7a81c26dea4234fbb92edc44125ae11e51f6
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97040283"
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
