---
ms.assetid: f7f6bac2-1100-4b00-a248-4ca3eb3cdbe9
title: Cambiar el logotipo de empresa en la página de inicio de sesión de AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fe5c138466ea288b5dfb8c7c284603150ab9d874
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190029"
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>Cambiar el logotipo de empresa en la página de inicio de sesión de AD FS

#### <a name="change-company-logo"></a>Cambio del logotipo de la compañía  
Para cambiar el logotipo de la compañía que se muestra en el inicio de sesión\-en la página, use el siguiente cmdlet de PowerShell de Windows PowerShell y la sintaxis.  

![cambiar el logotipo de](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> Recomendamos que las dimensiones del logotipo sean 260 × 35 a 96 ppp con un tamaño de archivo máximo de 10 kB.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.png"}  

  
> [!NOTE]  
> El parámetro `TargetName` es obligatorio. El tema predeterminado que se incluyó con AD FS se denomina *predeterminada*.  

## <a name="additional-references"></a>Referencias adicionales 
[AD FS Sign-personalización de usuario](AD-FS-user-sign-in-customization.md)  
