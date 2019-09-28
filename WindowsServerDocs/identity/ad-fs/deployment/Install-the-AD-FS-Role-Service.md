---
ms.assetid: c28a1b8b-5bec-4eed-8c95-a1a29cfc957c
title: Instalar el servicio de rol de AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 46231015ffb91f7c6a1f15615e0eede3fc948be2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408350"
---
# <a name="install-the-ad-fs-role-service"></a>Instalar el servicio de rol de AD FS

Puede usar el siguiente procedimiento para instalar el servicio de rol de AD FS en un equipo que ejecuta Windows Server 2012 R2 para convertirse en el primer servidor de Federación de una granja de servidores de Federación o en un servidor de Federación de una granja de servidores de Federación existente.  
  
La pertenencia al grupo **administradores**, o equivalente, en el equipo local es el requisito mínimo para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-ad-fs-server-role-via-the-add-roles-and-features-wizard"></a>Para instalar el rol de servidor AD FS mediante el Asistente para agregar roles y características  
  
1.  Abra el Administrador del servidor. Para abrir Administrador del servidor, haga clic en **Administrador del servidor** en la pantalla **Inicio** o **Administrador del servidor** en la barra de tareas del escritorio. En la pestaña **Inicio rápido** del icono **Página principal** en la página **Panel**, haga clic en **Agregar roles y características**. Como alternativa, puede hacer clic en **Agregar roles y características** en el menú **Administrar**.  
  
2.  En la página **Antes de comenzar** , haga clic en **Siguiente**.  
  
3.  En la página **Seleccionar tipo de instalación** , haga clic en **rol @ no__t-2Based o en característica @ no__t-3based instalación**y, a continuación, haga clic en **siguiente**.  
  
4.  En la página **Seleccionar servidor de destino**, haz clic en **Seleccionar un servidor del grupo de servidores**, comprueba que está seleccionado el equipo de destino y haz clic en **Siguiente**.  
  
5.  En la página **Seleccionar roles de servidor** , haz clic en **Servicios de federación de Active Directory**y, a continuación, en **Siguiente**.  
  
6.  En la página **Seleccionar características**, haga clic en **Siguiente**. Los requisitos previos necesarios están preseleccionados automáticamente. No tiene que seleccionar otras características.  
  
7.  En la página **Active Directory Servicio de federación \(AD FS @ no__t-2** , haga clic en **siguiente**.  
  
8.  Después de comprobar la información de la página **confirmar selecciones de instalación** , haga clic en **instalar**.  
  
9. En la página **Progreso de la instalación** , comprueba que todo se ha instalado correctamente y haz clic en **Cerrar**.  
  
### <a name="to-install-the-ad-fs-server-role-via-windows-powershell"></a>Para instalar el rol de servidor AD FS a través de Windows PowerShell  
  
1.  En el equipo que deseas configurar como servidor de Federación, abre la ventana de comandos de Windows PowerShell y, después, ejecuta el siguiente `Install-windowsfeature adfs-federation –IncludeManagementTools`comando:.  
  
## <a name="see-also"></a>Vea también 

[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guía de implementación de AD FS de Windows Server 2012 R2](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implementar una granja de servidores de federación](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

