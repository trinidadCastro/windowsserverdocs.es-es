---
description: 'Más información sobre: personalización para la localización'
ms.assetid: 38bbc002-a8fa-4211-9328-4ef67fca0acf
title: Personalización para la localización
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 8f39a5da85821d6c4cdeb6bff7803dc57e06623e
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97039883"
---
# <a name="customization-for-localization"></a>Personalización para la localización

El contenido web se puede localizar a idiomas distintos del inglés. Al localizarlo, ten en cuenta las siguientes consideraciones.

Después de personalizar el contenido, la personalización tendrá precedencia, así que deberás personalizarla para todos los idiomas que quieras admitir. Todo el contenido personalizado toma un parámetro de configuración regional. Al configurar contenido localizado, configúrelo primero con un país \- menos local, por ejemplo, "en", antes de configurar una configuración regional específica de país y región, como \- "en \- EE. UU.".

A continuación se muestran varios ejemplos de código adicionales.

```powershell
Set-AdfsWebTheme -TargetName default -Logo @{Locale="";Path="c:\contoso.png"}
Set-AdfsWebTheme -TargetName default -Illustration @{Locale="";Path="c:\illustration.png"}
```

A continuación se muestran varios ejemplos de código adicionales.

```powershell
Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" –locale "en"

Set-AdfsGlobalWebContent -ErrorPageDescriptionText "Il s'agit de description de page erreur de Contoso" –locale "fr"
```

Si desea personalizar el contenido web en idiomas distintos del inglés que requieran la entrada de Unicode, se recomienda usar el Windows PowerShell ISE. Para obtener más información, consulte [Introducción a la Windows PowerShell ISE](/previous-versions/mt707506(v=msdn.10)).

## <a name="additional-references"></a>Referencias adicionales

[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)
