---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: Agregar\-de signo en la descripción de la página
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8d3cc69bde1c9126f97926802b53d049ed1ef501
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859998"
---
# <a name="add-sign-in-page-description"></a>Agregar\-de signo en la descripción de la página


## <a name="to-add-sign-in-page-description"></a>Para agregar\-de signo en la descripción de la página  
Para agregar un signo\-en la descripción de la página a la página firmar\-de, use el siguiente cmdlet de Windows PowerShell y la siguiente sintaxis.  

![Agregar Descripción de inicio de sesión](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>" 
 
  
> [!IMPORTANT]  
> La cadena del parámetro `SignInPageDescriptionText` admite HTML puro con y sin las etiquetas. Por lo tanto, también puede ejecutar el siguiente cmdlet sin utilizar la etiqueta &lt;p&gt;.  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." ` 

Una vez que se ha personalizado el signo\-en la página, la personalización tiene prioridad; por lo tanto, debe personalizar para todos los idiomas que desee admitir. Todo el contenido personalizado toma un parámetro de configuración regional. Al configurar contenido localizado, debe configurarse con un país\-menos la configuración regional, por ejemplo, "en", antes de configurar el país y la región\-configuración regional específica, como "en\-US".  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)  
