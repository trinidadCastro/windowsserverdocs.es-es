---
title: Usar Windows PowerShell para configurar equipos cliente miembros que no sea de dominio
description: En este tema forma parte de BranchCache Deployment Guide para Windows Server 2016, que demuestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN de sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 1b511e1a-686d-441f-a1c7-d4d029e1a061
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9abb77e573d7b3f144ab831c655c81370a4a6af1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839956"
---
# <a name="use-windows-powershell-to-configure-non-domain-member-client-computers"></a>Usar Windows PowerShell para configurar equipos cliente miembros que no sea de dominio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para configurar manualmente un equipo cliente de BranchCache para el modo caché distribuida o modo caché hospedada.  
  
> [!NOTE]  
> Si ha configurado equipos cliente de BranchCache utilizando la directiva de grupo, la configuración de la directiva de grupo invalida cualquier configuración manual de equipos cliente a los que se aplican las directivas.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o un grupo equivalente.  
  
### <a name="to-enable-branchcache-distributed-or-hosted-cache-mode"></a>Para habilitar el modo caché distribuida u hospedada de BranchCache  
  
1.  En el equipo cliente de BranchCache que desea configurar, ejecutar Windows PowerShell como administrador y, a continuación, realice una de las siguientes acciones.  
  
    -   Para configurar el equipo cliente para el modo caché distribuida de BranchCache, escriba el siguiente comando y, a continuación, presione ENTRAR.  
  
        `Enable-BCDistributed`  
  
    -   Para configurar el equipo cliente para el modo de caché hospedada de BranchCache, escriba el siguiente comando y, a continuación, presione ENTRAR.  
  
        `Enable-BCHostedClient`  
  
        > [!TIP]  
        > Si desea especificar los servidores de caché hospedada disponibles, utilice el `-ServerNames` lista de los servidores de caché hospedada como el valor del parámetro separados por el parámetro con una coma. Por ejemplo, si tiene dos servidores de caché hospedada denominados HCS1 y HCS2, configurar el equipo cliente para el modo caché hospedada con el siguiente comando.  
        >   
        > `Enable-BCHostedClient -ServerNames HCS1,HCS2`  
  


