---
title: Identificar y resolver los problemas de operaciones del servidor de acceso remoto
description: Obtenga información acerca de cómo identificar problemas de operaciones del servidor de acceso remoto, sus causas raíz y la solución necesaria para corregir los problemas.
manager: brianlic
ms.topic: article
ms.assetid: 7ce84c9f-fd1f-4463-8fc7-d2f33344a2c9
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 23a79e792b7601b4eeadf05e5844d9ca722f3c6d
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98039895"
---
# <a name="identify-and-resolve-remote-access-server-operations-problems"></a>Identificar y resolver los problemas de operaciones del servidor de acceso remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

**Nota:** Windows Server 2012 combina DirectAccess y el Servicio de enrutamiento y acceso remoto (RRAS) en un único rol de acceso remoto.

Puede usar los procedimientos siguientes para identificar problemas de operaciones del servidor de acceso remoto, sus causas raíz y la resolución necesaria para corregir los problemas.

> [!NOTE]
> Debe haber iniciado sesión como miembro del grupo Admins. del dominio o como miembro del grupo administradores en cada equipo para completar las tareas descritas en este tema. Si no puede completar una tarea mientras inicia sesión con una cuenta que sea miembro del grupo administradores, intente realizar la tarea mientras inicia sesión con una cuenta que sea miembro del grupo Admins. del dominio.

En este tema se incluye información acerca de cómo realizar las siguientes tareas:

- Simular un problema de operaciones

- Identificar el problema de las operaciones y tomar medidas correctivas

- Restaurar el servicio auxiliar de IP

### <a name="simulate-an-operations-issue"></a><a name="BKMK_Simulate"></a>Simular un problema de operaciones

> [!CAUTION]
> Dado que es probable que el servidor de acceso remoto esté configurado correctamente y no experimente problemas, puede usar el siguiente procedimiento para simular un problema de las operaciones. Si el servidor está atendiendo actualmente a los clientes en un entorno de producción, es posible que no desee realizar estas acciones en este momento. En su lugar, puede leer los pasos para entender cómo solucionar los problemas que pueden surgir en el servidor de acceso remoto en el futuro.

El servicio auxiliar de IP (IPHlpSvc) hospeda las tecnologías de transición IPv6 (como IP-HTTPS, 6to4 o Teredo) y es necesario para que el servidor de DirectAccess funcione correctamente. Para mostrar un problema de las operaciones simuladas en el servidor de acceso remoto, debe detener el servicio de red de (IPHlpSvc).

##### <a name="to-stop-the-ip-helper-service"></a>Para detener el servicio auxiliar de IP

1.  En la pantalla **Inicio** del servidor de acceso remoto, haga clic en **herramientas administrativas** y, a continuación, haga doble clic en **servicios**.

2.  En la lista de **servicios**, desplácese hacia abajo y haga clic con el botón secundario en **aplicación auxiliar de IP** y, a continuación, haga clic en **detener**.

### <a name="identify-the-operations-issue-and-take-corrective-action"></a><a name="BKMK_Identify"></a>Identificar el problema de las operaciones y tomar medidas correctivas
La desactivación del servicio auxiliar de IP producirá un error grave en el servidor de acceso remoto. El panel de supervisión mostrará el estado de las operaciones del servidor y los detalles del problema.

##### <a name="to-identify-the-details-and-take-corrective-action"></a>Para identificar los detalles y tomar medidas correctivas

1.  En **Administrador del servidor**, haz clic en **Herramientas** y, a continuación, haz clic en **Administración de acceso remoto**.

2.  Haz clic en **PANEL** para ir a **Panel de acceso remoto** en la **Consola de administración de acceso remoto**.

3.  Asegúrese de que el servidor de acceso remoto está seleccionado en el panel izquierdo y, a continuación, en el panel central, haga clic en **Estado de las operaciones**.

4.  Verá la lista de componentes con iconos verdes o rojos, que indican su estado operativo. Haga clic en la fila **IP-https** en la lista. Al seleccionar una fila, los detalles de la operación se muestran en el panel de **detalles** de la siguiente manera:

    **Error**

    El servicio auxiliar de IP (IPHlpSvc) se ha detenido. Es posible que DirectAccess no funcione como se esperaba. El servicio auxiliar de IP proporciona conectividad de túnel mediante la plataforma de conectividad, las tecnologías de transición IPv6 y IP-HTTPS.

    **Causas**

    1.  Se ha detenido el servicio auxiliar de IP.

    2.  El servicio auxiliar de IP no responde.

    **Resolución**

    1.  Para asegurarse de que el servicio se está ejecutando, escriba **Get-Service Iphlpsvc** en un símbolo del sistema de Windows PowerShell.

    2.  Para habilitar el servicio, escriba **Start-Service Iphlpsvc** desde un símbolo del sistema de Windows PowerShell con privilegios elevados.

    3.  Para reiniciar el servicio, escriba **restart-Service Iphlpsvc** desde un símbolo del sistema de Windows PowerShell con privilegios elevados.

### <a name="restore-the-ip-helper-service"></a><a name="BKMK_Restart"></a>Restaurar el servicio auxiliar de IP
Para restaurar el servicio auxiliar de IP en el servidor de acceso remoto, puede seguir los pasos de resolución anteriores para iniciar o reiniciar el servicio, o puede usar el procedimiento siguiente para revertir el procedimiento que usó para simular el error del servicio de aplicación auxiliar de IP.

##### <a name="to-restart-the-ip-helper-service-on-the-remote-access-server"></a>Para reiniciar el servicio auxiliar de IP en el servidor de acceso remoto

1.  En la pantalla **Inicio** , haga clic en **herramientas administrativas** y, a continuación, haga doble clic en **servicios**.

2.  En la lista de **servicios**, desplácese hacia abajo y haga clic con el botón secundario en **aplicación auxiliar de IP** y, a continuación, haga clic en **iniciar**.

![](../../../media/Identify-and-resolve-Remote-Access-server-operations-problems/PowerShellLogoSmall.gif) * *_<em>Comandos equivalentes</em>_* de Windows PowerShell para Windows PowerShell _

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

```PowerShell
PS> Get-RemoteAccessHealth | Where-Object {$_.Component -eq "IP-HTTPS"} | Format-List -Property _
```
