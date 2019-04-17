---
ms.assetid: 1ca6f87f-7272-4767-b609-3e295ac7d32f
title: "Agregar el vínculo de privacidad"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 81a453b45693b8222bdfc0231885b506fdfcd2fc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="add-privacy-link"></a>Agregar el vínculo de privacidad 

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Para agregar el vínculo de privacidad que se muestra en la página de sign\, usa el siguiente cmdlet de PowerShell de Windows y la sintaxis.  

![Agregar el vínculo de privacidad](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  
 
`Set-AdfsGlobalWebContent -PrivacyLink https://fs1.contoso.com/privacy/ -PrivacyLinkText Privacy`  
 
  
> [!IMPORTANT]  
> La `linkText`parámetro en este cmdlet no es necesario a menos que use otro valor que el valor predeterminado, que es *privacidad*. La ventaja de usar el valor predeterminado es que las páginas están localizadas a todas las configuraciones regionales de cliente. Una vez la página de sign\ personalizada, la personalización tiene prioridad; por lo tanto, deberías personalizar para todos los idiomas que quieras admitir. Todo el contenido personalizado toma un parámetro de configuración regional. Al configurar el contenido localizado, debes configurar, con una configuración regional de country\ menos en primer lugar, por ejemplo, "en", antes de configurar el país y la configuración regional de region\ específicos, como "en\-us".  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión del usuario FS de anuncios](AD-FS-user-sign-in-customization.md)  
