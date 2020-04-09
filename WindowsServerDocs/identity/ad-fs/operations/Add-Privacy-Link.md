---
ms.assetid: 1ca6f87f-7272-4767-b609-3e295ac7d32f
title: Adición del vínculo de la privacidad
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: cf111fbfc14665d489201f7d88419bdeffb737f9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859398"
---
# <a name="add-privacy-link"></a>Adición del vínculo de la privacidad 


Para agregar el vínculo de privacidad que se muestra en la página firmar\-en, use el siguiente cmdlet de Windows PowerShell y la siguiente sintaxis.  

![Agregar vínculo de privacidad](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  
 
`Set-AdfsGlobalWebContent -PrivacyLink https://fs1.contoso.com/privacy/ -PrivacyLinkText Privacy`  
 
  
> [!IMPORTANT]  
> El parámetro `linkText` de este cmdlet solo es obligatorio si usa un valor distinto del predeterminado, que es *Privacy*. La ventaja de usar el valor predeterminado es que las páginas están localizadas para todas las configuraciones regionales de los clientes. Una vez que se ha personalizado el signo\-en la página, la personalización tiene prioridad; por lo tanto, debe personalizar para todos los idiomas que desee admitir. Todo el contenido personalizado toma un parámetro de configuración regional. Al configurar contenido localizado, debe configurarlo con un país\-menos la configuración regional en primer lugar, por ejemplo, "en", antes de configurar el país y la región\-configuración regional específica, como "en\-US".  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)  
