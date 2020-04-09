---
title: Supervisar el estado de las operaciones del servidor de acceso remoto y sus componentes
description: Este tema forma parte de la guía de supervisión y contabilidad de acceso remoto en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 077a3a64-2fa3-4994-9711-ec1fbdc081ba
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a93f8100b16da1cabbda8ed3e273a2601adb647a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860528"
---
# <a name="monitor-the-operations-status-of-the-remote-access-server-and-its-components"></a>Supervisar el estado de las operaciones del servidor de acceso remoto y sus componentes

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

**Nota:** Windows Server 2012 combina DirectAccess y el Servicio de enrutamiento y acceso remoto (RRAS) en un único rol de acceso remoto.  
  
La consola de administración de en el servidor de acceso remoto se puede usar para supervisar el estado de las operaciones.  
  
> [!NOTE]  
> Debe haber iniciado sesión como miembro del grupo Admins. del dominio o como miembro del grupo administradores en cada equipo para completar la tarea que se describe en este tema. Si no puede completar una tarea mientras inicia sesión con una cuenta que sea miembro del grupo administradores, intente realizar la tarea mientras inicia sesión con una cuenta que sea miembro del grupo Admins. del dominio.  
  
#### <a name="to-monitor-the-remote-access-server-operations-status"></a>Para supervisar el estado de las operaciones del servidor de acceso remoto  
  
1.  En **Administrador del servidor**, haz clic en **Herramientas** y, a continuación, haz clic en **Administración de acceso remoto**.  
  
2.  Haga clic en **Panel** para ir a **informes de acceso remoto** en la **consola de administración de acceso remoto**.  
  
3.  En el panel supervisión, observe el icono **Estado de las operaciones** en el icono **Estado del servidor** . Este icono muestra el estado de las operaciones del servidor y el estado de todos los componentes del servidor.  
  
4.  Haga clic en **Actualizar** en **tareas** en el panel derecho para recargar el estado de las operaciones. El estado de las operaciones se actualiza automáticamente cada cinco minutos, que es el intervalo de actualización predeterminado. Para cambiar el intervalo de actualización predeterminado, haga clic en **Configurar intervalo de actualización**.  
  
![](../../../media/Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components/PowerShellLogoSmall.gif)***<em>comandos equivalentes</em> de Windows PowerShell Windows PowerShell***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, incluso aunque puedan aparecer con las palabras ajustadas en varias líneas aquí debido a las restricciones de formato.  
  
> [!NOTE]  
> El comando para el estado de las operaciones de un clúster se incluye como referencia.  
  
```  
PS> Get-RemoteAccessHealth  
PS> Get-RemoteAccessHealth -Cluster  
```  
  


