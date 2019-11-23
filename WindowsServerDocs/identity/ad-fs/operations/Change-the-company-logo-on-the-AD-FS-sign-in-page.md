---
ms.assetid: f7f6bac2-1100-4b00-a248-4ca3eb3cdbe9
title: Cambiar el logotipo de la empresa en la página de inicio de sesión de AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/08/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b22c969e0113081e1ca8a662ae81a2ee24829835
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358306"
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>Cambiar el logotipo de la empresa en la página de inicio de sesión de AD FS

#### <a name="change-company-logo"></a>Cambio del logotipo de la compañía  
Para cambiar el logotipo de la compañía que se muestra en la página firmar\-en, use el siguiente cmdlet de PowerShell de Windows PowerShell y la siguiente sintaxis.  

![cambiar logotipo](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> Recomendamos que las dimensiones del logotipo sean 260 × 35 a 96 ppp con un tamaño de archivo máximo de 10 kB.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.png"}  

  
> [!NOTE]  
> El parámetro `TargetName` es obligatorio. El tema predeterminado que se publica con AD FS se denomina *default*.  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)  
