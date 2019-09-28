---
title: Tema Web de Azure AD UX en AD FS
description: En el documento siguiente se describe cómo cambiar el inicio de sesión de formularios de AD FS para que se parezca a la experiencia del usuario Azure AD.
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/24/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f4dd1d45646475be3788cd6b615b1743976eedae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358415"
---
# <a name="using-an-azure-ad-ux-web-theme-in-active-directory-federation-services"></a>Usar un tema Web de Azure AD UX en Servicios de federación de Active Directory (AD FS)
En la actualidad, el inicio de sesión de formularios AD FS no refleja la experiencia de inicio de sesión de Azure/O365.  Para proporcionar una experiencia más uniforme y sin problemas a los usuarios finales, hemos publicado el siguiente tema Web de hoja de estilos en cascada que se puede aplicar a los servidores de AD FS.  Actualmente, el inicio de sesión de formularios para AD FS en Windows Server 2016 tiene el siguiente aspecto:

![Inicio de sesión actual](media/Azure-UX-Web-Theme-in-AD-FS/one.png)


Con la nueva hoja de estilos, la experiencia del usuario será más similar a las experiencias de inicio de sesión de Azure y Office 365.

## <a name="download-the-css-style-sheet"></a>Descargar la hoja de estilos CSS
Puede descargar el tema web desde la siguiente [Ubicación](https://github.com/Microsoft/adfsWebCustomization/tree/master/centeredUi)de github.


## <a name="enabling-the-new-web-theme"></a>Habilitar el nuevo tema Web
Para habilitar el nuevo tema Web, use el procedimiento siguiente:

### <a name="to-enable-the-new-azure-ad-ux-web-theme-in-ad-fs"></a>Para habilitar el nuevo tema Web de Azure AD UX en AD FS
1. Iniciar PowerShell como administrador
2. Cree un nuevo tema web con PowerShell:`New-AdfsWebTheme –Name custom –StyleSheet @{path="c:\NewTheme.css"}`
3. Establezca el nuevo tema como el tema activo mediante PowerShell:  `Set-AdfsWebConfig -ActiveThemeName custom`
   ![PowerShell](media/Azure-UX-Web-Theme-in-AD-FS/two.png)
4. Para probar el inicio de sesión, vaya a<AD FS name.domain>https:/// ![ADFS/LS/idpinitiatedsignon.htm inicio de sesión.](media/Azure-UX-Web-Theme-in-AD-FS/three.png)

> ! Tenga en cuenta Debe asegurarse de que se ha habilitado idpinitiatedsignon.  No está habilitado de forma predeterminada.  Para habilitar idpinitiatedsignon, use el siguiente comando de PowerShell:`Set-AdfsProperties –EnableIdpInitiatedSignonPage $True`

## <a name="image-recommendations"></a>Recomendaciones de imagen
La habilitación de la interfaz de usuario centrada le permite usar las mismas imágenes para el fondo y el logotipo que ya tiene para Azure Active Directory la personalización de marca de la empresa. Por lo general, se aplican las mismas recomendaciones para el tamaño, la proporción y el formato.

### <a name="logo"></a>Logo

Descripción | Restricciones | Recomendaciones
------- | ------- | ----------
El logotipo se muestra en la parte superior del panel de inicio de sesión. | JPG o PNG transparente<br>Alto máximo: 36 PX<br>Ancho máximo: 245 PX | Use aquí el logotipo de su organización.<br>Use una imagen transparente. No suponga que el fondo será blanco.<br>No agregue relleno alrededor de su logotipo en la imagen o el logotipo tendrá un aspecto desproporcionadamente pequeño.

### <a name="background"></a>Background

Descripción | Restricciones | Recomendaciones
------- | ------- | ----------
Esta opción aparece en el fondo de la página de inicio de sesión, está anclada en el centro del espacio visible y se escala y recorta para rellenar la ventana del explorador.    <br>En las pantallas estrechas, como los teléfonos móviles, esta imagen no se muestra.<br>Cuando se carga la página, se aplica una máscara negra con una opacidad de 0,55 sobre esta imagen. | JPG o PNG<br>Dimensiones de la imagen: 1920 x 1080 píxeles<br>Tamaño de archivo: &lt;300 KB | <br>Use imágenes en las que no haya un foco de asunto fuerte. El formulario de inicio de sesión opaco aparece sobre el centro de esta imagen y puede cubrir cualquier parte de la imagen, en función del tamaño de la ventana del explorador.<br>Mantenga el tamaño de archivo pequeño para garantizar tiempos de carga rápidos.

## <a name="next-steps"></a>Pasos siguientes
- [Personalización de AD FS en Windows Server 2016](AD-FS-Customization-in-Windows-Server-2016.md)
- [Personalización avanzada](Advanced-Customization-of-AD-FS-Sign-in-Pages.md)
- [Temas web personalizados](Custom-Web-Themes-in-AD-FS.md)
