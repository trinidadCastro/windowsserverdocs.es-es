---
title: Comprueba la configuración del equipo cliente
description: En este tema es parte de la BranchCache implementación Guía para Windows Server 2016, que se muestra cómo implementar BranchCache en modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN en sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 31ea58b0-d407-4f62-8ec6-6a1b19174042
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d507f2097a9349cbefba520ad0dc143e0dd7452e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="verify-client-computer-settings"></a>Comprueba la configuración del equipo cliente

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este procedimiento para comprobar que el equipo cliente correctamente configurado para BranchCache.  
  
> [!NOTE]  
> Este procedimiento incluye pasos para actualizar manualmente la directiva de grupo y reiniciar el servicio BranchCache. No es necesario realizar estas acciones si reinicia el equipo, como se producirán automáticamente en este caso.  
  
Debe ser miembro del **administradores**, o equivalente para realizar este procedimiento.  
  
### <a name="to-verify-branchcache-client-computer-settings"></a>Para comprobar la configuración del equipo cliente BranchCache  
  
1.  Para actualizar la directiva de grupo en el equipo cliente cuya configuración BranchCache desea comprobar, ejecutar Windows PowerShell como administrador, escribe el siguiente comando y, a continuación, presione ENTRAR.  
  
    `gpupdate /force`  
  
2.  Para los equipos cliente que están configurados en modo de la memoria caché hospedada y se configuran detectar automáticamente hospedarse servidores de caché mediante el punto de conexión de servicio, ejecuta los siguientes comandos para detener y reiniciar el servicio BranchCache.  
  
    `net stop peerdistsvc`  
  
    `net start peerdistsvc`  
  
3.  Inspeccionar el modo de funcionamiento BranchCache actual ejecutando el siguiente comando.  
  
    `Get-BCStatus`  
  
4.  En Windows PowerShell, revise la salida de la **Get BCStatus** comando.  
  
    El valor de **BranchCacheIsEnabled** debe ser **True**.  
  
    En **ClientSettings**, el valor de **CurrentClientMode** debe ser **DistributedClient** o **HostedCacheClient**, según el modo en que configuraste con esta guía.  
  
    En **ClientSettings**, si el modo de la memoria caché hospedada configurado y proporciona los nombres de los servidores de la memoria caché hospedada durante la configuración o si el cliente ha localizado automáticamente hospedado con puntos de conexión de servicio, los servidores de caché **HostedCacheServerList** debe tener un valor que es el mismo que el nombre o nombres de los servidores de la memoria caché hospedada. Por ejemplo, si el servidor de la memoria caché hospedada se denomina HCS1 y el dominio es corp.contoso.com, el valor de **HostedCacheServerList** es **HCS1.corp.contoso.com**.  
  
5.  Si cualquiera de lo BranchCache configuración enumerada anteriormente no tiene los valores correctos, usa los pasos de esta guía para comprobar la configuración de directiva de grupo o la directiva de equipo Local, así como las excepciones del firewall, que has configurado, y asegúrate de que son correctas. Además, reinicia el equipo o sigue los pasos de este procedimiento para actualizar la directiva de grupo y reinicia el servicio BranchCache.  
  


