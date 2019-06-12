---
ms.assetid: 299e4fb9-8f1a-4275-ad7d-dad4f1594657
title: 'Tutorial: unión con un dispositivo iOS'
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 979802469737066612bc6242f942fd3acd077479
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444794"
---
# <a name="walkthrough-workplace-join-with-an-ios-device"></a>Tutorial: unirse al área de trabajo con un dispositivo iOS


> [!IMPORTANT] 
> Este método es relevante para sólo totalmente local, los clientes. Los clientes solo en la nube o híbrido no deben utilizar este método para registrar sus dispositivos iOS. Y este método no es compatible cuando los clientes a nivel local deciden cambiar a la nube. El dispositivo debe anular el registro y registrar con la nube. 

En este tema se muestra cómo unirse a un área de trabajo en un dispositivo iOS. Debe completar los pasos descritos en la [configurar el entorno de laboratorio para AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) sección antes de comenzar este tutorial. Puede usar el dispositivo para tener acceso a la misma aplicación web de empresa que ha accedido en [Tutorial: Unión con un dispositivo Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md).


## <a name="join-an-ios-device-with-workplace-join"></a>Unir un dispositivo iOS al área de trabajo

> [!IMPORTANT]
> Cuando se configura el DRS local, el dispositivo iOS debe confiar en el certificado de capa de sockets seguros (SSL) que se usó para configurar los servicios de federación de Active Directory (AD FS) en [paso 2: Configurar el servidor de federación (ADFS1) with Device Registration Service](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4), para unirse al área de trabajo se ejecute correctamente.
> 
> -   Si el certificado SSL de AD FS lo emitió una entidad de certificación (CA) de prueba, debes instalar el certificado de la entidad de certificación en tu dispositivo iOS.
> -   Si el certificado de la entidad de certificación está publicado en un sitio web, puedes ir al sitio web desde el dispositivo iOS e instalar el certificado.

En esta demostración, unes el dispositivo al área de trabajo.

#### <a name="to-join-an-ios-device-to-a-workplace"></a>Para unir un dispositivo iOS a un área de trabajo

1. -   **Cuando el servicio de registro del dispositivo de Azure Active Directory es el DRS configurado:** Abre Apple Safari y navega al extremo de perfil móvil de servicio de registro de dispositivos de Azure Active Directory para dispositivos iOS, <`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/<yourdomainname` > donde <`yourdomainname`> es el nombre de dominio que ha configurado con Azure Active Directory. Por ejemplo, si el nombre de dominio es contoso.com, la dirección URL sería: `https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com`

   -   **Cuando el DRS local es el DRS configurado**: Abre Apple Safari y navega hasta el extremo del perfil de servicio de registro de dispositivos (DRS) por el aire para dispositivos iOS, `https://adf1s.contoso.com/enrollmentserver/otaprofile`

   Hay muchas formas de comunicar la URL a los usuarios. Una forma recomendada es publicar esta dirección URL en un mensaje personalizado de acceso denegado de una aplicación en AD FS. Este tema se trata en la siguiente sección: [Crear una directiva de acceso de aplicaciones y el mensaje de denegación de acceso personalizada](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup#create-an-application-access-policy-and-custom-access-denied-message)

2. Inicie sesión en la página Web con una cuenta de dominio de empresa: <strong>roberth@contoso.com</strong> y la contraseña: <strong>P@ssword</strong>.

3. Se te pedirá que instales un perfil. En la pantalla **Instalar perfil** , haz clic en **Instalar**.

4. Cuando se te pida que confirmes la instalación del perfil, haz clic en **Instalar ahora**.

5. Si tu dispositivo requiere un PIN para desbloquearlo, se te pedirá que lo escribas.

6. La instalación del perfil termina cuando veas la pantalla **Perfil instalado** . Haga clic en **Listo**.

   Vuelve a Safari. Un mensaje te informa de que puedes cerrar Safari o salir de él.

> [!TIP]
> Para ver o quitar el perfil Unión al área de trabajo, ve a **Ajustes**, haz clic en **General** y, después, haz clic en **Perfiles** en tu dispositivo iOS.

## <a name="see-also"></a>Vea también


- [Unirse a un área de trabajo desde cualquier dispositivo para SSO y sin interrupciones de segundo Factor de autenticación a través de las aplicaciones de empresa](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [Configuración del entorno de laboratorio para AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
- [Tutorial: Workplace Join con un dispositivo Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md)



