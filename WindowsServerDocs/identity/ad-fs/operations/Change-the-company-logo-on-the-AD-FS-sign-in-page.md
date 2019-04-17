---
ms.assetid: f7f6bac2-1100-4b00-a248-4ca3eb3cdbe9
title: "Cambiar el logotipo de la empresa en la página de inicio de sesión de AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ca54abe7fe852b22f2f4d9a717e38d219fa50694
ms.sourcegitcommit: a00fc4426dc4ad0098257f01f0124d6c733d1aef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2018
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>Cambiar el logotipo de la empresa en la página de inicio de sesión de AD FS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

#### <a name="change-company-logo"></a>Cambiar el logotipo de compañía  
Para cambiar el logotipo de la empresa que se muestra en la página de sign\, usa el siguiente cmdlet de PowerShell de Windows PowerShell y la sintaxis.  

![cambiar el logotipo de](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> Te recomendamos que las dimensiones del logotipo sea 260 x 350 @ 96 PPP con un tamaño de archivo no superior a 10 KB.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.png"}  

  
> [!NOTE]  
> La `TargetName` parámetro es necesario. El tema predeterminado que se lanza con AD FS se denomina *predeterminado*.  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión del usuario FS de anuncios](AD-FS-user-sign-in-customization.md)  
