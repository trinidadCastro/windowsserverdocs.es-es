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
ms.openlocfilehash: b8832e7e53e94761a489e850726bbd206b8be62b
ms.sourcegitcommit: 02f1e11ba37a83e12d8ffa3372e3b64b20d90d00
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68863431"
---
# <a name="ad-fs-customization-in-windows-server-2016"></a>Personalización de AD FS en Windows Server 2016


En respuesta a los comentarios de las organizaciones que usan AD FS, hemos agregado herramientas adicionales para personalizar la experiencia de inicio de sesión de los usuarios para las aplicaciones individuales protegidas por AD FS.  
Además de especificar el contenido web por aplicación, como el texto de la descripción y los vínculos, ahora puede especificar temas Web completos por aplicación.  Esto incluye el logotipo, la ilustración, las hojas de estilos o un archivo onload. js completo.  
  
## <a name="global-settings"></a>Configuración global    
Para la configuración global general, puede hacer referencia a [la personalización de las páginas de inicio de sesión de AD FS](https://technet.microsoft.com/library/dn280950.aspx) incluidas con AD FS en Windows Server 2012 R2.  
  
## <a name="pre-requisites"></a>Requisitos previos  
Los siguientes requisitos previos son necesarios antes de intentar los procedimientos descritos en este documento.  
  
-   AD FS en Windows Server 2016 TP4 o posterior  
  
## <a name="configure-ad-fs-relying-parties"></a>Configuración de AD FS usuarios de confianza  
Los temas y elementos Web de inicio de sesión de cada usuario de confianza se pueden configurar mediante los ejemplos de PowerShell siguientes:  
  
### <a name="customize-messages"></a>Personalización de mensajes  
  
```  
PS C:\>Set-AdfsRelyingPartyWebContent  
    -TargetRelyingPartyName "<RP trust Name>"  
    -CompanyName "This text appears in place of the federation service display name"  
    -OrganizationalNameDescriptionText "This text appears right below the company name"  
    -SignInPageDescription "This text appears below the credential prompt"  
```  
  
### <a name="customize-company-name-logo-and-image"></a>Personalizar el nombre de la empresa, el logotipo y la imagen  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -Logo @{path="C:\Images\applogo.png"}  
    -Illustration @{path="C:\Images\appillustration.jpg"}  
```  
  
### <a name="customize-entire-page"></a>Personalizar toda la página  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -OnLoadScriptPath @{path="c:\scripts\adfstheme\onload.js"}  
```  
  
## <a name="custom-themes-and-advanced-custom-themes"></a>Temas personalizados y temas personalizados avanzados  
  
En el caso de los temas personalizados, consulte Personalización de las [páginas de inicio de sesión de AD FS](https://technet.microsoft.com/library/dn280950.aspx) y personalización [avanzada de las páginas de inicio de sesión de AD FS.](https://technet.microsoft.com/library/dn636121.aspx)  
  
## <a name="assigning-custom-web-themes-per-rp"></a>Asignar temas web personalizados por RP  
  
Para asignar un tema personalizado por RP, use el procedimiento siguiente:  
  
1. Cree un nuevo tema como una copia para el tema global predeterminado en AD FS  
`New-AdfsWebTheme -Name AppSpecificTheme -SourceName default`  
2. Exportar el tema para la personalización  
`Export-AdfsWebTheme -Name AppSpecificTheme -DirectoryPath c:\appspecifictheme`  
3. Personalizar archivos de tema (imágenes, CSS, onload. js): en su editor favorito o reemplazar el archivo  
4. Importar archivos personalizados del sistema de archivos a AD FS (con el objetivo del nuevo tema)  
`Set-AdfsWebTheme -TargetName AppSpecificTheme -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';Path="c:\appspecifictheme\script\onload.js"}`  
5. Aplicar el nuevo tema personalizado al RP específico (o RP)  
`Set-AdfsRelyingPartyWebTheme -TargetRelyingPartyName urn:app1 -SourceWebThemeName AppSpecificTheme`  
  
## <a name="home-realm-discovery"></a>Detección del dominio de inicio  
Para la personalización de la detección del dominio de inicio [, consulte Personalización de las páginas de inicio de sesión AD FS](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="updated-password-page"></a>Página contraseña actualizada  
Para obtener información sobre cómo personalizar la página de actualización de contraseña [, consulte Personalización de las páginas de inicio de sesión AD FS](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="customizing-and-alternate-ids"></a>Personalizar e identificadores alternativos  
Los usuarios pueden iniciar sesión en aplicaciones habilitadas para Servicios de federación de Active Directory (AD FS) (AD FS) con cualquier forma de identificador de usuario aceptada por Active Directory Domain Services (AD DS). Entre ellos se incluyen los nombres principales de usuariojohndoe@contoso.com(UPN) () o los nombres de cuenta SAM (contoso\johndoe o contoso. com\johndoe).  Para obtener más información, consulte [configuración del identificador de inicio de sesión alternativo.](Configuring-Alternate-Login-ID.md)  
  
Además, puede que desee personalizar la página de inicio de sesión de AD FS para proporcionar a los usuarios finales alguna sugerencia sobre el ID. de inicio de sesión alternativo. Para ello, agregue la descripción de la página de inicio de sesión personalizada para obtener más información, consulte [Personalización de las páginas de inicio de sesión de AD FS.](https://technet.microsoft.com/library/dn280950.aspx)   
  
También puede hacerlo personalizando la cadena "iniciar sesión con la cuenta de la organización" sobre el campo de nombre de usuario.  Para obtener más información, consulte [Personalización avanzada de páginas de inicio de sesión de AD FS](https://technet.microsoft.com/library/dn636121.aspx).  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)  
