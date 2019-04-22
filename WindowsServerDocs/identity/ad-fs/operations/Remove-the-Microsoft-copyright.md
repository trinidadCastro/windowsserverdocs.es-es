---
ms.assetid: c89a977c-b09f-44ec-be42-41e76a6cf3ad
title: Quitar el copyright de Microsoft
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: aa27a014b0803712a20fdd23f075486dc35f33c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820986"
---
# <a name="remove-the-microsoft-copyright"></a>Quitar el copyright de Microsoft 

>Se aplica a: Windows Server 2016, Windows Server 2012 R2
 
De forma predeterminada, las páginas de AD FS contienen el copyright de Microsoft. Para quitar el copyright de las páginas que personalices, puedes usar el siguiente procedimiento. 

![Quitar derechos de autor](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png) 
  
## <a name="to-remove-the-microsoft-copyright"></a>Para quitar el copyright de Microsoft  
  
1. Crea un tema personalizado basado en el predeterminado.

   ```powershell
   New-AdfsWebTheme –Name custom –SourceName default
   ```

2. Exporta el tema especificando la carpeta de salida.  

   ```powershell
   Export-AdfsWebTheme -Name custom -DirectoryPath C:\CustomWebTheme
   ```

3. Busque el `Style.css` archivo que se encuentra en la carpeta de salida. Mediante el ejemplo anterior, sería la ruta de acceso `C:\CustomWebTheme\Css\Style.css.`
  
4. Abra el `Style.css` archivo con un editor, como el Bloc de notas.  
  
5. Busque el fragmento de `#copyright` y cámbielo por lo siguiente:  

   ```css
   #copyright {color:#696969; display:none;}
   ```

6. Crear un tema personalizado que se basa en el nuevo `Style.css` archivo.  

   ```powershell
   Set-AdfsWebTheme -TargetName custom -StyleSheet @{locale="";path="C:\customWebTheme\css\style.css"}
   ```

7. Activa el nuevo tema.  

   ```powershell
   Set-AdfsWebConfig -ActiveThemeName custom
   ```

Ahora, ya no debería ver los derechos de autor en la parte inferior de la página de inicio de sesión.

![Quitar derechos de autor](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1a.png) 

## <a name="additional-references"></a>Referencias adicionales 
[AD FS Sign-personalización de usuario](AD-FS-user-sign-in-customization.md) 
