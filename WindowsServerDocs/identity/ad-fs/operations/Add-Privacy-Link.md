---
ms.assetid: 1ca6f87f-7272-4767-b609-3e295ac7d32f
title: Adición del vínculo de la privacidad
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: cd559e38c38e96d1417257fe7d6ff8ccfa180c6b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358419"
---
# <a name="add-privacy-link"></a>Adición del vínculo de la privacidad 


Para agregar el vínculo de privacidad que se muestra en la página Sign @ no__t-0in, use el siguiente cmdlet de Windows PowerShell y la siguiente sintaxis.  

![Agregar vínculo de privacidad](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  
 
`Set-AdfsGlobalWebContent -PrivacyLink https://fs1.contoso.com/privacy/ -PrivacyLinkText Privacy`  
 
  
> [!IMPORTANT]  
> El parámetro `linkText` de este cmdlet solo es obligatorio si usa un valor distinto del predeterminado, que es *Privacy*. La ventaja de usar el valor predeterminado es que las páginas están localizadas para todas las configuraciones regionales de los clientes. Después de personalizar la página Sign @ no__t-0in, la personalización tiene prioridad; por lo tanto, debe personalizar para todos los idiomas que desee admitir. Todo el contenido personalizado toma un parámetro de configuración regional. Al configurar contenido localizado, debe configurarlo con una configuración regional de país @ no__t-0less en primer lugar, por ejemplo, "en", antes de configurar la configuración regional de país y región @ no__t-1specific, como "en @ no__t-2US".  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)  
