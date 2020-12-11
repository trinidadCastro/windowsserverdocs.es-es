---
description: 'Más información acerca de: Tutorial: Workplace Join con un dispositivo iOS'
ms.assetid: 299e4fb9-8f1a-4275-ad7d-dad4f1594657
title: 'Tutorial: Workplace Join con un dispositivo iOS'
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/18/2018
ms.topic: article
ms.openlocfilehash: d77a004dbf9b2010b1bed6c300ad1ebe90c0099c
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97039533"
---
# <a name="walkthrough-workplace-join-with-an-ios-device"></a>Tutorial: unirse al área de trabajo con un dispositivo iOS


> [!IMPORTANT]
> Este método solo es relevante para los clientes locales completos. Los clientes híbridos o solo en la nube no deben usar este método para registrar sus dispositivos iOS. Y este método no es compatible cuando los clientes locales deciden pasar a la nube. Se debe anular el registro del dispositivo y registrarlo en la nube.

En este tema se muestra cómo unirse a un área de trabajo en un dispositivo iOS. Debe completar los pasos de la sección [configurar el entorno de laboratorio para AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) para poder probar este tutorial. Puede usar el dispositivo para tener acceso a la misma aplicación Web de la compañía a la que tuvo acceso en [Walkthrough: Workplace join con un dispositivo Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md).


## <a name="join-an-ios-device-with-workplace-join"></a>Unir un dispositivo iOS al área de trabajo

> [!IMPORTANT]
> Cuando se configura el DRS local, el dispositivo iOS tiene que confiar en el certificado de Capa de sockets seguros (SSL) que se usó para configurar los Servicios de federación de Active Directory (AD FS) en [Step 2: Configure the federation server (ADFS1) with Device Registration Service](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)para que la unión al área de trabajo se realice correctamente.
>
> -   Si el certificado SSL de AD FS lo emitió una entidad de certificación (CA) de prueba, debes instalar el certificado de la entidad de certificación en tu dispositivo iOS.
> -   Si el certificado de la entidad de certificación está publicado en un sitio web, puedes ir al sitio web desde el dispositivo iOS e instalar el certificado.

En esta demostración, unes el dispositivo al área de trabajo.

#### <a name="to-join-an-ios-device-to-a-workplace"></a>Para unir un dispositivo iOS a un área de trabajo

1. -   **Cuando registro de dispositivos de Azure Active Directory servicio es el DRS configurado:** Abra Apple Safari y vaya al punto de conexión de Perfil de servicio por aire de Registro de dispositivos de Azure Active Directory para dispositivos iOS, <`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/<yourdomainname` > donde <`yourdomainname`> es el nombre de dominio que ha configurado con Azure Active Directory. Por ejemplo, si el nombre de dominio es contoso.com, la dirección URL sería: `https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com`

   -   **Cuando DRS local es el DRS configurado**: Abra Apple Safari y navegue hasta el punto de conexión del perfil por aire del servicio de registro de dispositivos (DRS) para dispositivos iOS. `https://adf1s.contoso.com/enrollmentserver/otaprofile`

   Hay muchas formas de comunicar la URL a los usuarios. Una forma que se recomienda consiste en publicar esta dirección URL en un mensaje personalizado de acceso denegado a la aplicación en AD FS. Esto se trata en la próxima sección: [crear una directiva de acceso a la aplicación y un mensaje de acceso denegado personalizado](/azure/active-directory/active-directory-device-registration-on-premises-setup#create-an-application-access-policy-and-custom-access-denied-message) .

2. Inicie sesión en la página web con una cuenta de dominio de la compañía:  <strong>roberth@contoso.com</strong> y la contraseña: <strong>P@ssword</strong> .

3. Se te pedirá que instales un perfil. En la pantalla **Instalar perfil**, haz clic en **Instalar**.

4. Cuando se te pida que confirmes la instalación del perfil, haz clic en **Instalar ahora**.

5. Si tu dispositivo requiere un PIN para desbloquearlo, se te pedirá que lo escribas.

6. La instalación del perfil termina cuando veas la pantalla **Perfil instalado**. Haga clic en **Listo**.

   Vuelve a Safari. Un mensaje te informa de que puedes cerrar Safari o salir de él.

> [!TIP]
> Para ver o quitar el perfil Unión al área de trabajo, ve a **Ajustes**, haz clic en **General** y, después, haz clic en **Perfiles** en tu dispositivo iOS.

## <a name="see-also"></a>Consulte también


- [Unirse a un área de trabajo desde cualquier dispositivo para SSO y autenticación de segundo factor sin problemas en todas las aplicaciones de la compañía](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [Configuración del entorno de laboratorio para AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
- [Tutorial: Workplace Join con un dispositivo Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md)
