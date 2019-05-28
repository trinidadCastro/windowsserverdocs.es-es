---
ms.assetid: 0379abc3-25c7-46ab-9a6b-80a5152365b0
title: Temas Web personalizados en AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2bce52a5704706ad72799d00879e2f4e48f9d703
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189249"
---
# <a name="custom-web-themes-in-ad-fs"></a>Temas Web personalizados en AD FS 

El tema que se envía\-de\-el\-cuadro se denomina de forma predeterminada. Puedes exportar el tema predeterminado y usarlo para empezar rápidamente. Puedes personalizar la apariencia y el comportamiento (incluido el diseño, modificando el archivo .css), importar y aplicar el nuevo tema y, así, usar la apariencia y el comportamiento personalizados. Además, si usas el archivo .css, te será más fácil colaborar con los diseñadores web.  
  
El siguiente cmdlet crea un tema web personalizado duplicando el tema web predeterminado.  
  
  
`New-AdfsWebTheme –Name custom –SourceName default ` 

  
Puedes modificar el archivo .css y configurar el nuevo tema web con el nuevo archivo .css. Para exportar un tema web, usa el siguiente cmdlet.  
  

    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  

  
Para aplicar el archivo .css al nuevo tema, utiliza el siguiente cmdlet.  
  

    Set-AdfsWebTheme –TargetName custom –StyleSheet @{path=”c:\NewTheme.css”}  
  
  
El siguiente cmdlet crea un tema web personalizado a partir de una hoja de estilos nueva.  
  
  
`New-AdfsWebTheme –Name custom –StyleSheet @{path=”c:\NewTheme.css”} –RTLStyleSheetPath c:\NewRtlTheme.css ` 
  
  
  
Para aplicar el tema web personalizado en AD FS, use el siguiente cmdlet.  
  

`Set-AdfsWebConfig -ActiveThemeName custom`  

  
Para agregar JavaScript a AD FS, use el siguiente cmdlet.  
  
 
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=’ /adfs/portal/script/onload.js’;path="D:\inetpub\adfsassets\script\onload.js"}  


## <a name="additional-references"></a>Referencias adicionales 
[AD FS Sign-personalización de usuario](AD-FS-user-sign-in-customization.md)  
