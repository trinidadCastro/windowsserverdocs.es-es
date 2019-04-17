---
ms.assetid: 38bbc002-a8fa-4211-9328-4ef67fca0acf
title: "Personalización de la localización"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ac206d5aa8af970b65a014955ac66a8cf2835eb6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="customization-for-localization"></a>Personalización de la localización 

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Es posible localizar el contenido web a otros idiomas. Ten en cuenta las siguientes consideraciones cuando localizar.  
  
Una vez el contenido personalizado, la personalización tiene prioridad; por lo tanto, deberías personalizar para todos los idiomas que quieras admitir. Todo el contenido personalizado toma un parámetro de configuración regional. Al configurar el contenido localizado, configurarlo con una configuración regional de country\ menos en primer lugar, por ejemplo, "en", antes de configurar un país y la configuración regional específica region\ como "en\-us".  
  
El siguiente muestra algunos ejemplos de código adicional.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{Locale="";Path="c:\contoso.png"}  
      
    Set-AdfsWebTheme -TargetName default -Illustration @{Locale="";Path="c:\illustration.png"}  

  
El siguiente muestra algunos ejemplos de código adicional.  
  
 
    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" –locale "en"  
  
  

    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "Il s'agit de description de page erreur de Contoso" –locale "fr"  
 
  
Si quieres personalizar el contenido web en idiomas distintos del inglés que requiere la entrada de Unicode, te recomendamos que uses Windows PowerShell ISE. Para obtener más información, consulta [Introducción a Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión del usuario FS de anuncios](AD-FS-user-sign-in-customization.md) 
