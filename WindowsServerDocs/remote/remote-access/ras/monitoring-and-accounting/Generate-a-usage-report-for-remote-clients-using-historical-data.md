---
title: Generar un informe de uso para clientes remotos mediante datos históricos
description: Este tema forma parte de la Guía de supervisión de acceso remoto y las cuentas en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0305467b-ce39-4532-a05a-2cc5ff946f55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cfbac18f64123f97c54b29c1aeef7364af55e49a
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281134"
---
# <a name="generate-a-usage-report-for-remote-clients-using-historical-data"></a>Generar un informe de uso para clientes remotos mediante datos históricos

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

**Nota:** Windows Server 2012 combina DirectAccess y el servicio de enrutamiento y acceso remoto (RRAS) en un único rol de acceso remoto.  
  
La consola de administración en el servidor de acceso remoto puede usarse para generar un informe de uso para los clientes remotos que tienen acceso al servidor. Para generar un informe de uso para los clientes remotos, primero hay que habilitar cuentas en el servidor de acceso remoto. Después de generar el informe, puede usar el panel de supervisión que está disponible en la consola de administración en el servidor de acceso remoto para ver las estadísticas de carga en el servidor.  
  
> [!NOTE]  
> Debe ser iniciado sesión como miembro del grupo Admins. del dominio o un miembro del grupo Administradores en cada equipo para completar las tareas descritas en este tema. Si no se puede completar una tarea mientras ha iniciado sesión con una cuenta que sea miembro del grupo Administradores, intente realizar la tarea mientras ha iniciado sesión con una cuenta que sea miembro del grupo Admins. del dominio.  
  
#### <a name="to-enable-accounting-on-the-remote-access-server"></a>Para habilitar las cuentas en el servidor de acceso remoto  
  
1.  En **Administrador del servidor**, haz clic en **Herramientas** y, a continuación, haz clic en **Administración de acceso remoto**.  
  
2.  Haga clic en **informes** para navegar a **informes de acceso remoto** en el **consola de administración de acceso remoto**.  
  
3.  Haga clic en **configurar Contabilidad** en el **informes de acceso remoto** panel de tareas.  
  
4.  Seleccione el **usar las cuentas de la Bandeja de entrada** casilla de verificación para habilitar las cuentas en el servidor de acceso remoto.  
  
5.  Haga clic en **aplicar** para habilitar la configuración de cuentas en el servidor y, a continuación, haga clic en **cerrar** después de que el servidor ha aplicado la configuración correctamente.  
  
#### <a name="to-generate-the-usage-report"></a>Para generar el informe de uso  
  
1.  En **Administrador del servidor**, haz clic en **Herramientas** y, a continuación, haz clic en **Administración de acceso remoto**.  
  
2.  Haga clic en **informes** para navegar a **informes de acceso remoto** en el **consola de administración de acceso remoto**.  
  
3.  En el panel central, haga clic en las fechas del calendario para seleccionar la duración del informe **fecha de inicio:** y **fecha de finalización:** y, a continuación, haga clic en **generar informe**.  
  
4.  Verá la lista de usuarios que se han conectado al servidor de acceso remoto en el tiempo seleccionado y estadísticas detalladas sobre ellos. Haga clic en la primera fila de la lista. Cuando se selecciona una fila, se muestra la actividad del usuario remoto en el panel de vista previa. Ahora seleccione el **las estadísticas de carga del servidor** ficha en el panel de vista previa para ver el histórico de carga en el servidor.  
  
    Haga clic en el **las estadísticas de carga del servidor** ficha en el panel de vista previa para ver el histórico de carga en el servidor.  
  
> [!NOTE]  
> **Descripción de las sesiones**  
>   
> Cuentas de acceso remoto se basa en el concepto de **sesiones**. En contraposición con un **conexión**, un **sesión** se identifica mediante una combinación de nombre de usuario y dirección IP de cliente remoto. Por ejemplo, si está formado un túnel de equipo desde el cliente remoto, denominado CLIENTE1, una sesión se crea y almacena en la base de datos. Cuando pasa de un usuario con nombre User1 se conecta desde ese cliente más tarde (pero el túnel de equipo todavía está activo), la sesión se registra como una sesión independiente. La diferencia de las sesiones es conservar la distinción entre el túnel de equipo y el túnel de usuario.  
  
![Windows PowerShell](../../../media/Generate-a-usage-report-for-remote-clients-using-historical-data/PowerShellLogoSmall.gif)***<em>comandos equivalentes de Windows PowerShell</em>***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
En la siguiente secuencia de comandos, cambie el intervalo de fechas que se desea un informe en el **- StartDateTime** y **- Fechahorafin** parámetros.  
  
```  
PS> Get-RemoteAccessConnectionStatisticsSummary -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"  
Shows server load statistics.  
PS> Get-RemoteAccessUserActivity -HostIPAddress 10.0.0.1 -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"  
```  
  


