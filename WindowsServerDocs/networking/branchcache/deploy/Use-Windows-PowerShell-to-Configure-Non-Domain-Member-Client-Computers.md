---
title: Usar Windows PowerShell para configurar equipos cliente que no son miembros del dominio
description: Obtenga información acerca de cómo configurar manualmente un equipo cliente de BranchCache para el modo caché distribuida o el modo caché hospedada.
manager: brianlic
ms.topic: how-to
ms.assetid: 1b511e1a-686d-441f-a1c7-d4d029e1a061
ms.author: lizross
author: eross-msft
ms.date: 01/05/2021
ms.openlocfilehash: 5b745dcda699753c15d8c96af500633142f56add
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946751"
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



