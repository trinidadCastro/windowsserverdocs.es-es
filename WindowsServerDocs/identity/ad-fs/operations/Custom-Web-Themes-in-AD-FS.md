---
ms.assetid: 0379abc3-25c7-46ab-9a6b-80a5152365b0
title: Temas web personalizados en AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 69255eeaecd3e5198054242c1ab6dd1d0a58ce33
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816428"
---
# <a name="custom-web-themes-in-ad-fs"></a>Temas web personalizados en AD FS 

El tema que se envía\-de\-el cuadro de\-se llama predeterminado. Puedes exportar el tema predeterminado y usarlo para empezar rápidamente. Puedes personalizar la apariencia y el comportamiento (incluido el diseño, modificando el archivo .css), importar y aplicar el nuevo tema y, así, usar la apariencia y el comportamiento personalizados. Además, si usas el archivo .css, te será más fácil colaborar con los diseñadores web.  
  
El siguiente cmdlet crea un tema web personalizado duplicando el tema web predeterminado.  
  
  
`New-AdfsWebTheme –Name custom –SourceName default ` 

  
Puedes modificar el archivo .css y configurar el nuevo tema web con el nuevo archivo .css. Para exportar un tema web, usa el siguiente cmdlet.  
  

    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  

  
Para aplicar el archivo .css al nuevo tema, utiliza el siguiente cmdlet.  
  

    Set-AdfsWebTheme –TargetName custom –StyleSheet @{path="c:\NewTheme.css"}  
  
  
El siguiente cmdlet crea un tema web personalizado a partir de una hoja de estilos nueva.  
  
  
`New-AdfsWebTheme –Name custom –StyleSheet @{path="c:\NewTheme.css"} –RTLStyleSheetPath c:\NewRtlTheme.css ` 
  
  
  
Para aplicar el tema web personalizado a AD FS, use el siguiente cmdlet.  
  

`Set-AdfsWebConfig -ActiveThemeName custom`  

  
Para agregar JavaScript a AD FS, use el siguiente cmdlet.  
  
 
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=' /adfs/portal/script/onload.js';path="D:\inetpub\adfsassets\script\onload.js"}  


## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)  
