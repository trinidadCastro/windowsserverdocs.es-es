---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: Adición de la descripción de la página de inicio de sesión
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d16165894b455c4e3ff33b77e84ccce9e30f3be0
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2020
ms.locfileid: "87519834"
---
# <a name="add-sign-in-page-description"></a>Agregar \- Descripción de la página de inicio de sesión

## <a name="to-add-sign-in-page-description"></a>Para agregar la \- Descripción de la página de inicio de sesión
Para agregar una \- Descripción de la página de inicio de sesión a la \- Página de inicio de sesión, use el siguiente cmdlet de Windows PowerShell y la siguiente sintaxis.

![Agregar Descripción de inicio de sesión](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

```powershell
Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"
```

> [!IMPORTANT]
> La cadena del parámetro `SignInPageDescriptionText` admite HTML puro con y sin las etiquetas. Por lo tanto, también puede ejecutar el siguiente cmdlet sin utilizar &lt; la &gt; etiqueta p.  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." `

Después de personalizar la página de inicio \- de sesión, la personalización tiene precedencia, por lo que debe personalizarla para todos los idiomas que desee admitir. Todo el contenido personalizado toma un parámetro de configuración regional. Al configurar contenido localizado, debe configurarse con un país \- menos configuración regional en primer lugar, por ejemplo, "en", antes de configurar la configuración regional específica de país y región, como \- "en \- EE. UU.".

## <a name="additional-references"></a>Referencias adicionales

[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)
