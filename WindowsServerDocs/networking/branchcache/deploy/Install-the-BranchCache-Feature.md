---
title: Instalar la característica BranchCache
description: Obtenga información acerca de cómo instalar la característica BranchCache e iniciar el servicio BranchCache en un equipo que ejecute Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.
manager: brianlic
ms.topic: get-started-article
ms.assetid: 4f31dc61-2dbe-4c7e-b3f9-85ae49a45049
ms.author: lizross
author: eross-msft
ms.openlocfilehash: abbc7fe051cda1f18b0baa6ec334ee3b339b2925
ms.sourcegitcommit: 029b1e19ce11160d5f988046e04a83e8ab5a60dc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97904170"
---
# <a name="install-the-branchcache-feature"></a>Instalar la característica BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para instalar la característica BranchCache e iniciar el servicio BranchCache en un equipo que ejecute Windows Server &reg; 2016, Windows server 2012 R2 o Windows server 2012.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o un grupo equivalente.

Antes de realizar este procedimiento, se recomienda que instale y configure su aplicación basada en BITS o el servidor Web.

> [!NOTE]
> Para llevar a cabo este procedimiento con Windows PowerShell, ejecute Windows PowerShell como administrador, escriba los siguientes comandos en el símbolo del sistema de Windows PowerShell y, a continuación, presione Entrar.
>
> `Install-WindowsFeature BranchCache`
>
> `Restart-Computer`

### <a name="to-install-and-enable-the-branchcache-feature"></a>Para instalar y habilitar la característica BranchCache

1.  En el Administrador del servidor, haga clic en **Administrar** y en **Agregar roles y características**. Se abre el Asistente para agregar roles y características. Haga clic en **Next**.

2.  En **Seleccionar tipo de instalación**, asegúrese de que esté seleccionada la opción instalación basada en **características o en roles** y, a continuación, haga clic en **siguiente**.

3.  En **Seleccionar servidor de destino**, asegúrese de que está seleccionado el servidor correcto y, a continuación, haga clic en **siguiente**.

4.  En **Seleccionar roles de servidor**, haz clic en **Siguiente**.

5.  En **seleccionar características**, haga clic en **BranchCache** y, a continuación, haga clic en **siguiente**.

6.  En **Confirmar selecciones de instalación**, haga clic en **Instalar**. En el progreso de la **instalación**, la instalación de la característica BranchCache continúa. Una vez finalizada la instalación, haga clic en **cerrar**.

Después de instalar la característica BranchCache, el servicio BranchCache (también denominado PeerDistSvc) está habilitado y el tipo de inicio es automático.



