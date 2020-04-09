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
ms.openlocfilehash: eab7062c6678c0e9f3ef970ef9cff97fa63dd868
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816398"
---
# <a name="customization-for-localization"></a>Personalización para la localización 


El contenido web se puede localizar a idiomas distintos del inglés. Al localizarlo, ten en cuenta las siguientes consideraciones.  
  
Después de personalizar el contenido, la personalización tendrá precedencia, así que deberás personalizarla para todos los idiomas que quieras admitir. Todo el contenido personalizado toma un parámetro de configuración regional. Al configurar contenido localizado, configúrelo con un país\-menos la configuración regional en primer lugar, por ejemplo, "en", antes de configurar un país y una región\-configuración regional específica, como "en\-US".  
  
A continuación se muestran varios ejemplos de código adicionales.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{Locale="";Path="c:\contoso.png"}  
      
    Set-AdfsWebTheme -TargetName default -Illustration @{Locale="";Path="c:\illustration.png"}  

  
A continuación se muestran varios ejemplos de código adicionales.  
  
 
    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" –locale "en"  
  
  

    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "Il s'agit de description de page erreur de Contoso" –locale "fr"  
 
  
Si desea personalizar el contenido web en idiomas distintos del inglés que requieran la entrada de Unicode, se recomienda usar el Windows PowerShell ISE. Para obtener más información, consulte [Introducción a Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md) 
