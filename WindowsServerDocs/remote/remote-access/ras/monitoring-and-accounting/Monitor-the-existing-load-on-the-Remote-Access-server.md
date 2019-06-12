---
title: Supervisar la carga existente en el servidor de acceso remoto
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
ms.assetid: 62fa2895-62ae-42cf-817c-53e06ac2a26c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6d7a5aa7b699f5a8f24c4a36ee8ae314768329b4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446859"
---
# <a name="monitor-the-existing-load-on-the-remote-access-server"></a>Supervisar la carga existente en el servidor de acceso remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

**Nota:** Windows Server 2012 combina DirectAccess y el servicio de enrutamiento y acceso remoto (RRAS) en un único rol de acceso remoto.  
  
El término **carga** hace referencia a las estadísticas que se relacionan con el número de conexiones en el servidor de acceso remoto. Estos son los pasos necesarios para realizar un seguimiento de la carga en el servidor de acceso remoto.  
  
Puede usar el panel de supervisión que está disponible en la consola de administración en el servidor de acceso remoto para ver las estadísticas de carga para el servidor, o puede utilizar los contadores del Monitor de rendimiento para realizar el seguimiento de las estadísticas.  
  
> [!NOTE]  
> Debe ser iniciado sesión como miembro del grupo Admins. del dominio o un miembro del grupo Administradores en cada equipo para completar las tareas descritas en este tema. Si no se puede completar una tarea mientras ha iniciado sesión con una cuenta que sea miembro del grupo Administradores, intente realizar la tarea mientras ha iniciado sesión con una cuenta que sea miembro del grupo Admins. del dominio.  
  
#### <a name="to-use-the-monitoring-dashboard-to-monitor-the-remote-access-server-load"></a>Para utilizar el panel de supervisión para supervisar la carga del servidor de acceso remoto  
  
1.  En **Administrador del servidor**, haz clic en **Herramientas** y, a continuación, haz clic en **Administración de acceso remoto**.  
  
2.  Haz clic en **PANEL** para ir a **Panel de acceso remoto** en la **Consola de administración de acceso remoto**.  
  
3.  En el panel supervisión, tenga en cuenta la **estado de cliente remoto** mosaico dentro de la **estado del servidor** icono. Este icono muestra las estadísticas como el número total de clientes remotos que están conectados, el número total de clientes de DirectAccess que están conectados y el número máximo de usuarios que se han conectado en las últimas 24 horas.  
  
4.  Puede hacer clic en **actualizar** en **tareas** en el panel derecho para volver a cargar el estado de mantenimiento. Para cambiar el intervalo de actualización de forma predeterminada, haga clic en **configurar el intervalo de actualización** en **tareas**.  
  
#### <a name="to-use-the-performance-monitor-tool-to-monitor-performance-counters-on-the-remote-access-server"></a>Para usar la herramienta Monitor de rendimiento para supervisar los contadores de rendimiento en el servidor de acceso remoto  
  
1.  Haga clic en **iniciar**, haga clic en **herramientas administrativas**y, a continuación, haga doble clic en **Monitor de rendimiento**.  
  
2.  En **rendimiento**, haga clic en **Monitor de rendimiento**.  
  
3.  Haga clic en el **agregar** botón (indicado por un icono de cruz verde) en el **Monitor de rendimiento** barra de herramientas.  
  
4.  En la lista de **contadores disponibles**, seleccione todos los contadores en el **RAS** y **RAmgmtsvc** categorías y, a continuación, haga clic en **Agregar >>** .  
  
5.  De nuevo, en la lista de **contadores disponibles**, seleccione todos los contadores en el **conexiones IPsec** categoría y, a continuación, haga clic en **Agregar >>.**  
  
6.  Haga clic en **Aceptar** para agregar los contadores seleccionados en el **Monitor de rendimiento** consola para el seguimiento.  
  
**Monitor de rendimiento** gráficamente mostrará ahora las estadísticas de carga del servidor seleccionado.  
  
![Windows PowerShell](../../../media/Monitor-the-existing-load-on-the-Remote-Access-server/PowerShellLogoSmall.gif)***<em>comandos equivalentes de Windows PowerShell</em>***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
PS> Get-RemoteAccessConnectionStatisticsSummary  
```  
  


