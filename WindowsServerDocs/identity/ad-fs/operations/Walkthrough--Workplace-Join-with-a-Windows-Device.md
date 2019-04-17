---
ms.assetid: c17d143b-86b4-47c0-b76e-1862dda8f0bd
title: "Tutorial - unirse a un área de trabajo con un dispositivo de Windows"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fd222eb47982591e051594e8a572443b65c0357f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="walkthrough-workplace-join-with-a-windows-device"></a>Tutorial: Unión a un área de trabajo con un dispositivo de Windows

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Este tema muestra cómo usar al área de trabajo para conectar el dispositivo de Windows en tu empresa y cómo acceder a una aplicación web mediante el uso de inicio de sesión único. Debes completar los pasos de la [configurar el entorno de laboratorio de AD FS en Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) sección antes de que puedes probar este tutorial.

## <a name="access-the-web-application-before-device-registration"></a>Acceso a la aplicación web antes de registro del dispositivo
En este tutorial, acceder a una aplicación web de la empresa antes de unir el dispositivo para el área de trabajo. La página Web muestra las notificaciones que se incluyeron en el token de seguridad. Ten en cuenta que la lista de notificaciones no incluye ninguna información acerca del dispositivo. También es posible que observe que no tiene el inicio de sesión único.

#### <a name="to-access-the-web-application-before-you-use-workplace-join-on-your-device"></a>Para obtener acceso a la aplicación web antes de usar al área de trabajo en el dispositivo

1.  Inicie sesión en CLIENTE1 con tu cuenta de Microsoft.

2.  Abre Internet Explorer y navega a la aplicación genérica reclamaciones, **https://webserv1.contoso.com/claimapp**.

3.  Iniciar sesión en la página Web con una cuenta de dominio de la empresa: ** roberth@contoso.com **, contraseña: ** P@ssword **.

4.  La página Web incluye todas las notificaciones en el token de seguridad. Notificaciones de usuario solo están presentes en el token de seguridad.

5.  Cierra Internet Explorer.

6.  Abre Internet Explorer y navegar a la misma aplicación reclamaciones, **https://webserv1.contoso.com/claimapp**.

7.  Ten en cuenta que se pida volver a especificar sus credenciales. No están conectados a la empresa de un dispositivo al área de trabajo y, por tanto, no tienen un inicio de sesión único.

## <a name="join-your-device-with-workplace-join"></a>Une el dispositivo al área de trabajo

> [!IMPORTANT]
> Para que unirte a empresa lleve a cabo correctamente, el equipo cliente (CLIENTE1) debe confiar en el certificado SSL que se usó para configurar los servicios de federación de Active Directory (AD FS) en [paso 2: configurar el servidor de federación con el servicio de registro de dispositivo (ADFS1)](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4). También debe ser capaz de validar la información de revocación del certificado. Si tienes problemas con la al área de trabajo, puedes ver el registro de eventos en CLIENTE1.
> 
> Para ver el registro de eventos, abre el Visor de eventos, expande **registros de aplicaciones y servicios**, expanda **Microsoft**, expanda **Windows**y, a continuación, haz clic en **al área de trabajo**.

#### <a name="to-join-your-device-with-workplace-join"></a>Para unir el dispositivo con unirse a un área de trabajo

1.  Inicie sesión en CLIENTE1 con tu cuenta de Microsoft.

2.  En la **inicio** pantalla, abre el **accesos** barra y, a continuación, selecciona el **configuración** acceso. Selecciona **cambiar configuración de PC**.

3.  En la **configuración de PC** página, seleccione **red**y, a continuación, haz clic en **empresa**.

4.  En la **escribe tu identificador de usuario para obtener acceso a la empresa o activar la administración de dispositivos** , escriba ** roberth@contoso.com **y, a continuación, haz clic en **unirse a**.

5.  Cuando se te pide credenciales, escribe ** roberth@contoso.com **y la contraseña: ** P@ssword **. Haz clic en **Aceptar**.

6.  Ahora deberías ver el mensaje: "este dispositivo se unió a la red de área de trabajo."

### <a name="access-the-web-application-after-joining-the-workplace"></a>Acceso a la aplicación web después de unirse a la empresa
En esta parte de la demostración, tener acceso a una aplicación web de empresa desde el dispositivo que está conectado al área de trabajo. La página Web muestra las notificaciones que se incluyeron en el token de seguridad. Ten en cuenta que la lista de una reclamación incluye información de dispositivo y usuario. También es posible que observe que tienes ahora inicio de sesión único.

##### <a name="to-access-the-web-application-after-joining-the-workplace"></a>Para obtener acceso a la aplicación web después de unirse a la empresa

1.  Iniciar sesión en **CLIENTE1** con tu cuenta de Microsoft.

2.  Abre Internet Explorer y navega a la aplicación genérica reclamaciones, **https://webserv1.contoso.com/claimapp**.

3.  Iniciar sesión en la página Web con una cuenta de dominio de la empresa: ** roberth@contoso.com **, contraseña: ** P@ssword **.

4.  La página Web incluye notificaciones en el token de seguridad. El token contiene reclamaciones de usuarios y dispositivos.

5.  Cierra Internet Explorer.

6.  Abre Internet Explorer y navegar a la misma aplicación reclamaciones, **https://webserv1.contoso.com/claimapp**.

7.  Ten en cuenta que estás **no** pida que escribas las credenciales de nuevo. Está conectado desde un dispositivo con al área de trabajo y, por tanto, tienes el inicio de sesión único.

## <a name="see-also"></a>Consulta también
[Únete a área de trabajo desde cualquier dispositivo para SSO y transparente segundo Factor de autenticación a través de aplicaciones de la compañía](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[configurar el entorno de laboratorio de AD FS en Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
[Tutorial: al área de trabajo con un dispositivo iOS](Walkthrough--Workplace-Join-with-an-iOS-Device.md)



