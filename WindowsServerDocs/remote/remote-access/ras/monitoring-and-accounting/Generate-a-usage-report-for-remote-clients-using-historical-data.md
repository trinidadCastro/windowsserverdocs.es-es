---
title: Generar un informe de uso para clientes remotos mediante datos históricos
description: Este tema forma parte de la guía de supervisión y contabilidad de acceso remoto en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0305467b-ce39-4532-a05a-2cc5ff946f55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bae50345e8a6fd4018857e2a754d0274ce02855d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367252"
---
# <a name="generate-a-usage-report-for-remote-clients-using-historical-data"></a>Generar un informe de uso para clientes remotos mediante datos históricos

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

**Nota:** Windows Server 2012 combina DirectAccess y el servicio de enrutamiento y acceso remoto (RRAS) en un único rol de acceso remoto.  
  
La consola de administración de en el servidor de acceso remoto se puede usar para generar un informe de uso para los clientes remotos que acceden al servidor. Para generar un informe de uso para clientes remotos, primero debe habilitar las cuentas en el servidor de acceso remoto. Después de generar el informe, puede usar el panel de supervisión que está disponible en la consola de administración de en el servidor de acceso remoto para ver las estadísticas de carga en el servidor.  
  
> [!NOTE]  
> Debe haber iniciado sesión como miembro del grupo Admins. del dominio o como miembro del grupo administradores en cada equipo para completar las tareas descritas en este tema. Si no puede completar una tarea mientras inicia sesión con una cuenta que sea miembro del grupo administradores, intente realizar la tarea mientras inicia sesión con una cuenta que sea miembro del grupo Admins. del dominio.  
  
#### <a name="to-enable-accounting-on-the-remote-access-server"></a>Para habilitar las cuentas en el servidor de acceso remoto  
  
1.  En **Administrador del servidor**, haz clic en **Herramientas** y, a continuación, haz clic en **Administración de acceso remoto**.  
  
2.  Haga clic en **informes** para navegar a **informes de acceso remoto** en la **consola de administración de acceso remoto**.  
  
3.  Haga clic en **configurar cuentas** en el panel de tareas **informes de acceso remoto** .  
  
4.  Active la casilla **usar cuentas de bandeja de entrada** para habilitar las cuentas en el servidor de acceso remoto.  
  
5.  Haga clic en **aplicar** para habilitar la configuración de cuentas en el servidor y, a continuación, haga clic en **cerrar** después de que el servidor haya aplicado la configuración correctamente.  
  
#### <a name="to-generate-the-usage-report"></a>Para generar el informe de uso  
  
1.  En **Administrador del servidor**, haz clic en **Herramientas** y, a continuación, haz clic en **Administración de acceso remoto**.  
  
2.  Haga clic en **informes** para navegar a **informes de acceso remoto** en la **consola de administración de acceso remoto**.  
  
3.  En el panel central, haga clic en fechas en el calendario para seleccionar la **fecha de inicio** de la duración del informe: y **fecha de finalización:** y, a continuación, haga clic en **generar informe**.  
  
4.  Verá la lista de usuarios que se han conectado al servidor de acceso remoto en el tiempo seleccionado y estadísticas detalladas sobre ellos. Haga clic en la primera fila de la lista. Al seleccionar una fila, se muestra la actividad de usuario remoto en el panel de vista previa. Ahora, seleccione la pestaña **cargar estadísticas del servidor** en el panel de vista previa para ver la carga histórica en el servidor.  
  
    Haga clic en la pestaña **cargar estadísticas del servidor** en el panel de vista previa para ver la carga histórica en el servidor.  
  
> [!NOTE]  
> **Descripción de las sesiones**  
>   
> La contabilidad de acceso remoto se basa en el concepto de **sesiones**. A diferencia de una **conexión**, una **sesión** se identifica de forma única mediante una combinación de dirección IP de cliente remoto y nombre de usuario. Por ejemplo, si se forma un túnel de equipo desde el cliente remoto, denominado Client1, se creará una sesión y se almacenará en la base de datos de cuentas. Cuando un usuario llamado user1 se conecta desde ese cliente después de que transcurra un tiempo (pero el túnel del equipo todavía está activo), la sesión se registra como una sesión independiente. La distinción de sesiones es mantener la distinción entre el túnel del equipo y el túnel del usuario.  
  
](../../../media/Generate-a-usage-report-for-remote-clients-using-historical-data/PowerShellLogoSmall.gif)***<em>comandos equivalentes de Windows powershell</em> de @no__t 0Windows PowerShell***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
En el script siguiente, cambie el intervalo de fechas para el que desea un informe en los parámetros **-StartDateTime** y **-fechahorafin** .  
  
```  
PS> Get-RemoteAccessConnectionStatisticsSummary -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"  
Shows server load statistics.  
PS> Get-RemoteAccessUserActivity -HostIPAddress 10.0.0.1 -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"  
```  
  


