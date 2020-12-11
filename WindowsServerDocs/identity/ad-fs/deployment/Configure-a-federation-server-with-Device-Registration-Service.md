---
description: Más información acerca de cómo configurar un servidor de Federación con el servicio de registro de dispositivos
ms.assetid: fdd1c1fd-55aa-4eb8-ae84-53f811de042c
title: Configurar un servidor de federación con el Servicio de registro de dispositivos
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: ae72874eb575579691c00e9331adc0aee18fa1b8
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044233"
---
# <a name="configure-a-federation-server-with-device-registration-service"></a>Configurar un servidor de federación con el Servicio de registro de dispositivos

Puede habilitar el servicio de registro de dispositivos \( DRS \) en el servidor de Federación después de completar los procedimientos descritos en el [paso 4: configurar un servidor de Federación](/previous-versions/orphan-topics/ws.11/dn303424(v=ws.11)). El servicio de registro de dispositivos proporciona un mecanismo de incorporación para la autenticación de segundo factor sin problemas, el inicio de sesión único persistente \- \( y el \) acceso condicional a los consumidores que requieren acceso a los recursos de la empresa. Para obtener más información acerca de DRS, consulte [unirse al área de trabajo desde cualquier dispositivo para SSO y autenticación de segundo factor sin problemas en las aplicaciones de la empresa](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md) .

## <a name="prepare-your-active-directory-forest-to-support-devices"></a>Preparación del bosque de Active Directory para que admita los dispositivos

> [!NOTE]
> Se trata de una \- operación que se debe ejecutar para preparar el bosque de Active Directory para admitir dispositivos. Para completar este procedimiento, debió iniciar sesión con permisos de administrador de organización y el bosque de Active Directory debe tener el esquema de Windows Server 2012 R2.
>
> Además, DRS requiere que haya al menos un servidor de catálogo global en el dominio raíz del bosque. El servidor de catálogo global es necesario para ejecutar Initialize \- ADDeviceRegistration y durante la autenticación AD FS. AD FS Inicializa una representación en \- memoria del objeto de configuración de DRS en cada solicitud de autenticación y, si no se encuentra el objeto de configuración de DRS en un controlador de dominio del dominio actual, se intenta realizar la solicitud en el GC en el que se aprovisionaron los objetos de DRS durante la inicialización de \- ADDeviceRegistration.

#### <a name="to-prepare-the-active-directory-forest"></a>Para preparar el bosque de Active Directory

1.  En el servidor de federación, abra una ventana de comandos de Windows PowerShell y escriba:

    ```
    Initialize-ADDeviceRegistration
    ```

2.  Cuando se le solicite el valor de ServiceAccountName, escriba el nombre de la cuenta de servicio que seleccionó como cuenta de servicio para AD FS.  Si se trata de una cuenta de gMSA, escriba la cuenta en el formato **dominio \\ AccountName $** . En el caso de una cuenta de dominio, use el formato **dominio \\ AccountName**.

## <a name="enable-device-registration-service-on-a-federation-server-farm-node"></a>Habilitar el servicio de registro de dispositivos en un nodo de granja de servidores de Federación

> [!NOTE]
> Debe haber iniciado sesión con permisos de administrador de dominio para completar este procedimiento.

#### <a name="to-enable-device-registration-service"></a>Para habilitar el servicio de registro de dispositivos

1.  En el servidor de federación, abra una ventana de comandos de Windows PowerShell y escriba:

    ```
    Enable-AdfsDeviceRegistration
    ```

2.  Repita este paso en cada uno de los nodos de la granja de servidores de Federación de la granja de AD FS.

## <a name="enable-seamless-second-factor-authentication"></a>Habilitar la autenticación de segundo factor sin problemas
La autenticación de segundo factor sin problemas es una mejora en AD FS que proporciona un nivel adicional de protección de acceso a los recursos corporativos y a las aplicaciones de dispositivos externos que intentan acceder a ellos. Cuando un dispositivo personal está unido al área de trabajo, se convierte en un dispositivo "conocido" y los administradores pueden usar esta información para controlar el acceso condicional y la puerta de acceso a los recursos.

#### <a name="to-enable-seamless-second-factor-authentication-persistent-single-sign-on-sso-and-conditional-access-for-workplace-joined-devices"></a>Para habilitar la autenticación de segundo factor sin problemas, SSO de inicio de sesión único persistente \- \( \) y acceso condicional para dispositivos Unidos al área de trabajo

1.  En la consola de administración de AD FS, vaya a directivas de autenticación. Selecciona Editar autenticación principal global. Selecciona la casilla situada junto a Habilitar la autenticación de dispositivos y haz clic en Aceptar.

## <a name="update-the-web-application-proxy-configuration"></a>Actualización de la configuración del proxy de aplicación Web

> [!IMPORTANT]
> No es necesario publicar el servicio de registro de dispositivos en el proxy de aplicación Web.  El servicio de registro de dispositivos estará disponible a través del proxy de aplicación web una vez que esté habilitado en un servidor de Federación.  Es posible que necesite completar este procedimiento para actualizar la configuración del proxy de aplicación web si se ha implementado antes de habilitar el servicio de registro de dispositivos.

#### <a name="to-update-the-web-application-proxy-configuration"></a>Para actualizar la configuración del proxy de aplicación Web

1.  En el servidor proxy de aplicación Web, abra una ventana de comandos de Windows PowerShell y escriba

    ```
    Update-WebApplicationProxyDeviceRegistration
    ```

2.  Cuando se le pidan las credenciales, escriba las credenciales de una cuenta que tenga derechos administrativos en los servidores de Federación.

## <a name="see-also"></a>Consulte también

[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)

[Guía de implementación de AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)

[Implementar una granja de servidores de federación](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)

