---
description: Más información acerca de cómo cambiar la ilustración en la página de inicio de sesión de AD FS
ms.assetid: a4526500-24b3-423d-805c-24b0d8061aba
title: Cambiar la ilustración de la página de inicio de sesión de AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 30f6b4cc31869b06f57939123b5feca9371715a3
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97040223"
---
# <a name="change-the-illustration-on-the-ad-fs-sign-in-page"></a>Cambiar la ilustración de la página de inicio de sesión de AD FS

## <a name="change-the-illustration"></a>Cambiar la ilustración

Para cambiar la ilustración, el gráfico de la izquierda, que se muestra en la página de inicio \- de sesión, use el siguiente cmdlet de Windows PowerShell y la siguiente sintaxis.

![cambiar ilustración](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

> [!IMPORTANT]
> Recomendamos que las dimensiones de la ilustración sean 1420 × 1080 píxeles a 96 ppp con un tamaño de archivo máximo de 200 kB.

```powershell
Set-AdfsWebTheme -TargetName default -Illustration @{path="c:\Contoso\illustration.png"}
```

## <a name="additional-references"></a>Referencias adicionales

[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)
