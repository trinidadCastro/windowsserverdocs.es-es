---
ms.assetid: 38bbc002-a8fa-4211-9328-4ef67fca0acf
title: Personalización para la localización
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3cf209756c27c72836c7e2e1e58e84f3f5af2ca9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189178"
---
# <a name="customization-for-localization"></a>Personalización para la localización 


El contenido web se puede localizar a idiomas distintos del inglés. Al localizarlo, ten en cuenta las siguientes consideraciones.  
  
Después de personalizar el contenido, la personalización tendrá precedencia, así que deberás personalizarla para todos los idiomas que quieras admitir. Todo el contenido personalizado toma un parámetro de configuración regional. Al configurar contenido localizado, configurarlo con un país\-menos configuración regional en primer lugar, por ejemplo, "es-es", antes de configurar un país y región\-configuración regional específica, como "en\-nos".  
  
A continuación se muestran varios ejemplos de código adicionales.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{Locale="";Path="c:\contoso.png"}  
      
    Set-AdfsWebTheme -TargetName default -Illustration @{Locale="";Path="c:\illustration.png"}  

  
A continuación se muestran varios ejemplos de código adicionales.  
  
 
    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" –locale "en"  
  
  

    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "Il s'agit de description de page erreur de Contoso" –locale "fr"  
 
  
Si desea personalizar el contenido web a idiomas distintos del inglés que requieran Unicode, se recomienda que use Windows PowerShell ISE. Para obtener más información, consulte [Introducción a Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Referencias adicionales 
[AD FS Sign-personalización de usuario](AD-FS-user-sign-in-customization.md) 
