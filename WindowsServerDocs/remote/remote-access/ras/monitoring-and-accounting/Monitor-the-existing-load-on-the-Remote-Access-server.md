---
title: Supervisar la carga existente en el servidor de acceso remoto
description: Este tema forma parte de la guía de supervisión y contabilidad de acceso remoto en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 62fa2895-62ae-42cf-817c-53e06ac2a26c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 9a04cdff7214fd4d7ef64fba930f282e48dbfa01
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970222"
---
# <a name="monitor-the-existing-load-on-the-remote-access-server"></a>Supervisar la carga existente en el servidor de acceso remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

**Nota:** Windows Server 2012 combina DirectAccess y el Servicio de enrutamiento y acceso remoto (RRAS) en un único rol de acceso remoto.

El término **carga** hace referencia a las estadísticas relacionadas con el número de conexiones en el servidor de acceso remoto. A continuación se indican los pasos necesarios para realizar el seguimiento de la carga en el servidor de acceso remoto.

Puede usar el panel de supervisión que está disponible en la consola de administración de en el servidor de acceso remoto para ver las estadísticas de carga del servidor, o puede usar los contadores del monitor de rendimiento para realizar el seguimiento de las estadísticas.

> [!NOTE]
> Debe haber iniciado sesión como miembro del grupo Admins. del dominio o como miembro del grupo administradores en cada equipo para completar las tareas descritas en este tema. Si no puede completar una tarea mientras inicia sesión con una cuenta que sea miembro del grupo administradores, intente realizar la tarea mientras inicia sesión con una cuenta que sea miembro del grupo Admins. del dominio.

#### <a name="to-use-the-monitoring-dashboard-to-monitor-the-remote-access-server-load"></a>Para usar el panel de supervisión para supervisar la carga del servidor de acceso remoto

1.  En **Administrador del servidor**, haz clic en **Herramientas** y, a continuación, haz clic en **Administración de acceso remoto**.

2.  Haz clic en **PANEL** para ir a **Panel de acceso remoto** en la **Consola de administración de acceso remoto**.

3.  En el panel supervisión, observe el icono **Estado de cliente remoto** en el icono **Estado del servidor** . Este icono muestra estadísticas como el número total de clientes remotos conectados, el número total de clientes de DirectAccess que están conectados y el número máximo de usuarios que se conectaron en las últimas 24 horas.

4.  Puede hacer clic en **Actualizar** en **tareas** en el panel derecho para volver a cargar el estado de mantenimiento. Para cambiar el intervalo de actualización predeterminado, haga clic en **Configurar intervalo de actualización** en **tareas**.

#### <a name="to-use-the-performance-monitor-tool-to-monitor-performance-counters-on-the-remote-access-server"></a>Para usar la herramienta Monitor de rendimiento con el fin de supervisar los contadores de rendimiento en el servidor de acceso remoto

1.  Haga clic en **Inicio**, en **herramientas administrativas**y, a continuación, haga doble clic en **monitor de rendimiento**.

2.  En **rendimiento**, haga clic en **monitor de rendimiento**.

3.  Haga clic en el botón **Agregar** (indicado por un icono de cruz verde) en la barra de herramientas del **monitor de rendimiento** .

4.  En la lista de **contadores disponibles**, seleccione todos los contadores de las categorías **ras** y **RAmgmtsvc** y, a continuación, haga clic en **Agregar>>**.

5.  De nuevo, en la lista de **contadores disponibles**, seleccione todos los contadores en la categoría **conexiones IPSec** y, a continuación, haga clic en **Agregar>>.**

6.  Haga clic en **Aceptar** para agregar los contadores seleccionados en la consola del **monitor de rendimiento** para el seguimiento.

El **monitor de rendimiento** mostrará ahora gráficamente las estadísticas de carga del servidor seleccionado.

![](../../../media/Monitor-the-existing-load-on-the-Remote-Access-server/PowerShellLogoSmall.gif)***<em>Comandos equivalentes</em> de Windows PowerShell Windows PowerShell***

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

```
PS> Get-RemoteAccessConnectionStatisticsSummary
```



