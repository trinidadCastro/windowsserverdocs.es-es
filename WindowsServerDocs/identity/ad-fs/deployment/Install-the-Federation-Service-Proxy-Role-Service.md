---
ms.assetid: c50ecc6a-9504-4b4a-816f-e762dcf3a95e
title: "Instalar el servicio de rol de Proxy de federación servicio"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: aea1ef335604aa18f0b1a1c22ef13f6fee1601b3
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="install-the-federation-service-proxy-role-service"></a>Instalar el servicio de rol de Proxy de federación servicio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Después de configurar un equipo con las aplicaciones de requisitos previos y los certificados, estás listo para instalar el servicio de Proxy de servicios de federación rol de servicios de federación de Active Directory \(AD FS\). Puedes usar el siguiente procedimiento para instalar el servicio de rol de Proxy de federación de servicio. Cuando se instala el servicio de rol de Proxy de federación de servicio en un equipo, dicho equipo se convierte en un proxy de federación de servidor.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-federation-service-proxy-role-service"></a>Para instalar el servicio de rol de Proxy de federación de servicio  
  
1.  En la **inicio**, escriba**administrador del servidor**, y, a continuación, presione ENTRAR.  
  
2.  Haz clic en **administrar**y, a continuación, haz clic en **agregar Roles y características** para iniciar el agregar Roles y características de asistente.  
  
3.  En la **antes de comenzar** página, haz clic en **siguiente**.  
  
4.  En la **selecciona el tipo de instalación** página, haz clic en **instalación basado en Feature\ o Role\**y haz clic en **siguiente**.  
  
5.  En la **servidor de destino selecciona** página, haz clic en **seleccionar un servidor desde el grupo de servidores**, comprueba que el equipo de destino se resalta y, a continuación, haz clic en **siguiente**.  
  
6.  En la **seleccionar roles de servidor** página, haz clic en **los servicios de federación de Active Directory**y, a continuación, haz clic en siguiente.  
  
    > [!NOTE]  
    > Si se le pide instalar características adicionales de .NET Framework o servicio de activación de procesos de Windows, haz clic en **agregar características** para su instalación.  
  
7.  En la **Select features** página, comprueba que se establecen las características y, a continuación, haz clic en **siguiente**.  
  
8.  En la **servicios de federación de Active Directory \(AD FS\)** página, haz clic en **siguiente**.  
  
9. En la **seleccione los servicios de rol**, seleccione la **Proxy de servicios de federación** y, a continuación, haz clic en **siguiente**.  
  
10. En la **rol de servidor Web \(IIS\)** página, haz clic en **siguiente**.  
  
11. En la **seleccione los servicios de rol** página, haz clic en **siguiente**.  
  
12. Después de comprobar la información en la **Confirmar selecciones de instalación** página, seleccione la **reiniciar automáticamente el servidor de destino en caso necesario** y, a continuación, haz clic en **instalar**.  
  
13. En la **progreso de la instalación** página, comprueba que todo lo que ha instalado correctamente y, a continuación, haz clic en **cerrar**.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: Configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de comprobación: Configurar un servidor Proxy Server federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

