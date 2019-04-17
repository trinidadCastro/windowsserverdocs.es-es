---
title: Instalar la característica BranchCache
description: En este tema es parte de la BranchCache implementación Guía para Windows Server 2016, que se muestra cómo implementar BranchCache en modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN en sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 4f31dc61-2dbe-4c7e-b3f9-85ae49a45049
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a69848536b56521da9b5ef07689aba7f8690e888
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-branchcache-feature"></a>Instalar la característica BranchCache

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este procedimiento para instalar la característica BranchCache e iniciar el servicio de BranchCache en un equipo que ejecute Windows Server&reg; de 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
Pertenencia a **administradores** o equivalente, es lo mínimo necesario para realizar este procedimiento.  
  
Antes de realizar este procedimiento, se recomienda que instala y configura tu servidor Web o aplicación basada en BITS.  
  
> [!NOTE]  
> Para realizar este procedimiento mediante Windows PowerShell, ejecuta Windows PowerShell como administrador, escribe los siguientes comandos en el símbolo del sistema de Windows PowerShell y, a continuación, presione ENTRAR.  
>   
> `Install-WindowsFeature BranchCache`  
>   
> `Restart-Computer`  
  
### <a name="to-install-and-enable-the-branchcache-feature"></a>Para instalar y habilitar la característica BranchCache  
  
1.  En el administrador del servidor, haz clic en **administrar**y, a continuación, haz clic en **agregar Roles y características**. Abre el Asistente para agregar Roles y características. Haz clic en **siguiente**.  
  
2.  En **selecciona el tipo de instalación**, asegúrate de que **instalación basada en rol o característica** está seleccionado y, a continuación, haz clic en **siguiente**.  
  
3.  En **servidor de destino selecciona**, asegúrate de que el servidor correcto está seleccionado y, a continuación, haz clic en **siguiente**.  
  
4.  En **seleccionar roles de servidor**, haz clic en **siguiente**.  
  
5.  En **Select features**, haz clic en **BranchCache**y, a continuación, haz clic en **siguiente**.  
  
6.  En **Confirmar selecciones de instalación**, haz clic en **instalar**. En **progreso de la instalación**, la instalación de la característica BranchCache continuará. Una vez finalizada la instalación, haz clic en **cerrar**.  
  
Después de instalar la característica BranchCache, el servicio BranchCache - también se denomina la PeerDistSvc - está habilitado y el tipo de inicio es automático.  
  


