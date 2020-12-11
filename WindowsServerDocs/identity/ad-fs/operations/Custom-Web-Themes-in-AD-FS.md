---
description: 'Más información acerca de: temas web personalizados en AD FS'
ms.assetid: 0379abc3-25c7-46ab-9a6b-80a5152365b0
title: Temas web personalizados en AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 303d1903be5a21d35abe2724401d73170673751c
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97039943"
---
# <a name="custom-web-themes-in-ad-fs"></a>Temas web personalizados en AD FS

El tema que se envía fuera \- del \- \- cuadro se denomina predeterminado. Puedes exportar el tema predeterminado y usarlo para empezar rápidamente. Puedes personalizar la apariencia y el comportamiento (incluido el diseño, modificando el archivo .css), importar y aplicar el nuevo tema y, así, usar la apariencia y el comportamiento personalizados. Además, si usas el archivo .css, te será más fácil colaborar con los diseñadores web.

El siguiente cmdlet crea un tema web personalizado duplicando el tema web predeterminado.

```powershell
New-AdfsWebTheme –Name custom –SourceName default
```

Puedes modificar el archivo .css y configurar el nuevo tema web con el nuevo archivo .css. Para exportar un tema web, usa el siguiente cmdlet.

```powershell
Export-AdfsWebTheme –Name default –DirectoryPath c:\theme
```

Para aplicar el archivo .css al nuevo tema, utiliza el siguiente cmdlet.

```powershell
Set-AdfsWebTheme –TargetName custom –StyleSheet @{path="c:\NewTheme.css"}
```

El siguiente cmdlet crea un tema web personalizado a partir de una hoja de estilos nueva.

```powershell
New-AdfsWebTheme –Name custom –StyleSheet @{path="c:\NewTheme.css"} –RTLStyleSheetPath c:\NewRtlTheme.css
```

Para aplicar el tema web personalizado a AD FS, use el siguiente cmdlet.

```powershell
Set-AdfsWebConfig -ActiveThemeName custom
```

Para agregar JavaScript a AD FS, use el siguiente cmdlet.

```powershell
Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=' /adfs/portal/script/onload.js';path="D:\inetpub\adfsassets\script\onload.js"}
```

## <a name="additional-references"></a>Referencias adicionales

[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)
