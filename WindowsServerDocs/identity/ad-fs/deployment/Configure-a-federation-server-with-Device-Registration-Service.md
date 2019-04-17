---
ms.assetid: fdd1c1fd-55aa-4eb8-ae84-53f811de042c
title: "Configurar un servidor de federación con el servicio de registro de dispositivo"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 511a039afd47cf7570fffdcaf17842e0eccc5683
ms.sourcegitcommit: 9278435cbfa8dbeb30d0557ed0d27832b154edd2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2018
---
# <a name="configure-a-federation-server-with-device-registration-service"></a>Configurar un servidor de federación con el servicio de registro de dispositivo

>Se aplica a: Windows Server 2012 R2

Puede habilitar el servicio de registro de dispositivo \(DRS\) en el servidor de federación después de completar los procedimientos descritos en [paso 4: configurar un servidor de federación](https://technet.microsoft.com/library/dn303424.aspx). El servicio de registro de dispositivos proporciona un mecanismo de incorporación para transparente segundo factor de autenticación, persistentes solo sign\ en \(SSO\) y acceso condicional a los usuarios que necesitan acceder a recursos de la compañía. Para obtener más información acerca de DRS, consulta [unir al área de trabajo desde cualquier dispositivo para SSO y transparente segundo Factor de autenticación a través de aplicaciones de la compañía](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)  
  
## <a name="prepare-your-active-directory-forest-to-support-devices"></a>Preparar el bosque de Active Directory para admitir dispositivos  
  
> [!NOTE]  
> Se trata de una operación en tiempo de one\ que deben ejecutarse para preparar el bosque de Active Directory para admitir dispositivos. Debes iniciar sesión con permisos de administrador de empresa y el bosque de Active Directory debe tener el esquema de Windows Server 2012 R2 para completar este procedimiento.  
>   
> Además, DRS requiere tener al menos un servidor de catálogo global en el dominio raíz del bosque. Se requiere el servidor de catálogo global para poder ejecutar archivos\ ADDeviceRegistration y durante la autenticación de AD FS. AD FS Inicializa una representación de memoria in\ del objeto DRS config en cada solicitud de autenticación y si el objeto de DRS configuración no se encuentra en un controlador de dominio en el dominio actual, la solicitud se intenta hacer en el catálogo global en el que los objetos de DRS aprovisionados durante archivos\ ADDeviceRegistration.  
  
#### <a name="to-prepare-the-active-directory-forest"></a>Para preparar el bosque de Active Directory  
  
1.  En el servidor de federación, abre una ventana de comandos de Windows PowerShell y escribe:  
  
    ```  
    Initialize-ADDeviceRegistration  
    ```  
  
2.  Cuando se te pida ServiceAccountName, escribe el nombre de la cuenta de servicio seleccionado como la cuenta de servicio de AD FS.  Si se trata de una cuenta de gMSA, escribe la cuenta en la **domain\\accountname$** formato. Para una cuenta de dominio, use el formato **domain\\accountname**.  
  
## <a name="enable-device-registration-service-on-a-federation-server-farm-node"></a>Habilitar el servicio de registro de dispositivo en un nodo de granja de servidores de federación  
  
> [!NOTE]  
> Debes iniciar sesión con permisos de administrador de dominio para completar este procedimiento.  
  
#### <a name="to-enable-device-registration-service"></a>Para habilitar el servicio de registro de dispositivo  
  
1.  En el servidor de federación, abre una ventana de comandos de Windows PowerShell y escribe:  
  
    ```  
    Enable-AdfsDeviceRegistration  
    ```  
  
2.  Repite este paso en cada nodo de granja de federación de la granja de AD FS.  
  
## <a name="enable-seamless-second-factor-authentication"></a>Habilitar transparente segunda autenticación multifactor  
Transparente segunda autenticación multifactor es una mejora en AD FS que proporciona un nivel adicional de protección de acceso a recursos corporativos y aplicaciones de los dispositivos externos que estás intentando acceder a ellos. Cuando un dispositivo personal está unido a un área de trabajo, se convierte en un dispositivo 'conocido' y los administradores pueden usar esta información para controlar el acceso condicional y da acceso a los recursos.  
  
#### <a name="to-enable-seamless-second-factor-authentication-persistent-single-sign-on-sso-and-conditional-access-for-workplace-joined-devices"></a>Para habilitar transparente segundo factor de autenticación, persistentes solo sign\ en \(SSO\) y acceso condicional para dispositivos Unidos a un área de trabajo  
  
1.  En la consola de administración de AD FS, vaya a directivas de autenticación. Selecciona edita autenticación principal Global. Selecciona la casilla junto a habilitar la autenticación de dispositivo y, a continuación, haz clic en Aceptar.  
  
## <a name="update-the-web-application-proxy-configuration"></a>Actualizar la configuración de Proxy de aplicación Web  
  
> [!IMPORTANT]  
> No es necesario publicar el servicio de registro del dispositivo en el Proxy de aplicación Web.  El servicio de registro del dispositivo estarán disponible a través del Proxy de aplicación Web una vez que se habilita en un servidor de federación.  Debes completar este procedimiento para actualizar la configuración de Proxy de aplicación Web si se implementó antes de habilitar el servicio de registro del dispositivo.  
  
#### <a name="to-update-the-web-application-proxy-configuration"></a>Para actualizar la configuración de Proxy de aplicación Web  
  
1.  En el servidor Proxy de aplicación Web, abrir una ventana de comandos de Windows PowerShell y el tipo  
  
    ```  
    Update-WebApplicationProxyDeviceRegistration  
    ```  
  
2.  Cuando se pidan las credenciales, escribe las credenciales de una cuenta que tenga derechos administrativos en los servidores de federación.  
  
## <a name="see-also"></a>Consulta también 

[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guía de implementación de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implementar un conjunto de servidor de federación](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

