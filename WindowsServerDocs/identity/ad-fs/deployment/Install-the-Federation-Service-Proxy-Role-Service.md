---
ms.assetid: c50ecc6a-9504-4b4a-816f-e762dcf3a95e
title: Instalar el servicio de rol Proxy de Servicio de federación
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6e78c52f1928a3401c0532ab7c25616b012a1d8b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192100"
---
# <a name="install-the-federation-service-proxy-role-service"></a>Instalar el servicio de rol Proxy de Servicio de federación

Después de configurar un equipo con las aplicaciones de requisitos previos y los certificados, está listo para instalar el servicio de rol Proxy de servicio de federación de Active Directory Federation Services \(AD FS\). Puede usar el procedimiento siguiente para instalar el servicio de rol Proxy de servicio de federación. Cuando se instala el servicio de rol Proxy de servicio de federación en un equipo, ese equipo se convierte en un servidor proxy de federación.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-federation-service-proxy-role-service-using-the-server-manager"></a>Para instalar el servicio de rol Proxy de servicio de federación con el administrador del servidor
  
1.  En el **iniciar** , escriba**administrador del servidor**, y, a continuación, presione ENTRAR.  
  
2.  Haga clic en **administrar**y, a continuación, haga clic en **agregar Roles y características** para iniciar el Asistente de las características y agregar Roles.  
  
3.  En la página **Antes de comenzar** , haga clic en **Siguiente**.  
  
4.  En el **Seleccionar tipo de instalación** página, haga clic en **rol\-características o en\-instalación basada en**y haga clic en **siguiente**.  
  
5.  En el **Seleccionar servidor de destino** página, haga clic en **seleccionar un servidor del grupo de servidores**, compruebe que el equipo de destino está resaltado y, a continuación, haga clic en **siguiente**.  
  
6.  En el **seleccionar roles de servidor** página, haga clic en **acceso remoto**y, a continuación, haga clic en siguiente.  
  
    > [!NOTE]  
    > Si se le solicite instalar características adicionales de .NET Framework o Windows Process Activation Service, haga clic en **agregar características** para instalarlos.  
  
7. En el **seleccionar servicios de rol** , seleccione el **Proxy de servicio de federación** casilla de verificación y, a continuación, haga clic en **siguiente**.  

8. Después de comprobar la información de la página **Confirmar selecciones de instalación** , selecciona la casilla **Reiniciar automáticamente el servidor de destino en caso necesario** y haz clic en **Instalar**.  
  
13. En la página **Progreso de la instalación** , comprueba que todo se ha instalado correctamente y haz clic en **Cerrar**.  

### <a name="to-install-the-federation-service-proxy-role-service-using-powershell"></a>Para instalar el servicio de rol Proxy de servicio de federación mediante PowerShell

1. Abra Windows PowerShell (ejecutar como administrador)

2. Escriba el siguiente comando y presione **ENTRAR**:

        Install-WindowsFeature Web-Application-Proxy -IncludeManagementTools



  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de comprobación: configuración de un servidor proxy de federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

