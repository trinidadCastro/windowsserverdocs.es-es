---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: Agregar signo @ no__t-0in Descripción de la página
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
# <a name="add-sign-in-page-description"></a>Agregar signo @ no__t-0in Descripción de la página


## <a name="to-add-sign-in-page-description"></a>Para agregar la descripción de la página Sign @ no__t-0in  
Para agregar una descripción de la página Sign @ no__t-0in a la página Sign @ no__t-1in, use el siguiente cmdlet de Windows PowerShell y la siguiente sintaxis.  

![Agregar Descripción de inicio de sesión](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>" 
 
  
> [!IMPORTANT]  
> La cadena del parámetro `SignInPageDescriptionText` admite HTML puro con y sin las etiquetas. Por lo tanto, también puede ejecutar el siguiente cmdlet sin usar la etiqueta &lt;P @ no__t-1.  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." ` 

Después de personalizar la página Sign @ no__t-0in, la personalización tiene prioridad; por lo tanto, debe personalizar para todos los idiomas que desee admitir. Todo el contenido personalizado toma un parámetro de configuración regional. Al configurar contenido localizado, debe configurarse con una configuración regional de país @ no__t-0less en primer lugar, por ejemplo, "en", antes de configurar la configuración regional de país y región @ no__t-1specific, como "en @ no__t-2US".  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)  
