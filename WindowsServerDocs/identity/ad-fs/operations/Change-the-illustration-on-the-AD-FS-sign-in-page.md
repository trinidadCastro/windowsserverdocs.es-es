---
ms.assetid: a4526500-24b3-423d-805c-24b0d8061aba
title: "Cambiar la ilustración en la página de inicio de sesión de AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ac5e60aaad864248b58a3908e7aa9622165fbc14
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="change-the-illustration-on-the-ad-fs-sign-in-page"></a>Cambiar la ilustración en la página de inicio de sesión de AD FS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

## <a name="change-the-illustration"></a>Cambiar la ilustración  


Para cambiar la ilustración, el gráfico a la izquierda, que se muestra en la página de sign\, usa el siguiente cmdlet de PowerShell de Windows PowerShell y la sintaxis.  

![ilustración de cambiar](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> Te recomendamos que las dimensiones de la ilustración 1420 x 1080 píxeles @ 96 PPP con un tamaño de archivo no superior a 200 KB.  
  
 
    Set-AdfsWebTheme -TargetName default -Illustration @{path="c:\Contoso\illustration.png"}  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión del usuario FS de anuncios](AD-FS-user-sign-in-customization.md)  
  
  
