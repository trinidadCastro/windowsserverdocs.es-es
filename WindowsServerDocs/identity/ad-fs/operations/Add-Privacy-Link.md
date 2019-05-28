---
ms.assetid: 1ca6f87f-7272-4767-b609-3e295ac7d32f
title: Adición del vínculo de la privacidad
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3617335a179ab419982ab57343999ad4fcaf522a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190162"
---
# <a name="add-privacy-link"></a>Adición del vínculo de la privacidad 


Para agregar el vínculo de privacidad que se muestra en el inicio de sesión\-en la página, use el siguiente cmdlet de Windows PowerShell y la sintaxis.  

![Agregar vínculo de privacidad](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  
 
`Set-AdfsGlobalWebContent -PrivacyLink https://fs1.contoso.com/privacy/ -PrivacyLinkText Privacy`  
 
  
> [!IMPORTANT]  
> El parámetro `linkText` de este cmdlet solo es obligatorio si usa un valor distinto del predeterminado, que es *Privacy*. La ventaja de usar el valor predeterminado es que las páginas están localizadas para todas las configuraciones regionales de los clientes. después del signo\-en la página es personalizado, la personalización tendrá precedencia; por lo tanto, que deberás personalizarla para todos los idiomas que desea admitir. Todo el contenido personalizado toma un parámetro de configuración regional. Al configurar contenido localizado, debe configurar con un país\-menos configuración regional en primer lugar, por ejemplo, "es-es", antes de configurar el país y región\-configuración regional específica, como "en\-nos".  

## <a name="additional-references"></a>Referencias adicionales 
[AD FS Sign-personalización de usuario](AD-FS-user-sign-in-customization.md)  
