---
title: Tema del sitio Web de experiencia de usuario de Azure AD en AD FS
description: El siguiente documento describe cómo cambiar el inicio de sesión de formularios de AD FS para que se parezca a la experiencia de usuario de Azure AD.
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/24/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1064084fe357e54d7230f58e486aa4e62958f6ae
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445011"
---
# <a name="using-an-azure-ad-ux-web-theme-in-active-directory-federation-services"></a>Uso de un tema de sitio Web de la experiencia de usuario de Azure AD en servicios de federación de Active Directory
Inicio de sesión de forma de AD FS en actualmente no refleja la experiencia de inicio de sesión de Azure u Office 365.  Para proporcionar una experiencia más uniforme y sin problemas para los usuarios finales, hemos publicado siga en cascada estilo web tema de la hoja que se puede aplicar a los servidores de AD FS.  Actualmente, lo formularios Inicio de sesión de AD FS en Windows Server 2016 el siguiente aspecto:

![El inicio de sesión actual](media/Azure-UX-Web-Theme-in-AD-FS/one.png)


Con la nueva hoja de estilos, la experiencia del usuario tendrá un aspecto más parecido a las experiencias de inicio de sesión de Azure y Office 365.

## <a name="download-the-css-style-sheet"></a>Descargue la hoja de estilos CSS
Puede descargar el tema web desde Github siguiente [ubicación](https://github.com/Microsoft/adfsWebCustomization/tree/master/centeredUi).


## <a name="enabling-the-new-web-theme"></a>Habilitar el nuevo tema web
Para habilitar el nuevo tema web use el procedimiento siguiente:

### <a name="to-enable-the-new-azure-ad-ux-web-theme-in-ad-fs"></a>Para habilitar el nuevo tema web de experiencia de usuario de Azure AD en AD FS
1. Inicie PowerShell como administrador
2. Cree un nuevo tema web con PowerShell:  `New-AdfsWebTheme –Name custom –StyleSheet @{path="c:\NewTheme.css"}`
3. Establezca el nuevo tema como el tema activo mediante PowerShell:  `Set-AdfsWebConfig -ActiveThemeName custom`
   ![PowerShell](media/Azure-UX-Web-Theme-in-AD-FS/two.png)
4. Probar el inicio de sesión, vaya a https://<AD FS name.domain>/adfs/ls/idpinitiatedsignon.htm ![inicio de sesión](media/Azure-UX-Web-Theme-in-AD-FS/three.png)

> ! [NOTA] Deberá asegurarse de que se ha habilitado ese idpinitiatedsignon.  No está habilitado de forma predeterminada.  Para habilitar idpinitiatedsignon use el siguiente comando de PowerShell:  `Set-AdfsProperties –EnableIdpInitiatedSignonPage $True`

## <a name="image-recommendations"></a>Recomendaciones de imagen
Habilitación de la interfaz de usuario centrado le permite usar las mismas imágenes de fondo y el logotipo que ya tenga para la personalización de marca de empresa de Azure Active Directory. Por lo general, se aplican las mismas recomendaciones de tamaño, la relación y el formato.

### <a name="logo"></a>Logo

Descripción | Restricciones | Recomendaciones
------- | ------- | ----------
El logotipo se muestra en la parte superior del panel de inicio de sesión. | JPG o PNG transparente<br>Alto máximo: 36 px<br>Ancho máximo: 245 px | Use el logotipo de su organización aquí.<br>Usar una imagen transparente. No dé por sentado que el fondo será blanco.<br>No agregue relleno alrededor del logotipo en la imagen o su logotipo parecerá desproporcionadamente pequeño.

### <a name="background"></a>Background

Descripción | Restricciones | Recomendaciones
------- | ------- | ----------
Esta opción aparece en el fondo de la página de inicio de sesión, se ancla en el centro del espacio visible y se escala y recorta para rellenar la ventana del explorador.    <br>En las pantallas estrechas, como teléfonos móviles, no se muestra esta imagen.<br>Se aplica una máscara gruesa con una opacidad de 0,55 sobre esta imagen cuando se carga la página. | JPG o PNG<br>Dimensiones de la imagen: 1920 x 1080 px<br>Tamaño del archivo: &lt; 300 KB. | <br>Usar imágenes donde no hay un enfoque del asunto sólido. El formulario de inicio de sesión opaco aparece sobre el centro de esta imagen y puede cubrir cualquier parte de la imagen, dependiendo del tamaño de la ventana del explorador.<br>Mantenga el tamaño de archivo pequeño para garantizar tiempos de carga rápida.

## <a name="next-steps"></a>Pasos siguientes
- [Personalización de AD FS en Windows Server 2016](AD-FS-Customization-in-Windows-Server-2016.md)
- [Personalización avanzada](Advanced-Customization-of-AD-FS-Sign-in-Pages.md)
- [Temas web personalizados](Custom-Web-Themes-in-AD-FS.md)
