---
title: Identificar y resolver los problemas de operaciones del servidor de acceso remoto
description: Este tema forma parte de la Guía de supervisión de acceso remoto y las cuentas en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ce84c9f-fd1f-4463-8fc7-d2f33344a2c9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b682685883a2200caf8f4286674bb3e2cbe6651b
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282782"
---
# <a name="identify-and-resolve-remote-access-server-operations-problems"></a>Identificar y resolver los problemas de operaciones del servidor de acceso remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

**Nota:** Windows Server 2012 combina DirectAccess y el servicio de enrutamiento y acceso remoto (RRAS) en un único rol de acceso remoto.  
  
Puede que con los procedimientos siguientes para identificar problemas de operaciones del servidor de acceso remoto, sus causas y la resolución necesaria para corregir los problemas.  
  
> [!NOTE]  
> Debe ser iniciado sesión como miembro del grupo Admins. del dominio o un miembro del grupo Administradores en cada equipo para completar las tareas descritas en este tema. Si no se puede completar una tarea mientras ha iniciado sesión con una cuenta que sea miembro del grupo Administradores, intente realizar la tarea mientras ha iniciado sesión con una cuenta que sea miembro del grupo Admins. del dominio.  
  
En este tema incluye información sobre cómo realizar las tareas siguientes:  
  
- Simular un problema de operaciones  
  
- Identificar el problema de las operaciones y tomar medidas correctivas  
  
- Restaurar el servicio de aplicación auxiliar IP  
  
### <a name="BKMK_Simulate"></a>Simular un problema de operaciones  
  
> [!CAUTION]  
> Dado que el acceso remoto servidor probablemente está configurado correctamente y no tiene problemas, puede usar el procedimiento siguiente para simular un problema de las operaciones. Si el servidor está dando servicio a clientes en un entorno de producción, es posible que no desea realizar estas acciones en este momento. En su lugar, puede leer los pasos necesarios para entender cómo tratar los problemas que pueden surgir en el servidor de acceso remoto en el futuro.  
  
La aplicación auxiliar IP (IPHlpSvc) hosts IPv6 transición tecnologías de servicios (por ejemplo, Teredo, 6to4 o IP-HTTPS) y es necesario para el servidor de DirectAccess funcionar correctamente. Para demostrar un problema simulado operaciones en el servidor de acceso remoto, debe detener el servicio de red (IPHlpSvc).  
  
##### <a name="to-stop-the-ip-helper-service"></a>Para detener el servicio de aplicación auxiliar IP  
  
1.  En el **iniciar** pantalla del servidor de acceso remoto, haga clic en **herramientas administrativas**y, a continuación, haga doble clic en **servicios**.  
  
2.  En la lista de **servicios**, desplácese hacia abajo y haga clic en **aplicación auxiliar IP**y, a continuación, haga clic en **detener**.  
  
### <a name="BKMK_Identify"></a>Identificar el problema de las operaciones y tomar medidas correctivas  
Al desactivar el servicio de aplicación auxiliar IP provocará un error grave en el servidor de acceso remoto. El panel de supervisión mostrará el estado de las operaciones del servidor y los detalles del problema.  
  
##### <a name="to-identify-the-details-and-take-corrective-action"></a>Para identificar los detalles y tomar medidas correctivas  
  
1.  En **Administrador del servidor**, haz clic en **Herramientas** y, a continuación, haz clic en **Administración de acceso remoto**.  
  
2.  Haz clic en **PANEL** para ir a **Panel de acceso remoto** en la **Consola de administración de acceso remoto**.  
  
3.  Asegúrese de que su servidor de acceso remoto está seleccionado en el panel izquierdo y, a continuación, en el panel central, haga clic en **estado de las operaciones**.  
  
4.  Verá la lista de componentes con iconos rojo o verde, que indican su estado de funcionamiento. Haga clic en el **IP-HTTPS** fila en la lista. Cuando se selecciona una fila, se muestran los detalles de la operación en el **detalles** panel como sigue:  
  
    **Error**  
  
    Se detuvo el servicio de aplicación auxiliar IP (IPHlpSvc). DirectAccess podría no funcionar según lo previsto. El servicio de aplicación auxiliar IP proporciona conectividad de túnel mediante el uso de la plataforma de conectividad y tecnologías de transición IPv6 IP-HTTPS.  
  
    **Causas**  
  
    1.  Se detuvo el servicio auxiliar de IP.  
  
    2.  El servicio auxiliar de IP no responde.  
  
    **Resolución**  
  
    1.  Para asegurarse de que el servicio se está ejecutando, escriba **iphlpsc Get-Service** en un símbolo del sistema de Windows PowerShell.  
  
    2.  Para habilitar el servicio, escriba **iphlpsvc Start-Service** desde un símbolo del sistema de Windows PowerShell con privilegios elevados.  
  
    3.  Para reiniciar el servicio, escriba **iphlpsvc Restart-Service** desde un símbolo del sistema de Windows PowerShell con privilegios elevados.  
  
### <a name="BKMK_Restart"></a>Restaurar el servicio de aplicación auxiliar IP  
Para restaurar el servicio de aplicación auxiliar IP en el servidor de acceso remoto, puede seguir los pasos anteriores para iniciar o reiniciar el servicio o puede usar el procedimiento siguiente para revertir el procedimiento que usó para simular el error del servicio auxiliar de IP.  
  
##### <a name="to-restart-the-ip-helper-service-on-the-remote-access-server"></a>Para reiniciar el servicio de aplicación auxiliar IP en el servidor de acceso remoto  
  
1.  En el **iniciar** pantalla, haga clic en **herramientas administrativas**y, a continuación, haga doble clic en **servicios**.  
  
2.  En la lista de **servicios**, desplácese hacia abajo y haga clic en **aplicación auxiliar IP**y, a continuación, haga clic en **iniciar**.  
  
![Windows PowerShell](../../../media/Identify-and-resolve-Remote-Access-server-operations-problems/PowerShellLogoSmall.gif)***<em>comandos equivalentes de Windows PowerShell</em>***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
PS> Get-RemoteAccessHealth | Where-Object {$_.Component -eq "IP-HTTPS"} | Format-List -Property *  
```  
  


