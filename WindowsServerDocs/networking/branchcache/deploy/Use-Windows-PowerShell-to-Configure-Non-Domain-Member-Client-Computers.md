---
title: Usar Windows PowerShell para configurar los equipos cliente no del dominio miembro
description: En este tema es parte de la BranchCache implementación Guía para Windows Server 2016, que se muestra cómo implementar BranchCache en modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN en sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 1b511e1a-686d-441f-a1c7-d4d029e1a061
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a5415d9fa5a4af806f23a1af9c907b1f02e9627b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="use-windows-powershell-to-configure-non-domain-member-client-computers"></a>Usar Windows PowerShell para configurar los equipos cliente no del dominio miembro

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puede usar este procedimiento para configurar manualmente un equipo de cliente BranchCache para el modo de caché distribuida u hospedado en modo de caché.  
  
> [!NOTE]  
> Si has configurado BranchCache los equipos cliente mediante Directiva de grupo, la configuración de directiva de grupo invalida cualquier configuración manual de los equipos cliente a la que se aplican las directivas.  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar este procedimiento.  
  
### <a name="to-enable-branchcache-distributed-or-hosted-cache-mode"></a>Para habilitar el modo de caché hospedada o distribuido BranchCache  
  
1.  En el equipo de cliente BranchCache que desea configurar, ejecutar Windows PowerShell como administrador y, a continuación, realiza una de las siguientes acciones.  
  
    -   Para configurar el equipo cliente para el modo de caché distribuida BranchCache, escribe el siguiente comando y, a continuación, presione ENTRAR.  
  
        `Enable-BCDistributed`  
  
    -   Para configurar el equipo cliente para el modo de caché BranchCache hospedado, escribe el siguiente comando y, a continuación, presione ENTRAR.  
  
        `Enable-BCHostedClient`  
  
        > [!TIP]  
        > Si quieres especificar los servidores de la memoria caché hospedada disponible, usa el `-ServerNames` parámetro con una coma lista separada de los servidores de la memoria caché hospedada como el valor del parámetro. Por ejemplo, si tienes dos servidores de memoria caché hospedada denominados HCS1 y HCS2, configurar el equipo cliente para el modo de la memoria caché hospedada con el siguiente comando.  
        >   
        > `Enable-BCHostedClient -ServerNames HCS1,HCS2`  
  


