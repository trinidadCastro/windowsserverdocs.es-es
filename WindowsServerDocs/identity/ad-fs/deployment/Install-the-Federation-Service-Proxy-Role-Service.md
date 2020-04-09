---
ms.assetid: c50ecc6a-9504-4b4a-816f-e762dcf3a95e
title: Instalar el servicio de rol Proxy de Servicio de federación
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8d2ff177c821baf31bb5453b7c50e3eadca2aab7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855388"
---
# <a name="install-the-federation-service-proxy-role-service"></a>Instalar el servicio de rol Proxy de Servicio de federación

Después de configurar un equipo con los certificados y las aplicaciones de requisitos previos, está listo para instalar el servicio de rol Proxy de Servicio de federación de Servicios de federación de Active Directory (AD FS) \(AD FS\). Puede usar el siguiente procedimiento para instalar el servicio de rol Proxy de Servicio de federación. Al instalar el servicio de rol de Proxy de Servicio de federación en un equipo, ese equipo se convierte en un servidor proxy de Federación.  
  
La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-federation-service-proxy-role-service-using-the-server-manager"></a>Para instalar el servicio de rol de Proxy de Servicio de federación mediante el Administrador del servidor
  
1.  En la pantalla **Inicio** , escriba**Administrador del servidor**y, a continuación, presione Entrar.  
  
2.  Haga clic en **Administrar** y luego haga clic en **Agregar roles y características** para iniciar el Asistente para agregar roles y características.  
  
3.  En la página **Antes de comenzar**, haz clic en **Siguiente**.  
  
4.  En la página **Seleccionar tipo de instalación** , haga clic en **rol\-basado o en instalación basada en\-de características**y, a continuación, haga clic en **siguiente**.  
  
5.  En la página **Seleccionar servidor de destino**, haz clic en **Seleccionar un servidor del grupo de servidores**, comprueba que el equipo de destino esté destacado y luego haz clic en **Siguiente**.  
  
6.  En la página **Seleccionar roles de servidor** , haz clic en **acceso remoto**y, después, haz clic en siguiente.  
  
    > [!NOTE]  
    > Si se le pide que instale características adicionales de .NET Framework o del Servicio de activación de procesos de Windows, haga clic en **Agregar características** para instalarlas.  
  
7. En la página **Seleccionar servicios de rol**, active la casilla **Proxy de Servicio de federación** y luego haga clic en **Siguiente**.  

8. Después de comprobar la información de la página **Confirmar selecciones de instalación**, active la casilla **Reiniciar automáticamente el servidor de destino en caso necesario** y luego haga clic en **Instalar**.  
  
13. En la página **Progreso de la instalación**, comprueba que todo se ha instalado correctamente y haz clic en **Cerrar**.  

### <a name="to-install-the-federation-service-proxy-role-service-using-powershell"></a>Para instalar el servicio de rol de Proxy de Servicio de federación mediante PowerShell

1. Abrir Windows PowerShell (ejecutar como administrador)

2. Escriba el siguiente comando y presione **entrar**:

        Install-WindowsFeature Web-Application-Proxy -IncludeManagementTools



  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configuración de un servidor de Federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de comprobación: configuración de un servidor proxy de Federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

