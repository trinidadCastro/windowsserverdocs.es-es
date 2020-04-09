---
ms.assetid: f7f6bac2-1100-4b00-a248-4ca3eb3cdbe9
title: Cambiar el logotipo de la empresa en la página de inicio de sesión de AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/08/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d12429b22265495cb8168ce3e5993a5cf3e74a0c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859978"
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
