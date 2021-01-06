---
title: Usar Windows PowerShell para configurar equipos cliente que no son miembros del dominio
description: Obtenga información acerca de cómo configurar manualmente un equipo cliente de BranchCache para el modo caché distribuida o el modo caché hospedada.
manager: brianlic
ms.topic: get-started-article
ms.assetid: 1b511e1a-686d-441f-a1c7-d4d029e1a061
ms.author: lizross
author: eross-msft
ms.openlocfilehash: fac1b4ffe9b51d28b7bce917352335921d3b700e
ms.sourcegitcommit: 029b1e19ce11160d5f988046e04a83e8ab5a60dc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97904490"
---
# <a name="use-windows-powershell-to-configure-non-domain-member-client-computers"></a>Usar Windows PowerShell para configurar equipos cliente que no son miembros del dominio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para configurar manualmente un equipo cliente de BranchCache para el modo caché distribuida o el modo caché hospedada.

> [!NOTE]
> Si ha configurado equipos cliente de BranchCache utilizando la directiva de grupo, la configuración de la directiva de grupo invalida cualquier configuración manual de equipos cliente a los que se aplican las directivas.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o un grupo equivalente.

### <a name="to-enable-branchcache-distributed-or-hosted-cache-mode"></a>Para habilitar el modo de caché hospedada o distribuida de BranchCache

1.  En el equipo cliente de BranchCache que desea configurar, ejecute Windows PowerShell como administrador y, a continuación, realice una de las siguientes acciones.

    -   Para configurar el equipo cliente para el modo caché distribuida de BranchCache, escriba el siguiente comando y, a continuación, presione Entrar.

        `Enable-BCDistributed`

    -   Para configurar el equipo cliente para el modo caché hospedada de BranchCache, escriba el siguiente comando y, a continuación, presione Entrar.

        `Enable-BCHostedClient`

        > [!TIP]
        > Si desea especificar los servidores de caché hospedada disponibles, use el `-ServerNames` parámetro con una lista separada por comas de los servidores de caché hospedada como valor de parámetro. Por ejemplo, si tiene dos servidores de caché hospedada denominados HCS1 y HCS2, configure el equipo cliente para el modo caché hospedada con el siguiente comando.
        >
        > `Enable-BCHostedClient -ServerNames HCS1,HCS2`



