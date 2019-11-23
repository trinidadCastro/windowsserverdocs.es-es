---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: Agregar\-de signo en la descripción de la página
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3b34a4e54aebd5b9dc3655eecd770a25f7ea97cf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407744"
---
# <a name="add-sign-in-page-description"></a>Agregar\-de signo en la descripción de la página


## <a name="to-add-sign-in-page-description"></a>Para agregar\-de signo en la descripción de la página  
Para agregar un signo\-en la descripción de la página a la página firmar\-de, use el siguiente cmdlet de Windows PowerShell y la siguiente sintaxis.  

![Agregar Descripción de inicio de sesión](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>" 
 
  
> [!IMPORTANT]  
> La cadena del parámetro `SignInPageDescriptionText` admite HTML puro con y sin las etiquetas. Por lo tanto, también puede ejecutar el siguiente cmdlet sin utilizar la etiqueta &lt;p&gt;.  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." ` 

una vez que se ha personalizado el signo\-en la página, la personalización tiene prioridad; por lo tanto, debe personalizar para todos los idiomas que desee admitir. Todo el contenido personalizado toma un parámetro de configuración regional. Al configurar contenido localizado, debe configurarse con un país\-menos la configuración regional, por ejemplo, "en", antes de configurar el país y la región\-configuración regional específica, como "en\-US".  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)  
