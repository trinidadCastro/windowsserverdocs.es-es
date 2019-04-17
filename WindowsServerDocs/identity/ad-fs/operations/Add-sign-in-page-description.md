---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: "Agregar sign\\ en la página Descripción"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 128a4ffd8d4b9dfcfe5f0e8e2df8a0e1505cbb24
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="add-sign-in-page-description"></a>Agregar sign\ en la página Descripción

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

## <a name="to-add-sign-in-page-description"></a>Para agregar la descripción de la página de inicio de sesión sign\  
Para agregar una descripción sign\ en la página a la página de sign\, usa el siguiente cmdlet de PowerShell de Windows PowerShell y la sintaxis.  

![Agregar el registro de descripción](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>" 
 
  
> [!IMPORTANT]  
> La cadena para la `SignInPageDescriptionText`parámetro compatible con ambas HTML puro con las etiquetas y sin. Por lo tanto, también puedes ejecutar el cmdlet siguiente sin usar el &lt;p&gt; etiqueta.  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." ` 

Una vez la página de sign\ personalizada, la personalización tiene prioridad; por lo tanto, deberías personalizar para todos los idiomas que quieras admitir. Todo el contenido personalizado toma un parámetro de configuración regional. Al configurar el contenido localizado, debe estar configurado con una configuración regional de country\ menos en primer lugar, por ejemplo, "en", antes de configurar el país y la configuración regional de region\ específicos, como "en\-us".  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión del usuario FS de anuncios](AD-FS-user-sign-in-customization.md)  
