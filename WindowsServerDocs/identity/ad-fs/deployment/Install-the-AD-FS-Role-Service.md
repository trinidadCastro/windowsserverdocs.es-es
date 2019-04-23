---
ms.assetid: c28a1b8b-5bec-4eed-8c95-a1a29cfc957c
title: Instalar el servicio de rol de AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9851134d1ad73092ee44c34c99bc2d873d20ca07
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831176"
---
# <a name="install-the-ad-fs-role-service"></a>Instalar el servicio de rol de AD FS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Puede usar el procedimiento siguiente para instalar el servicio de rol de AD FS en un equipo que ejecuta Windows Server 2012 R2 para convertirse en el primer servidor de federación en una granja de servidores de federación o un servidor de federación en una granja de servidores de federación existente.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es el requisito mínimo para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-ad-fs-server-role-via-the-add-roles-and-features-wizard"></a>Para instalar el rol del servidor de AD FS mediante el Asistente para agregar roles y características  
  
1.  Abra el Administrador del servidor. Para abrir el administrador del servidor, haga clic en **administrador del servidor** en el **iniciar** pantalla, o **administrador del servidor** en la barra de tareas en el escritorio. En la pestaña **Inicio rápido** del icono **Página principal** en la página **Panel**, haga clic en **Agregar roles y características**. Como alternativa, puede hacer clic en **Agregar roles y características** en el menú **Administrar**.  
  
2.  En la página **Antes de comenzar** , haga clic en **Siguiente**.  
  
3.  En el **Seleccionar tipo de instalación** página, haga clic en **rol\-características o en\-instalación basada en**y, a continuación, haga clic en **siguiente**.  
  
4.  En la página **Seleccionar servidor de destino**, haz clic en **Seleccionar un servidor del grupo de servidores**, comprueba que está seleccionado el equipo de destino y haz clic en **Siguiente**.  
  
5.  En la página **Seleccionar roles de servidor** , haz clic en **Servicios de federación de Active Directory**y, a continuación, en **Siguiente**.  
  
6.  En la página **Seleccionar características**, haga clic en **Siguiente**. Los requisitos previos están preseleccionados por usted. No es necesario que seleccionar todas las demás características.  
  
7.  En el **servicio de federación de Active Directory \(AD FS\)**  página, haga clic en **siguiente**.  
  
8.  Después de comprobar la información de la **Confirmar selecciones de instalación** página, haga clic en **instalar**.  
  
9. En la página **Progreso de la instalación** , comprueba que todo se ha instalado correctamente y haz clic en **Cerrar**.  
  
### <a name="to-install-the-ad-fs-server-role-via-windows-powershell"></a>Para instalar el rol de servidor de AD FS a través de Windows PowerShell  
  
1.  En el equipo que desea configurar como un servidor de federación, abra la ventana de comandos de Windows PowerShell y, a continuación, ejecute el siguiente comando: `Install-windowsfeature adfs-federation –IncludeManagementTools`.  
  
## <a name="see-also"></a>Vea también 

[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guía de implementación de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implementar una granja de servidores de federación](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

