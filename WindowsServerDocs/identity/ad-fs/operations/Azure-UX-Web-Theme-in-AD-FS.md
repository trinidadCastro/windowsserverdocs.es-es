---
title: Tema de Web de Azure AD experiencia del usuario en AD FS
description: "El siguiente documento describe cómo cambiar el inicio de sesión en formularios de AD FS de modo que es similar a la experiencia del usuario de Azure AD."
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/24/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e7bac1db17eb4facc7643fe0db0ccf00c119a45d
ms.sourcegitcommit: 9278435cbfa8dbeb30d0557ed0d27832b154edd2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2018
---
# <a name="using-an-azure-ad-ux-web-theme-in-active-directory-federation-services"></a>Usa un tema de Web de la experiencia de usuario de Azure AD en servicios de federación de Active Directory
Inicia sesión los formularios de AD FS actualmente no refleje la experiencia de inicio de sesión de Azure o O365.  Para proporcionar una experiencia más uniforme y transparente para los usuarios finales, hemos publicado sigue en cascada estilo web tema de la hoja que se puede aplicar a los servidores de AD FS.  Actualmente, el formularios Inicio de sesión de AD FS en Windows Server 2016 aspecto siguiente:

![El inicio de sesión actual](media/Azure-UX-Web-Theme-in-AD-FS/one.png)


Con la nueva hoja de estilo, la experiencia del usuario tendrá un aspecto más como las experiencias de inicio de sesión en Azure y Office 365.

## <a name="download-the-css-style-sheet"></a>Descargar de la hoja de estilos CSS
Puedes descargar el tema de web de la siguiente Github [ubicación ](https://github.com/Microsoft/adfsWebCustomization/tree/master/centeredUi).


## <a name="enabling-the-new-web-theme"></a>Habilitar el nuevo tema web
Para habilitar el nuevo tema web usa el siguiente procedimiento:

### <a name="to-enable-the-new-azure-ad-ux-web-theme-in-ad-fs"></a>Para habilitar el nuevo tema de web de la experiencia de usuario de Azure AD en AD FS
1.  Inicia PowerShell como administrador
2.  Crear un nuevo tema web mediante PowerShell:  `New-AdfsWebTheme –Name custom –StyleSheet @{path="c:\NewTheme.css"}`
3.  Establecer el nuevo tema como del tema activo mediante PowerShell: <ph x="1">'Conjunto AdfsWebConfig - ActiveThemeName custom'
! [</ph>PowerShell](media/Azure-UX-Web-Theme-in-AD-FS/two.png)
4.  Prueba el inicio de sesión de yendo a https://<AD FS name.domain>/adfs/ls/idpinitiatedsignon.htm ![inicio de sesión](media/Azure-UX-Web-Theme-in-AD-FS/three.png)

>! [NOTA] Debes asegurarte de que idpinitiatedsignon se ha habilitado.  No está habilitado de manera predeterminada.  Para permitir el uso de idpinitiatedsignon el siguiente comando de PowerShell:  `Set-AdfsProperties –EnableIdpInitiatedSignonPage $True`

## <a name="image-recommendations"></a>Recomendaciones de imagen
Las siguientes son las recomendaciones de tamaño de la imagen de fondo y la imagen de logotipo:

### <a name="logo"></a>Logotipo
- Alto de 24 px de tamaño, 256px máximo ancho
- No agregues relleno alrededor del logotipo dentro del activo.  Asegúrate de que el fondo del elemento es transparente.

### <a name="background"></a>En segundo plano
- El tamaño de 1024 x 1080 píxeles con un tamaño de archivo no superior a 200KB.  Debes usar la mayor resolución posible con la relación de aspecto 16:9 o 16:10 que mantiene el tamaño de imagen en la restricción.

## <a name="next-steps"></a>Pasos siguientes
- [AD FS personalización en Windows Server 2016](AD-FS-Customization-in-Windows-Server-2016.md)
- [Personalización avanzada](Advanced-Customization-of-AD-FS-Sign-in-Pages.md)
- [Temas de web personalizados](Custom-Web-Themes-in-AD-FS.md)
