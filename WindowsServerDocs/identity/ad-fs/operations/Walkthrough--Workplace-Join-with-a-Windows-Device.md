---
ms.assetid: c17d143b-86b4-47c0-b76e-1862dda8f0bd
title: 'Tutorial: Workplace Join con un dispositivo Windows'
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9867e11aa659be9aff9912780e1186a796a7232e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357770"
---
# <a name="walkthrough-workplace-join-with-a-windows-device"></a>Tutorial: unirse al área de trabajo con un dispositivo Windows

En este tema se muestra cómo usar la unión al área de trabajo para conectar tu dispositivo de Windows con tu área de trabajo, y cómo acceder a una aplicación web usando el inicio de sesión único. Debe completar los pasos de la sección [configurar el entorno de laboratorio para AD FS en Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) para poder probar este tutorial.

## <a name="access-the-web-application-before-device-registration"></a>Acceder a la aplicación web antes de registrar el dispositivo
En este tutorial, accederás a una aplicación web antes de unir el dispositivo al área de trabajo. La página web muestra las notificaciones que se incluyeron en tu token de seguridad. Ten en cuenta que la lista de notificaciones no incluye ninguna información sobre tu dispositivo. También verás que no tienes inicio de sesión único.

#### <a name="to-access-the-web-application-before-you-use-workplace-join-on-your-device"></a>Para acceder a la aplicación web antes de usar la unión al área de trabajo en tu dispositivo

1. Inicie sesión en Client1 con su cuenta Microsoft.

2. Abra Internet Explorer y vaya a la aplicación de notificaciones genéricas **https://webserv1.contoso.com/claimapp** .

3. Inicie sesión en la página web con una cuenta de dominio de la compañía: <strong>roberth@contoso.com</strong>, contraseña: <strong>P@ssword</strong>.

4. La página web enumera todas las notificaciones de tu token de seguridad. En el token de seguridad solo se encuentran las notificaciones de usuario.

5. Cierra Internet Explorer.

6. Abre Internet Explorer y ve a la misma aplicación de notificaciones, **https://webserv1.contoso.com/claimapp** .

7. Ten en cuenta que se te pedirá que especifiques de nuevo tus credenciales. No te conectas al área de trabajo desde un dispositivo unido al área de trabajo y, por lo tanto, no tienes inicio de sesión único.

## <a name="join-your-device-with-workplace-join"></a>Unir un dispositivo al área de trabajo

> [!IMPORTANT]
> Para que Workplace Join se realice correctamente, el equipo cliente (Client1) debe confiar en el certificado SSL que se usó para configurar Servicios de federación de Active Directory (AD FS) (AD FS) en [Step 2: Configure el servidor de Federación con el servicio de registro de dispositivos (ADFS1) ](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4). También debe poder validar la información de revocación del certificado. Si tiene problemas con la unión al área de trabajo, puede consultar el registro de eventos de Client1.
> 
> Para ver el registro de eventos, abre el Visor de eventos, expande **Registros de aplicaciones y servicios**, expande **Microsoft**, expande **Windows**y haz clic en **Unión al área de trabajo**.

#### <a name="to-join-your-device-with-workplace-join"></a>Para unir tu dispositivo al área de trabajo

1. Inicie sesión en Client1 con su cuenta Microsoft.

2. En la pantalla **Inicio** , abre la barra **Accesos** y selecciona el acceso a **Configuración** . Selecciona **Cambiar configuración de PC**.

3. En la página **Configuración de PC** , selecciona **Red**y haz clic en **Área de trabajo**.

4. En el cuadro **Escriba su userid para obtener acceso al área de trabajo o activar la administración de dispositivos** , escriba <strong>roberth@contoso.com</strong>y, a continuación, haga clic en **unirse**.

5. Cuando se le pidan las credenciales, escriba <strong>roberth@contoso.com</strong>y password: <strong>P@ssword</strong>. Haga clic en **Aceptar**.

6. Ahora deberías ver el mensaje: "Este dispositivo se unió a la red de tu área de trabajo."

### <a name="access-the-web-application-after-joining-the-workplace"></a>Acceder a la aplicación web después de la unión al área de trabajo
En esta parte de la demostración, accederás a una aplicación web de la compañía desde el dispositivo que está conectado a la unión al área de trabajo. La página web muestra las notificaciones que se incluyeron en tu token de seguridad. Ten en cuenta que la lista de notificaciones incluye la información tanto del dispositivo como del usuario. También verás que ahora tienes inicio de sesión único.

##### <a name="to-access-the-web-application-after-joining-the-workplace"></a>Para acceder a la aplicación web después de la unión al área de trabajo

1. Inicia sesión en **Client1** con tu cuenta Microsoft.

2. Abra Internet Explorer y vaya a la aplicación de notificaciones genéricas **https://webserv1.contoso.com/claimapp** .

3. Inicie sesión en la página web con una cuenta de dominio de la compañía: <strong>roberth@contoso.com</strong>, contraseña: <strong>P@ssword</strong>.

4. La página web enumera las notificaciones de tu token de seguridad. El token contiene notificaciones tanto de usuario como de dispositivo.

5. Cierra Internet Explorer.

6. Abre Internet Explorer y ve a la misma aplicación de notificaciones, **https://webserv1.contoso.com/claimapp** .

7. Ten en cuenta que **no** se te pedirá que especifiques de nuevo tus credenciales. Te conectas desde un dispositivo unido al área de trabajo y, por lo tanto, tiene inicio de sesión único.

## <a name="see-also"></a>Vea también
[Unirse al área de trabajo desde cualquier dispositivo para SSO y autenticación de segundo factor sin problemas en todas las aplicaciones](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)de la empresa 
[Configure el entorno de laboratorio para AD FS en Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
 @ no__t-4Walkthrough: Workplace Join con un dispositivo iOS](Walkthrough--Workplace-Join-with-an-iOS-Device.md)



