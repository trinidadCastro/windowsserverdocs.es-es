---
title: Instalar la característica BranchCache
description: En este tema forma parte de BranchCache Deployment Guide para Windows Server 2016, que demuestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN de sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 4f31dc61-2dbe-4c7e-b3f9-85ae49a45049
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8b4aecd9e9355a6c2d5ac485ac77c76428fe295f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872196"
---
# <a name="install-the-branchcache-feature"></a>Instalar la característica BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para instalar la característica BranchCache e iniciar el servicio de BranchCache en un equipo que ejecuta Windows Server&reg; 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o un grupo equivalente.  
  
Antes de realizar este procedimiento, se recomienda que instale y configure su aplicación basada en BITS o el servidor Web.  
  
> [!NOTE]  
> Para llevar a cabo este procedimiento con Windows PowerShell, ejecute Windows PowerShell como administrador, escriba los siguientes comandos en el símbolo del sistema de Windows PowerShell y, a continuación, presione ENTRAR.  
>   
> `Install-WindowsFeature BranchCache`  
>   
> `Restart-Computer`  
  
### <a name="to-install-and-enable-the-branchcache-feature"></a>Para instalar y habilitar la característica BranchCache  
  
1.  En el Administrador del servidor, haga clic en **Administrar** y en **Agregar roles y características**. Se abre el Asistente para agregar Roles y características. Haz clic en **Siguiente**.  
  
2.  En **Seleccionar tipo de instalación**, asegúrese de que **instalación basada en roles o basada en características** está seleccionada y, a continuación, haga clic en **siguiente**.  
  
3.  En **Seleccionar servidor de destino**, asegúrese de que el servidor correcto está seleccionado y, a continuación, haga clic en **siguiente**.  
  
4.  En **Seleccionar roles de servidor**, haz clic en **Siguiente**.  
  
5.  En **seleccionar características**, haga clic en **BranchCache**y, a continuación, haga clic en **siguiente**.  
  
6.  En **Confirmar selecciones de instalación**, haga clic en **Instalar**. En **progreso de la instalación**, continúa la instalación de la característica BranchCache. Cuando se complete la instalación, haga clic en **cerrar**.  
  
Después de instalar la característica BranchCache, el servicio BranchCache - también se denomina el PeerDistSvc - está habilitado y el tipo de inicio es automático.  
  


