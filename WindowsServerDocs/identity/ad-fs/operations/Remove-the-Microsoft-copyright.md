---
ms.assetid: c89a977c-b09f-44ec-be42-41e76a6cf3ad
title: Quitar los derechos de autor de Microsoft
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c2e6f9445e53a5b5867a763d58ad4a6ca3600cbe
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="remove-the-microsoft-copyright"></a>Quitar los derechos de autor de Microsoft 

>Se aplica a: Windows Server 2016, Windows Server 2012 R2
 
De manera predeterminada, las páginas de AD FS contienen los derechos de autor de Microsoft. Para quitar esta derechos de autor de las páginas personalizadas, puedes usar el siguiente procedimiento. 

![Quitar derechos de autor](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png) 
  
## <a name="to-remove-the-microsoft-copyright"></a>Para quitar los derechos de autor de Microsoft  
  
1.  Crear un tema personalizado que se basa en el valor predeterminado.  
  

    `New-AdfsWebTheme –Name custom –SourceName default ` 
 
  
2.  Exportar el tema mediante la especificación de la carpeta de resultados.  

    `Export-AdfsWebTheme -Name custom -DirectoryPath C:\customWebTheme ` 

  
3.  Busque el archivo Style.css que se encuentra en la carpeta de resultados. Al usar el ejemplo anterior, la ruta de acceso sería C:\\CustomWebTheme\\Css\\Style.css.  
  
4.  Abre el archivo Style.css con un editor, como el Bloc de notas.  
  
5.  Busque la `#copyright` parte y, a continuación, cambia al siguiente:  
  `#copyright {color:#696969; display:none;} ` 
 
6.  Crear un tema personalizado que se basa en el nuevo archivo Style.css.  
  
    `Set-AdfsWebTheme -TargetName custom -StyleSheet @{locale="";path="C:\customWebTheme\css\style.css"}  `

7.  Activar el nuevo tema.  
  

    `Set-AdfsWebConfig -ActiveThemeName custom ` 


Ahora, ya no deberías ver los derechos de autor en la parte inferior de la página de inicio de sesión.

![Quitar derechos de autor](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1a.png) 

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión del usuario FS de anuncios](AD-FS-user-sign-in-customization.md) 
