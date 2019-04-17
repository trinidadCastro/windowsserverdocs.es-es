---
ms.assetid: c28a1b8b-5bec-4eed-8c95-a1a29cfc957c
title: Instalar el servicio de AD FS rol
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9851134d1ad73092ee44c34c99bc2d873d20ca07
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="install-the-ad-fs-role-service"></a>Instalar el servicio de AD FS rol

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Puedes usar el siguiente procedimiento para instalar el servicio de rol de AD FS en un equipo que ejecute Windows Server 2012 R2a convertirse en el primer servidor de federación de una granja de servidores de federación o un servidor de federación en un conjunto de servidor de federación existente.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es el requisito mínimo para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-ad-fs-server-role-via-the-add-roles-and-features-wizard"></a>Para instalar el rol de servidor de AD FS mediante el Asistente de agregar roles y características  
  
1.  Abre el administrador del servidor. Para abrir el administrador del servidor, haz clic en **administrador del servidor** en la **inicio** pantalla, o **administrador del servidor** en la barra de tareas en el escritorio. En la **inicio rápido** ficha de la **bienvenida** icono en la **panel** página, haz clic en **agregar roles y características**. Como alternativa, puedes hacer clic en **agregar Roles y características** en la **administrar** menú.  
  
2.  En la **antes de comenzar** página, haz clic en **siguiente**.  
  
3.  En la **selecciona el tipo de instalación** página, haz clic en **instalación basado en Feature\ o Role\**y, a continuación, haz clic en **siguiente**.  
  
4.  En la **servidor de destino selecciona** página, haz clic en **seleccionar un servidor desde el grupo de servidores**, comprueba que el equipo de destino está seleccionado y, a continuación, haz clic en **siguiente**.  
  
5.  En la **seleccionar roles de servidor** página, haz clic en **los servicios de federación de Active Directory**y, a continuación, haz clic en **siguiente**.  
  
6.  En la **Select features** página, haz clic en **siguiente**. Los requisitos previos necesarios están preseleccionados para TI. No tienes que seleccionar todas las demás características.  
  
7.  En la **servicios de federación de Active Directory \(AD FS\)** página, haz clic en **siguiente**.  
  
8.  Después de comprobar la información en la **Confirmar selecciones de instalación** página, haz clic en **instalar**.  
  
9. En la **progreso de la instalación** página, comprueba que todo lo que ha instalado correctamente y, a continuación, haz clic en **cerrar**.  
  
### <a name="to-install-the-ad-fs-server-role-via-windows-powershell"></a>Para instalar el rol de servidor de AD FS mediante Windows PowerShell  
  
1.  En el equipo que quieres configurar como un servidor de federación, abre la ventana de comandos de Windows PowerShell y, a continuación, ejecuta el siguiente comando:`Install-windowsfeature adfs-federation –IncludeManagementTools`.  
  
## <a name="see-also"></a>Consulta también 

[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guía de implementación de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implementar un conjunto de servidor de federación](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

