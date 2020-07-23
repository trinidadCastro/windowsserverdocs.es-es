---
title: Establecer la prioridad de destino para anular el orden de referencias
description: En este artículo se describe cómo establecer la prioridad de destino para invalidar el orden de referencia
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 77fe5b82b73a0f37ba81dda210f15d6017788822
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966827"
---
# <a name="set-target-priority-to-override-referral-ordering"></a>Set target priority to override referral ordering

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Una referencia es una lista ordenada de destinos que un equipo cliente recibe de un controlador de dominio o un servidor de espacio de nombres cuando el usuario tiene acceso a una carpeta o raíz de espacio de nombres con destinos en el espacio de nombres. Cada destino de una referencia se ordena según el método de ordenación para la carpeta o raíz de espacio de nombres.

Para definir con más detalle el modo en que se ordenan los destinos, puede establecer prioridades para cada uno de los destinos. Por ejemplo, puede especificar que el destino sea primero entre todos los destinos, el último entre todos los destinos, o el primero o el último entre todos los destinos de igual costo.

## <a name="to-set-target-priority-on-a-root-target-for-a-domain-based-namespace"></a>Para establecer la prioridad de destinos en un destino de raíz para un espacio de nombres basado en dominio

Para establecer la prioridad de destino en un destino de raíz para un espacio de nombres basado en dominio, utilice el procedimiento siguiente:

1.  Haga clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **espacios de nombres** , haga clic en el espacio de nombres basado en dominio para los destinos raíz para los que desea establecer la prioridad.

3.  En el panel de **detalles** , en la pestaña **servidores de espacio de nombres** , haga clic con el botón secundario en el destino de raíz cuya prioridad desee cambiar y, a continuación, haga clic en **propiedades**.

4.  En la pestaña **Opciones avanzadas** , haga clic en **invalidar orden de referencia**y, a continuación, haga clic en la prioridad que desee.

    -   **Primero entre todos los destinos**  Especifica que siempre se debe hacer referencia a los usuarios a este destino si el destino está disponible.
    -   **Último entre todos los destinos** Especifica que nunca se debe hacer referencia a este destino a los usuarios a menos que todos los demás destinos no estén disponibles.
    -   **Primero entre los destinos de igual costo**  Especifica que se debe hacer referencia a los usuarios a este destino antes que a otros destinos de igual costo (lo que suele significar otros destinos del mismo sitio).
    -   **Último de los destinos de igual costo**  Especifica que nunca se debe hacer referencia a este destino a los usuarios si hay otros destinos de igual costo disponibles (lo que suele significar otros destinos del mismo sitio).

## <a name="to-set-target-priority-on-a-folder-target"></a>Para establecer la prioridad de destinos en un destino de carpeta

Para establecer la prioridad de destino en un destino de carpeta, utilice el siguiente procedimiento:

1.  Haga clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de la consola, en el nodo **espacios de nombres** , haga clic en la carpeta de los destinos para los que desea establecer la prioridad.

3.  En el panel de **detalles** , en la pestaña destinos de la **carpeta** , haga clic con el botón secundario en el destino de carpeta cuya prioridad desee cambiar y, a continuación, haga clic en **propiedades** .

4.  En la pestaña **Opciones avanzadas** , haga clic en **invalidar orden de referencia** y, a continuación, haga clic en la prioridad que desee.

> [!NOTE]
> Para establecer las prioridades de destino mediante Windows PowerShell, use los cmdlets [set-DfsnRootTarget](/powershell/module/dfsr/update-dfsrconfigurationfromad?view=win10-ps) y [set-DfsnFolderTarget](/powershell/module/dfsr/update-dfsrconfigurationfromad?view=win10-ps) con los parámetros **ReferralPriorityClass** y **ReferralPriorityRank** . Estos cmdlets se introdujeron en Windows Server 2012.

## <a name="additional-references"></a>Referencias adicionales

-   [Ajustar espacios de nombres DFS](tuning-dfs-namespaces.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)
