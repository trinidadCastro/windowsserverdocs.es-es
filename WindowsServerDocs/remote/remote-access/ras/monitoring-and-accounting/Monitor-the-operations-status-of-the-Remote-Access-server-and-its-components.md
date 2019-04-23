---
title: Supervisar el estado de las operaciones del servidor de acceso remoto y sus componentes
description: Este tema forma parte de la Guía de supervisión de acceso remoto y las cuentas en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 077a3a64-2fa3-4994-9711-ec1fbdc081ba
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 83d12826bd8128bc38d84c252045d74cf416457e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882886"
---
# <a name="monitor-the-operations-status-of-the-remote-access-server-and-its-components"></a>Supervisar el estado de las operaciones del servidor de acceso remoto y sus componentes

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

**Nota:** Windows Server 2012 combina DirectAccess y el servicio de enrutamiento y acceso remoto (RRAS) en un único rol de acceso remoto.  
  
La consola de administración en el servidor de acceso remoto puede usarse para supervisar su estado de las operaciones.  
  
> [!NOTE]  
> Debe ser iniciado sesión como miembro del grupo Admins. del dominio o un miembro del grupo Administradores en cada equipo para completar la tarea descrita en este tema. Si no se puede completar una tarea mientras ha iniciado sesión con una cuenta que sea miembro del grupo Administradores, intente realizar la tarea mientras ha iniciado sesión con una cuenta que sea miembro del grupo Admins. del dominio.  
  
#### <a name="to-monitor-the-remote-access-server-operations-status"></a>Para supervisar el estado de las operaciones del servidor de acceso remoto  
  
1.  En **Administrador del servidor**, haz clic en **Herramientas** y, a continuación, haz clic en **Administración de acceso remoto**.  
  
2.  Haga clic en **panel** para navegar a **informes de acceso remoto** en el **consola de administración de acceso remoto**.  
  
3.  En el panel supervisión, tenga en cuenta la **estado de las operaciones** mosaico dentro de la **estado del servidor** icono. Este icono muestra el estado de las operaciones de servidor y el estado de todos los componentes del servidor.  
  
4.  Haga clic en **actualizar** en **tareas** en el panel derecho para volver a cargar el estado de las operaciones. El estado de las operaciones se actualiza automáticamente cada cinco minutos, que es el intervalo de actualización predeterminado. Para cambiar el intervalo de actualización de forma predeterminada, haga clic en **configurar el intervalo de actualización**.  
  
![Windows PowerShell](../../../media/Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
> [!NOTE]  
> El comando de estado de las operaciones de un clúster se incluye como referencia.  
  
```  
PS> Get-RemoteAccessHealth  
PS> Get-RemoteAccessHealth -Cluster  
```  
  


