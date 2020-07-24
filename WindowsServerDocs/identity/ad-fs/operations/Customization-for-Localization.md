---
ms.assetid: 38bbc002-a8fa-4211-9328-4ef67fca0acf
title: Personalización para la localización
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9a6bce383e04a892203c38cadc7ec2b7388b23a6
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965417"
---
# <a name="customization-for-localization"></a>Personalización para la localización 


El contenido web se puede localizar a idiomas distintos del inglés. Al localizarlo, ten en cuenta las siguientes consideraciones.  
  
Después de personalizar el contenido, la personalización tendrá precedencia, así que deberás personalizarla para todos los idiomas que quieras admitir. Todo el contenido personalizado toma un parámetro de configuración regional. Al configurar contenido localizado, configúrelo primero con un país \- menos local, por ejemplo, "en", antes de configurar una configuración regional específica de país y región, como \- "en \- EE. UU.".  
  
A continuación se muestran varios ejemplos de código adicionales.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{Locale="";Path="c:\contoso.png"}  
      
    Set-AdfsWebTheme -TargetName default -Illustration @{Locale="";Path="c:\illustration.png"}  

  
A continuación se muestran varios ejemplos de código adicionales.  
  
 
    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" –locale "en"  
  
  

    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "Il s'agit de description de page erreur de Contoso" –locale "fr"  
 
  
Si desea personalizar el contenido web en idiomas distintos del inglés que requieran la entrada de Unicode, se recomienda usar el Windows PowerShell ISE. Para obtener más información, consulte [Introducción a la Windows PowerShell ISE](/previous-versions/mt707506(v=msdn.10)).  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md) 
