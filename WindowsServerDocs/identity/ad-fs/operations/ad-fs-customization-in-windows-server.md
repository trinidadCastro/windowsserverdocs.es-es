---
ms.assetid: 25f5aff1-6abf-4dea-b531-f1d9943bc181
title: Personalización de AD FS en Windows Server 2016
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e4d463f5f25fe85dc95d767c9c75b722e60b012
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852036"
---
# <a name="ad-fs-customization-in-windows-server-2016"></a>Personalización de AD FS en Windows Server 2016

>Se aplica a: Windows Server 2016

En respuesta a comentarios de las organizaciones que usan AD FS, hemos agregado herramientas adicionales para personalizar el usuario inicie sesión en la experiencia para las aplicaciones protegidas por AD FS.  
Además de especificar el contenido de cada aplicación web como texto de descripción y vínculos, ahora puede especificar los temas de todo el sitio web por aplicación.  Esto incluye el logotipo, ilustración, las hojas de estilos o un archivo de onload.js todo.  
  
## <a name="global-settings"></a>Configuración global    
Para la configuración global general puede hacer referencia a [personalización de AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx) suministrados con AD FS en Windows Server 2012 R2.  
  
## <a name="pre-requisites"></a>Requisitos previos  
Los siguientes requisitos previos necesarios antes de intentar los procedimientos descritos en este documento.  
  
-   AD FS en Windows Server 2016 TP4 o posterior  
  
## <a name="configure-ad-fs-relying-parties"></a>Configurar AD FS de confianza  
Por el usuario de confianza entidad inicio de sesión web elementos y los temas pueden configurarse con los ejemplos de PowerShell siguientes:  
  
### <a name="customize-messages"></a>Personalizar mensajes  
  
```  
PS C:\>Set-AdfsRelyingPartyWebContent  
    -TargetRelyingPartyName "<RP trust Name>"  
    -CompanyName "This text appears in place of the federation service display name"  
    -OrganizationalNameDescriptionText "This text appears right below the company name"  
    -SignInPageDescription "This text appears below the credential prompt"  
```  
  
### <a name="customize-company-name-logo-and-image"></a>Personalizar la imagen del logotipo y nombre de la empresa  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -Logo @{path="C:\Images\applogo.png"}  
    -Illustration @{path="C:\Images\appillustration.jpg"}  
```  
  
### <a name="customize-entire-page"></a>Personalizar la página completa  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -OnLoadScriptPath @{path="c:\scripts\adfstheme\onload.js"}  
```  
  
## <a name="custom-themes-and-advanced-custom-themes"></a>Temas personalizados y avanzados temas personalizados  
  
Para temas personalizados, consulte [personalización de AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx) y [personalización avanzada de AD FS Sign-in Pages.](https://technet.microsoft.com/library/dn636121.aspx)  
  
## <a name="assigning-custom-web-themes-per-rp"></a>Asignación de temas web personalizados por RP  
  
Para asignar un tema personalizado por RP use el procedimiento siguiente:  
  
1. Crear un nuevo tema como una copia del valor predeterminado, un tema global en AD FS  
<code>New-AdfsWebTheme -Name AppSpecificTheme -SourceName default</code> 2.  Exportar el tema para la personalización <code>Export-AdfsWebTheme -Name AppSpecificTheme -DirectoryPath c:\appspecifictheme</code>  
3. Personalizar archivos de tema (imágenes, css, onload.js -) en su editor favorito o reemplazar el archivo de 4. Importar archivos personalizados desde el sistema de archivos a AD FS (como destino el nuevo tema) <code>Set-AdfsWebTheme -TargetName AppSpecificTheme -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';Path="c:\appspecifictheme\script\onload.js"}</code>  
5. Aplicar el tema nuevo y personalizado al RP específicos (o de RP) <code>Set-AdfsRelyingPartyWebTheme -TargetRelyingPartyName urn:app1 -SourceWebThemeName AppSpecificTheme</code>  
  
## <a name="home-realm-discovery"></a>Detección del dominio de inicio  
Para la detección del dominio de inicio Consulte personalización [personalización de AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="updated-password-page"></a>Página de contraseña actualizada  
Para obtener información acerca de cómo personalizar la página de actualización de contraseña, consulte [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="customizing-and-alternate-ids"></a>Id. de la personalización y alternativo  
Los usuarios pueden iniciar sesión en servicios de federación de Active Directory (AD FS): habilita las aplicaciones que usan cualquier forma de identificador de usuario que está aceptado por los servicios de dominio de Active Directory (AD DS). Estos incluyen nombres principales de usuario (UPN) (johndoe@contoso.com) o nombres de cuenta sam (contoso\johndoe o contoso.com\johndoe) calificados con el dominio.  Para obtener más información sobre este, consulte [Id. de configuración de inicio de sesión alternativo.](Configuring-Alternate-Login-ID.md)  
  
Además es posible que desee personalizar la página de inicio de sesión de AD FS para dar a los usuarios finales alguna sugerencia sobre el identificador de inicio de sesión alternativo. Puede hacerlo mediante la adición de la descripción de la página de inicio de sesión personalizada para obtener más información, vea [Customizing the AD FS Sign-in Pages.](https://technet.microsoft.com/library/dn280950.aspx)   
  
También puede hacerlo mediante la personalización de la cadena "Iniciar sesión con cuenta profesional" por encima del campo de nombre de usuario.  Para obtener información sobre este, consulte [personalización avanzada de AD FS Sign-in Pages](https://technet.microsoft.com/library/dn636121.aspx).  

## <a name="additional-references"></a>Referencias adicionales 
[AD FS Sign-personalización de usuario](AD-FS-user-sign-in-customization.md)  
