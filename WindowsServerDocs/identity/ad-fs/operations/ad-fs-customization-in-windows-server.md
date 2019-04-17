---
ms.assetid: 25f5aff1-6abf-4dea-b531-f1d9943bc181
title: "AD FS personalización en Windows Server 2016"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e4d463f5f25fe85dc95d767c9c75b722e60b012
ms.sourcegitcommit: a2699e93a0a19cb138c1fde0c9af36774a12f865
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2017
---
# <a name="ad-fs-customization-in-windows-server-2016"></a>AD FS personalización en Windows Server 2016

>Se aplica a: Windows Server 2016

En respuesta a los comentarios de las organizaciones que usan AD FS, hemos agregado herramientas adicionales para personalizar el usuario inicia sesión en la experiencia para aplicaciones individuales protegido por AD FS.  
Además de especificar contenido web de cada aplicación como vínculos y texto de descripción, ahora puedes especificar temas de todo el sitio web por aplicación.  Esto incluye el logotipo, ilustración, hojas de estilo o un archivo completo onload.js.  
  
## <a name="global-settings"></a>Configuración global    
Para la configuración global general puede hacer referencia a [personalización de las páginas de signo en AD FS](https://technet.microsoft.com/library/dn280950.aspx) que acompaña a AD FS en Windows Server 2012 R2.  
  
## <a name="pre-requisites"></a>Requisitos previos  
Se requieren los siguientes requisitos previos antes de seguir los procedimientos descritos en este documento.  
  
-   AD FS de Windows Server 2016 TP4 o versiones posteriores  
  
## <a name="configure-ad-fs-relying-parties"></a>Configurar AD FS confiar partes  
Por el usuario de confianza web de inicio de sesión fabricantes elementos y temas se puede configurar mediante los siguientes ejemplos de PowerShell:  
  
### <a name="customize-messages"></a>Personalizar los mensajes  
  
```  
PS C:\>Set-AdfsRelyingPartyWebContent  
    -TargetRelyingPartyName "<RP trust Name>"  
    -CompanyName "This text appears in place of the federation service display name"  
    -OrganizationalNameDescriptionText "This text appears right below the company name"  
    -SignInPageDescription "This text appears below the credential prompt"  
```  
  
### <a name="customize-company-name-logo-and-image"></a>Personalizar la imagen de logotipo y nombre de la compañía  
  
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
  
Para temas personalizados, consulte [personalización de las páginas de signo en AD FS](https://technet.microsoft.com/library/dn280950.aspx) y [personalización avanzada de signo en AD FS páginas.](https://technet.microsoft.com/library/dn636121.aspx)  
  
## <a name="assigning-custom-web-themes-per-rp"></a>Asignar temas personalizados web cada punto de reunión  
  
Para asignar un tema personalizado cada punto de reunión usa el siguiente procedimiento:  
  
1. Crear un nuevo tema como una copia para el valor predeterminado, un tema global de AD FS  
<code>New-AdfsWebTheme -Name AppSpecificTheme -SourceName default</code>  
2.  Exportar el tema de personalización <code>Export-AdfsWebTheme -Name AppSpecificTheme -DirectoryPath c:\appspecifictheme</code>3.personalizar los archivos de tema (imágenes, css, onload.js) en tu editor favorito o reemplazar el archivo de 4. Importar archivos personalizados desde el sistema de archivos a AD FS (el nuevo tema de la selección de destino) <code>Set-AdfsWebTheme -TargetName AppSpecificTheme -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';Path="c:\appspecifictheme\script\onload.js"}</code>5.el nuevo tema personalizado se aplican al punto de reunión específicos (o del punto de reunión)
<code>Set-AdfsRelyingPartyWebTheme -TargetRelyingPartyName urn:app1 -SourceWebThemeName AppSpecificTheme</code>  
  
## <a name="home-realm-discovery"></a>Detección de dominio de inicio  
Para el dominio de inicio dicovery consulta personalización [personalización de las páginas de signo en AD FS](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="updated-password-page"></a>Página de contraseña actualizada  
Para obtener información sobre cómo personalizar la página de la contraseña de actualización vea [personalizar las páginas AD FS Sign-in](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="customizing-and-alternate-ids"></a>Identificadores de personalización y alternativos  
Los usuarios pueden iniciar sesión en los servicios de federación de Active Directory (AD FS)-habilitado el uso de cualquier forma de identificador de usuario que acepte los servicios de dominio de Active Directory (AD DS) de aplicaciones. Estos incluyen nombres Principal de usuario (UPN) (johndoe@contoso.com) o de dominio completo nombres de cuenta sam (contoso\johndoe o contoso.com\johndoe #).  Para obtener más información sobre esta consulta [identificador de configuración de inicio de sesión alternativo.](Configuring-Alternate-Login-ID.md)  
  
Además puedes personalizar la página de inicio de sesión de AD FS para dar a los usuarios finales algunos sugerencia sobre el identificador de inicio de sesión alternativo. Puedes hacerlo agregando la descripción de la página de inicio de sesión personalizada para obtener más información, consulta [personalizar las páginas AD FS Sign-in.](https://technet.microsoft.com/library/dn280950.aspx)   
  
También puedes hacer esto mediante la personalización de la cadena "que inicia sesión con cuenta corporativa" por encima del campo de nombre de usuario.  Para obtener información sobre esto, consulta [personalización avanzada de AD FS Sign-in páginas](https://technet.microsoft.com/library/dn636121.aspx).  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión del usuario FS de anuncios](AD-FS-user-sign-in-customization.md)  
