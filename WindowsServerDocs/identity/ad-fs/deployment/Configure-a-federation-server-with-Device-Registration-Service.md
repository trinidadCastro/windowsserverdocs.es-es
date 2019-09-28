---
ms.assetid: fdd1c1fd-55aa-4eb8-ae84-53f811de042c
title: Configurar un servidor de federación con el Servicio de registro de dispositivos
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6d4285816993ffd277df471348149b3b54039939
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359771"
---
# <a name="configure-a-federation-server-with-device-registration-service"></a>Configurar un servidor de federación con el Servicio de registro de dispositivos

Puede habilitar el servicio \(de registro de dispositivos DRS\) en el servidor de Federación después de completar [los procedimientos descritos en el paso 4: Configurar un servidor](https://technet.microsoft.com/library/dn303424.aspx)de Federación. El servicio de registro de dispositivos proporciona un mecanismo de incorporación para la autenticación de segundo factor sin problemas\-, \(SSO\)de inicio de sesión único persistente y acceso condicional a los consumidores que requieren acceso a la empresa. recursos. Para obtener más información acerca de DRS, consulte [unirse al área de trabajo desde cualquier dispositivo para SSO y autenticación de segundo factor sin problemas en las aplicaciones de la empresa](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md) .  
  
## <a name="prepare-your-active-directory-forest-to-support-devices"></a>Preparar el bosque de Active Directory para admitir dispositivos  
  
> [!NOTE]  
> Se trata de una\-operación que se debe ejecutar para preparar el bosque de Active Directory para admitir dispositivos. Debe haber iniciado sesión con permisos de administrador de organización y el bosque de Active Directory debe tener el esquema de Windows Server 2012 R2 para completar este procedimiento.  
>   
> Además, DRS requiere que haya al menos un servidor de catálogo global en el dominio raíz del bosque. El servidor de catálogo global es necesario para ejecutar Initialize\-ADDeviceRegistration y durante la autenticación AD FS. AD FS Inicializa una representación en\-memoria del objeto de configuración de DRS en cada solicitud de autenticación y, si el objeto de configuración de DRS no se encuentra en un controlador de dominio del dominio actual, la solicitud se intenta contra el GC en el que se encontraban los objetos de DRS. aprovisionado durante la inicialización\-de ADDeviceRegistration.  
  
#### <a name="to-prepare-the-active-directory-forest"></a>Para preparar el bosque de Active Directory  
  
1.  En el servidor de Federación, abra una ventana de comandos de Windows PowerShell y escriba:  
  
    ```  
    Initialize-ADDeviceRegistration  
    ```  
  
2.  Cuando se le solicite ServiceAccountName, escriba el nombre de la cuenta de servicio que seleccionó como cuenta de servicio para AD FS.  Si se trata de una cuenta de gMSA, escriba la cuenta en el formato **\\dominio AccountName $** . En el caso de una cuenta de dominio, use el formato **dominio\\AccountName**.  
  
## <a name="enable-device-registration-service-on-a-federation-server-farm-node"></a>Habilitar el servicio de registro de dispositivos en un nodo de granja de servidores de Federación  
  
> [!NOTE]  
> Debe haber iniciado sesión con permisos de administrador de dominio para completar este procedimiento.  
  
#### <a name="to-enable-device-registration-service"></a>Para habilitar el servicio de registro de dispositivos  
  
1.  En el servidor de Federación, abra una ventana de comandos de Windows PowerShell y escriba:  
  
    ```  
    Enable-AdfsDeviceRegistration  
    ```  
  
2.  Repita este paso en cada uno de los nodos de la granja de servidores de Federación de la granja de AD FS.  
  
## <a name="enable-seamless-second-factor-authentication"></a>Habilitar la autenticación de segundo factor sin problemas  
La autenticación de segundo factor sin problemas es una mejora en AD FS que proporciona un nivel adicional de protección de acceso a los recursos corporativos y a las aplicaciones de dispositivos externos que intentan acceder a ellos. Cuando un dispositivo personal está unido al área de trabajo, se convierte en un dispositivo "conocido" y los administradores pueden usar esta información para controlar el acceso condicional y la puerta de acceso a los recursos.  
  
#### <a name="to-enable-seamless-second-factor-authentication-persistent-single-sign-on-sso-and-conditional-access-for-workplace-joined-devices"></a>Para habilitar la autenticación de segundo factor sin problemas,\-SSO \(\) de inicio de sesión único persistente y acceso condicional para dispositivos Unidos al área de trabajo  
  
1.  En la consola de administración de AD FS, vaya a directivas de autenticación. Seleccione Editar autenticación principal global. Active la casilla situada junto a habilitar la autenticación de dispositivos y, a continuación, haga clic en Aceptar.  
  
## <a name="update-the-web-application-proxy-configuration"></a>Actualización de la configuración del proxy de aplicación Web  
  
> [!IMPORTANT]  
> No es necesario publicar el servicio de registro de dispositivos en el proxy de aplicación Web.  El servicio de registro de dispositivos estará disponible a través del proxy de aplicación web una vez que esté habilitado en un servidor de Federación.  Es posible que necesite completar este procedimiento para actualizar la configuración del proxy de aplicación web si se ha implementado antes de habilitar el servicio de registro de dispositivos.  
  
#### <a name="to-update-the-web-application-proxy-configuration"></a>Para actualizar la configuración del proxy de aplicación Web  
  
1.  En el servidor proxy de aplicación Web, abra una ventana de comandos de Windows PowerShell y escriba  
  
    ```  
    Update-WebApplicationProxyDeviceRegistration  
    ```  
  
2.  Cuando se le pidan las credenciales, escriba las credenciales de una cuenta que tenga derechos administrativos en los servidores de Federación.  
  
## <a name="see-also"></a>Vea también 

[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guía de implementación de AD FS de Windows Server 2012 R2](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implementar una granja de servidores de federación](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

