---
title: Compruebe la configuración de equipos cliente
description: En este tema forma parte de BranchCache Deployment Guide para Windows Server 2016, que demuestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN de sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 31ea58b0-d407-4f62-8ec6-6a1b19174042
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d628886186474d3f05d7961ca3d3b45b8bf12e73
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834056"
---
# <a name="verify-client-computer-settings"></a>Compruebe la configuración de equipos cliente

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para comprobar que el equipo cliente está configurado correctamente para BranchCache.  
  
> [!NOTE]  
> Este procedimiento incluye pasos para actualizar manualmente la directiva de grupo y reiniciar el servicio de BranchCache. No es necesario realizar estas acciones si reinicia el equipo, ya que se producirán automáticamente en este caso.  
  
Debe ser miembro del **administradores**, o equivalente para realizar este procedimiento.  
  
### <a name="to-verify-branchcache-client-computer-settings"></a>Para comprobar la configuración de equipos cliente de BranchCache  
  
1.  Para actualizar la directiva de grupo en el equipo cliente cuya configuración de BranchCache que desea comprobar, ejecute Windows PowerShell como administrador, escriba el siguiente comando y, a continuación, presione ENTRAR.  
  
    `gpupdate /force`  
  
2.  Para los equipos cliente que están configurados en modo de caché hospedada y se configuran detectar automáticamente servidores de caché hospedada por punto de conexión de servicio, ejecute los siguientes comandos para detener y reiniciar el servicio de BranchCache.  
  
    `net stop peerdistsvc`  
  
    `net start peerdistsvc`  
  
3.  Inspeccione el modo de funcionamiento de BranchCache actual, ejecute el comando siguiente.  
  
    `Get-BCStatus`  
  
4.  En Windows PowerShell, revise la salida de la **Get BCStatus** comando.  
  
    El valor de **BranchCacheIsEnabled** debe ser **True**.  
  
    En **ClientSettings**, el valor de **CurrentClientMode** debe ser **DistributedClient** o **HostedCacheClient**, en función de la modo que se han configurado con esta guía.  
  
    En **ClientSettings**si configura el modo caché hospedada y proporcionan los nombres de los servidores de caché hospedada durante la configuración o si el cliente automáticamente se encuentra hospedado mediante puntos de conexión de servicio, los servidores de caché  **HostedCacheServerList** debe tener un valor que es el mismo que el nombre o nombres de los servidores de caché hospedada. Por ejemplo, si el servidor de caché hospedada se denomina HCS1 y el dominio es corp.contoso.com, el valor de **HostedCacheServerList** es **HCS1.corp.contoso.com**.  
  
5.  Si alguno de lo valores enumerados anteriormente de BranchCache no tiene los valores correctos, use los pasos de esta guía para comprobar la configuración de directiva de grupo o directiva de equipo Local, así como las excepciones de firewall que ha configurado, y asegúrese de que sean correctos. Además, reinicie el equipo o siga los pasos de este procedimiento para actualizar la directiva de grupo y reinicie el servicio de BranchCache.  
  


