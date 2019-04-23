---
ms.assetid: fdd1c1fd-55aa-4eb8-ae84-53f811de042c
title: Configurar un servidor de federación con el Servicio de registro de dispositivos
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 511a039afd47cf7570fffdcaf17842e0eccc5683
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843066"
---
# <a name="configure-a-federation-server-with-device-registration-service"></a>Configurar un servidor de federación con el Servicio de registro de dispositivos

>Se aplica a: Windows Server 2012 R2

Puede habilitar Device Registration Service \(DRS\) en el servidor de federación después de completar los procedimientos de [paso 4: Configurar un servidor de federación](https://technet.microsoft.com/library/dn303424.aspx). El servicio de registro de dispositivos proporciona un mecanismo de incorporación sin interrupciones de segundo para la autenticación multifactor, inicio de sesión único persistente\-en \(SSO\)y el acceso condicional a los consumidores que requieren acceso a la empresa recursos. Para obtener más información sobre el servicio DRS, consulte [unirse al área de trabajo desde cualquier dispositivo para SSO y sin problemas segundo Factor Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)  
  
## <a name="prepare-your-active-directory-forest-to-support-devices"></a>Preparar el bosque de Active Directory para admitir los dispositivos  
  
> [!NOTE]  
> Esto es\-hora operación que se debe ejecutar para preparar el bosque de Active Directory para admitir los dispositivos. Debe haber iniciado sesión con permisos de administrador de empresa y el bosque de Active Directory debe tener el esquema de Windows Server 2012 R2 para completar este procedimiento.  
>   
> Además, DRS requiere que haya al menos un servidor de catálogo global en el dominio raíz del bosque. El servidor de catálogo global es obligatorio para poder ejecutar Initialize\-ADDeviceRegistration y durante la autenticación de AD FS. AD FS Inicializa un en\-representación de memoria de la configuración de DRS de objetos en cada solicitud de autenticación y si el objeto de configuración de DRS no se encuentra en un controlador de dominio en el dominio actual, se intenta realizar la solicitud en el catálogo global en el que fueron los objetos de DRS aprovisionado durante la inicialización\-ADDeviceRegistration.  
  
#### <a name="to-prepare-the-active-directory-forest"></a>Para preparar el bosque de Active Directory  
  
1.  En el servidor de federación, abra una ventana de comandos de Windows PowerShell y escriba:  
  
    ```  
    Initialize-ADDeviceRegistration  
    ```  
  
2.  Cuando se le pida ServiceAccountName, escriba el nombre de la cuenta de servicio que seleccionó como cuenta de servicio de AD FS.  Si es una cuenta de gMSA, escriba la cuenta en el **dominio\\cuenta$** formato. Para una cuenta de dominio, use el formato **dominio\\accountname**.  
  
## <a name="enable-device-registration-service-on-a-federation-server-farm-node"></a>Habilitar el servicio de registro de dispositivos en un nodo de granja de servidores de federación  
  
> [!NOTE]  
> Debe haber iniciado la sesión con permisos de administrador de dominio para completar este procedimiento.  
  
#### <a name="to-enable-device-registration-service"></a>Para habilitar el servicio registro de dispositivos  
  
1.  En el servidor de federación, abra una ventana de comandos de Windows PowerShell y escriba:  
  
    ```  
    Enable-AdfsDeviceRegistration  
    ```  
  
2.  Repita este paso en cada nodo de granja de servidores de federación en la granja de AD FS...  
  
## <a name="enable-seamless-second-factor-authentication"></a>Habilitar sin problemas la autenticación de segundo factor  
Conexión directa segundo factor de autenticación es una mejora en AD FS que proporciona un nivel adicional de protección del acceso a aplicaciones y recursos corporativos desde dispositivos externos que están intentando tener acceso a ellos. Cuando un dispositivo personal está unido al área de trabajo, se convierte en un dispositivo 'conocido' y los administradores pueden usar esta información para controlar el acceso condicional y obtener acceso a los recursos.  
  
#### <a name="to-enable-seamless-second-factor-authentication-persistent-single-sign-on-sso-and-conditional-access-for-workplace-joined-devices"></a>Para habilitar sin problemas en segundo lugar, la autenticación de factor persistente inicio de sesión único\-en \(SSO\) y el acceso condicional para dispositivos Unidos a un área de trabajo  
  
1.  En la consola de administración de AD FS, vaya a directivas de autenticación. Seleccione Editar autenticación principal Global. Active la casilla situada junto al habilitar la autenticación de dispositivo y, a continuación, haga clic en Aceptar.  
  
## <a name="update-the-web-application-proxy-configuration"></a>Actualizar la configuración de Proxy de aplicación Web  
  
> [!IMPORTANT]  
> No es necesario publicar el servicio de registro de dispositivos en el Proxy de aplicación Web.  El servicio de registro del dispositivo estará disponible a través del Proxy de aplicación Web, una vez que se ha habilitado en un servidor de federación.  Deberá completar este procedimiento para actualizar la configuración de Proxy de aplicación Web si se implementó antes de habilitar el servicio de registro de dispositivos.  
  
#### <a name="to-update-the-web-application-proxy-configuration"></a>Para actualizar la configuración de Proxy de aplicación Web  
  
1.  En el servidor Proxy de aplicación Web, abra una ventana de comandos de Windows PowerShell y escriba  
  
    ```  
    Update-WebApplicationProxyDeviceRegistration  
    ```  
  
2.  Cuando se le solicite las credenciales, escriba las credenciales de una cuenta que tenga derechos administrativos en los servidores de federación.  
  
## <a name="see-also"></a>Vea también 

[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guía de implementación de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implementar una granja de servidores de federación](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

