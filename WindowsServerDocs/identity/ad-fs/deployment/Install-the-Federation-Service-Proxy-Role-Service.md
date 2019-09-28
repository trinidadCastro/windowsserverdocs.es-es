---
ms.assetid: c50ecc6a-9504-4b4a-816f-e762dcf3a95e
title: Instalar el servicio de rol Proxy de Servicio de federación
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1f66e863c28aea7c9214c8363328a103b0a92f06
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359571"
---
# <a name="install-the-federation-service-proxy-role-service"></a>Instalar el servicio de rol Proxy de Servicio de federación

Después de configurar un equipo con los certificados y las aplicaciones de requisitos previos, está listo para instalar el servicio de rol Proxy de Servicio de federación de Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1. Puede usar el siguiente procedimiento para instalar el servicio de rol Proxy de Servicio de federación. Al instalar el servicio de rol de Proxy de Servicio de federación en un equipo, ese equipo se convierte en un servidor proxy de Federación.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-federation-service-proxy-role-service-using-the-server-manager"></a>Para instalar el servicio de rol de Proxy de Servicio de federación mediante el Administrador del servidor
  
1.  En la pantalla **Inicio** , escriba**Administrador del servidor**y, a continuación, presione Entrar.  
  
2.  Haga clic en **administrar**y, a continuación, haga clic en **Agregar roles y características** para iniciar el Asistente para agregar roles y características.  
  
3.  En la página **Antes de comenzar** , haga clic en **Siguiente**.  
  
4.  En la página **Seleccionar tipo de instalación** , haga clic en **role @ no__t-2Based o en Feature @ no__t-3based Installation**y haga clic en **Next**.  
  
5.  En la página **Seleccionar servidor de destino** , haga clic en **seleccionar un servidor del grupo de servidores**, compruebe que el equipo de destino esté resaltado y, a continuación, haga clic en **siguiente**.  
  
6.  En la página **Seleccionar roles de servidor** , haz clic en **acceso remoto**y, después, haz clic en siguiente.  
  
    > [!NOTE]  
    > Si se le pide que instale características adicionales de .NET Framework o del servicio de activación de procesos de Windows, haga clic en **Agregar características** para instalarlas.  
  
7. En la página **seleccionar servicios de función** , active la casilla **proxy de servicio de Federación** y, a continuación, haga clic en **siguiente**.  

8. Después de comprobar la información de la página **Confirmar selecciones de instalación** , selecciona la casilla **Reiniciar automáticamente el servidor de destino en caso necesario** y haz clic en **Instalar**.  
  
13. En la página **Progreso de la instalación** , comprueba que todo se ha instalado correctamente y haz clic en **Cerrar**.  

### <a name="to-install-the-federation-service-proxy-role-service-using-powershell"></a>Para instalar el servicio de rol de Proxy de Servicio de federación mediante PowerShell

1. Abrir Windows PowerShell (ejecutar como administrador)

2. Escriba el siguiente comando y presione **entrar**:

        Install-WindowsFeature Web-Application-Proxy -IncludeManagementTools



  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de comprobación: configuración de un servidor proxy de federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

