---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: Agregar inicio de sesión\-en la descripción de la página
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 553cdcf2ac98b23d06cc43a64220cf8433117fc8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814016"
---
# <a name="add-sign-in-page-description"></a>Agregar inicio de sesión\-en la descripción de la página

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

## <a name="to-add-sign-in-page-description"></a>Agregar un inicio de sesión\-en la descripción de la página  
Para agregar un inicio de sesión\-en la descripción de la página para el inicio de sesión\-en la página, use el siguiente cmdlet de Windows PowerShell y la sintaxis.  

![agregar inicio de sesión en la descripción](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>" 
 
  
> [!IMPORTANT]  
> La cadena del parámetro `SignInPageDescriptionText` admite HTML puro con y sin las etiquetas. Por lo tanto, también puede ejecutar el siguiente cmdlet sin utilizar el &lt;p&gt; etiqueta.  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." ` 

después del signo\-en la página es personalizado, la personalización tendrá precedencia; por lo tanto, que deberás personalizarla para todos los idiomas que desea admitir. Todo el contenido personalizado toma un parámetro de configuración regional. Al configurar contenido localizado, debe configurarse con un país\-menos configuración regional en primer lugar, por ejemplo, "es-es", antes de configurar el país y región\-configuración regional específica, como "en\-nos".  

## <a name="additional-references"></a>Referencias adicionales 
[AD FS Sign-personalización de usuario](AD-FS-user-sign-in-customization.md)  
