---
ms.assetid: 0379abc3-25c7-46ab-9a6b-80a5152365b0
title: Temas de Web personalizados en AD FS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 663623cf34fcfe715080945ebcc135587b5ddba1
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: es-ES
---
# <a name="custom-web-themes-in-ad-fs"></a>Temas de Web personalizados en AD FS 

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

El tema que esté enviada out\-descripción\-the\-box se llama de forma predeterminada. Puedes exportar el tema predeterminado y usarlo para que pueda comenzar rápidamente. Puedes personalizar la apariencia y comportamiento, que incluye el diseño modificando el archivo .css, importar y aplicar este nuevo tema y, a continuación, puedes usar el aspecto personalizado y el comportamiento. Uso el archivo .css también hace que sea más fácil trabajar con los diseñadores web.  
  
El siguiente cmdlet crea un tema de web personalizado, lo que duplica el tema de web predeterminado.  
  
  
`New-AdfsWebTheme –Name custom –SourceName default ` 

  
Puedes modificar el archivo .css y configurar el nuevo tema web mediante el nuevo archivo .css. Para exportar un tema de la web, usa el cmdlet siguiente.  
  

    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  

  
Para aplicar el archivo .css en el tema nuevo, usa el cmdlet siguiente.  
  

    Set-AdfsWebTheme –TargetName custom –StyleSheet @{path=”c:\NewTheme.css”}  
  
  
El cmdlet siguiente, crea un tema personalizado web desde una nueva hoja de estilos.  
  
  
`New-AdfsWebTheme –Name custom –StyleSheet @{path=”c:\NewTheme.css”} –RTLStyleSheetPath c:\NewRtlTheme.css ` 
  
  
  
Para aplicar el tema personalizado web a AD FS, usa el cmdlet siguiente.  
  

`Set-AdfsWebConfig -ActiveThemeName custom`  

  
Para agregar JavaScript a AD FS, usa el cmdlet siguiente.  
  
 
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=’ /adfs/portal/script/onload.js’;path="D:\inetpub\adfsassets\script\onload.js"}  


## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión del usuario FS de anuncios](AD-FS-user-sign-in-customization.md)  
