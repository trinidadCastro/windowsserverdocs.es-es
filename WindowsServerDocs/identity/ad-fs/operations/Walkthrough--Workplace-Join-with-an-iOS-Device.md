---
ms.assetid: 299e4fb9-8f1a-4275-ad7d-dad4f1594657
title: Tutorial - unirse a un área de trabajo con un dispositivo iOS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0a8643bab913dfec07c6bbea0c068e1240f16c5b
ms.sourcegitcommit: a2260c96b0e49519d180c7382b921ce8ddb053fe
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough-workplace-join-with-an-ios-device"></a>Tutorial: Unirse un área de trabajo con un dispositivo iOS

>Se aplica a: Windows Server 2012 R2

Este tema muestra al área de trabajo en un dispositivo iOS. Debes completar los pasos de la [configurar el entorno de laboratorio de AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) sección antes de que puedes probar este tutorial. Puedes usar el dispositivo para acceder a la misma aplicación web de empresa que acceder en [Tutorial: al área de trabajo con un dispositivo Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md).

## <a name="join-an-ios-device-with-workplace-join"></a>Unirse a un dispositivo iOS al área de trabajo

> [!IMPORTANT]
> Cuando se configura DRS local, el dispositivo iOS debe confiar en el certificado de capa de sockets seguros (SSL) que se usó para configurar los servicios de federación de Active Directory (AD FS) en [paso 2: configurar el servidor de federación (ADFS1) con el servicio de registro de dispositivo](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4), en lugar de trabajo combinación correctamente.
> 
> -   Si se ha emitido el certificado SSL de AD FS de una entidad de certificación (CA) de prueba, debes instalar el certificado de entidad de certificación en el dispositivo iOS.
> -   Si el certificado de entidad de certificación se publica en un sitio Web, puede instalar el certificado y busca el sitio Web de tu dispositivo iOS.

En esta demostración, unir el dispositivo para el área de trabajo.

#### <a name="to-join-an-ios-device-to-a-workplace"></a>Para unir un dispositivo iOS a un área de trabajo

1.  -   **Cuando el servicio de registro del dispositivo de Azure Active Directory es la DRS configurado:** abrir Apple Safari y navega al extremo de perfil de conexión de servicio de registro del dispositivo de Azure Active Directory para dispositivos iOS, <`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/<yourdomainname` > donde <`yourdomainname`> es el nombre de dominio que hayas configurado con Azure Active Directory. Por ejemplo, si el nombre de dominio es contoso.com, sería la dirección URL: `https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com`

    -   **Cuando DRS local es el DRS configurado**: abre Apple Safari y navega hacia el extremo de perfil de conexión de servicio de registro de dispositivo (DRS) para dispositivos iOS,`https://adf1s.contoso.com/enrollmentserver/otaprofile`

    Existen muchas formas de comunicarse esta dirección URL a los usuarios. Una forma recomendada es publicar esta dirección URL en un acceso a la aplicación personalizada deniega el mensaje en AD FS. Esto se explica en la sección próxima: [crear una directiva de acceso de la aplicación y personalizado mensaje de acceso denegado](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup#create-an-application-access-policy-and-custom-access-denied-message)

2.  Iniciar sesión en la página Web con una cuenta de dominio de la empresa: ** roberth@contoso.com ** y la contraseña: ** P@ssword **.

3.  Se te pida instalar un perfil. En la **instalar perfil** de pantalla, haz clic en **instalar**.

4.  Cuando aparezca el mensaje para confirmar la instalación del perfil, haz clic en **instalar ahora**.

5.  Si el dispositivo requiere un PIN para desbloquear el dispositivo, le introducir tu PIN.

6.  Finalice la instalación del perfil cuando veas la **perfil instalado** pantalla. Haz clic en **listo**.

    Vuelve a Safari. Un mensaje te informará que puede cerrar o salir de Safari.

> [!TIP]
> Para ver o eliminar el perfil al área de trabajo, vaya a **configuración**, haz clic en **General**y, a continuación, haz clic en **perfiles** en tu dispositivo iOS.

## <a name="see-also"></a>Consulta también


- [Unir al área de trabajo desde cualquier dispositivo para SSO y transparente segundo Factor de autenticación entre las aplicaciones de empresa](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [Configurar el entorno de laboratorio de AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
- [Tutorial: Unión a un área de trabajo con un dispositivo de Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md)



