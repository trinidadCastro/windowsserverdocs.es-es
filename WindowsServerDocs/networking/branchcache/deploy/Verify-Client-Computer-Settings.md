---
title: Comprobar la configuración del equipo cliente
description: Este tema forma parte de la guía de implementación de BranchCache para Windows Server 2016, que muestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso del ancho de banda WAN en las sucursales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 31ea58b0-d407-4f62-8ec6-6a1b19174042
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6d0adbf0db2d7888ca12ca49f50fc37baa8cbc16
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356509"
---
# <a name="verify-client-computer-settings"></a>Comprobar la configuración del equipo cliente

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para comprobar que el equipo cliente está configurado correctamente para BranchCache.  
  
> [!NOTE]  
> En este procedimiento se incluyen los pasos para actualizar manualmente directiva de grupo y para reiniciar el servicio BranchCache. No es necesario realizar estas acciones si se reinicia el equipo, ya que se producirán automáticamente en este caso.  
  
Debe ser miembro de **los administradores**o equivalente para realizar este procedimiento.  
  
### <a name="to-verify-branchcache-client-computer-settings"></a>Para comprobar la configuración del equipo cliente de BranchCache  
  
1.  Para actualizar directiva de grupo en el equipo cliente cuya configuración de BranchCache desea comprobar, ejecute Windows PowerShell como administrador, escriba el siguiente comando y, a continuación, presione Entrar.  
  
    `gpupdate /force`  
  
2.  Para los equipos cliente que están configurados en modo caché hospedada y están configurados para detectar automáticamente servidores de caché hospedada por punto de conexión de servicio, ejecute los siguientes comandos para detener y reiniciar el servicio BranchCache.  
  
    `net stop peerdistsvc`  
  
    `net start peerdistsvc`  
  
3.  Inspeccione el modo operativo de BranchCache actual ejecutando el siguiente comando.  
  
    `Get-BCStatus`  
  
4.  En Windows PowerShell, revise la salida del comando **Get-BCStatus** .  
  
    El valor de **BranchCacheIsEnabled** debe ser **true**.  
  
    En **ClientSettings**, el valor de **CurrentClientMode** debe ser **DistributedClient** o **HostedCacheClient**, según el modo que haya configurado con esta guía.  
  
    En **ClientSettings**, si configuró el modo caché hospedada y proporcionó los nombres de los servidores de caché hospedada durante la configuración, o si el cliente ha encontrado automáticamente servidores de caché hospedada mediante puntos de conexión de servicio,  **HostedCacheServerList** debe tener un valor que sea el mismo que el nombre o los nombres de los servidores de caché hospedada. Por ejemplo, si el servidor de caché hospedada se denomina HCS1 y el dominio es corp.contoso.com, el valor de **HostedCacheServerList** es **HCS1.Corp.contoso.com**.  
  
5.  Si alguno de los valores de BranchCache indicados anteriormente no tiene los valores correctos, siga los pasos de esta guía para comprobar la configuración de la Directiva de directiva de grupo o de equipo local, así como las excepciones de firewall que configuró y asegúrese de que son correctas. Además, reinicie el equipo o siga los pasos de este procedimiento para actualizar directiva de grupo y reiniciar el servicio BranchCache.  
  


