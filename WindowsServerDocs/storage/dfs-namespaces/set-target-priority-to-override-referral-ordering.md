---
title: Establecer la prioridad de destino para anular el orden de referencias
description: En este artículo se describe cómo establecer la prioridad de destino para anular el orden de referencias
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 59db08d5ef46b696f550a5fa0738c5c1f9375fda
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826366"
---
# <a name="set-target-priority-to-override-referral-ordering"></a>Establecer la prioridad de destino para anular el orden de referencias

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Una referencia es una lista ordenada de destinos que un equipo cliente recibe de un controlador de dominio o un servidor de espacio de nombres cuando el usuario tiene acceso a una carpeta o raíz de espacio de nombres con destinos en el espacio de nombres. Cada destino de una referencia se ordena según el método de ordenación para la carpeta o raíz de espacio de nombres. 

Para definir con más detalle el modo en que se ordenan los destinos, puedes establecer una prioridad para cada uno de los destinos. Por ejemplo, puedes especificar que un determinado destino sea el primero o el último de todos los destinos, o bien el primero o el último de todos los destinos que tengan el mismo coste.

## <a name="to-set-target-priority-on-a-root-target-for-a-domain-based-namespace"></a>Para establecer la prioridad de destinos en un destino de raíz para un espacio de nombres basado en dominio

Para establecer una prioridad de destinos en un destino de raíz para un espacio de nombres basado en dominio, utiliza el siguiente procedimiento:

1.  Haga clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de la consola, en el nodo **Espacios de nombres**, haz clic en el espacio de nombres basado en dominio a cuyos destinos de raíz deseas asignar una prioridad.

3.  En el panel **Detalles**, en la pestaña **Servidores de espacio de nombres**, haz clic con el botón derecho en el destino de raíz cuya prioridad quieres cambiar y luego haz clic en **Propiedades**.

4.  En la pestaña **Opciones avanzadas**, haz clic en **Invalidar orden de referencia** y luego haz clic en la prioridad que desees.

    -   **Primero de todos los destinos**: especifica que los usuarios siempre deben hacer referencia a este destino si está disponible.
    -   **Último de todos los destinos**: especifica que los usuarios nunca deben hacer referencia a este destino a menos que el resto de los destinos no estén disponibles.
    -   **Primero de los destinos de igual costo**: especifica que los usuarios deben hacer referencia a este destino antes que a otros destinos de igual coste (lo que suele significar otros destinos del mismo sitio).
    -   **Último de los destinos de igual costo**: especifica que los usuarios nunca deben hacer referencia a este destino si hay otros destinos de igual coste disponibles (lo que suele significar otros destinos del mismo sitio).

## <a name="to-set-target-priority-on-a-folder-target"></a>Para establecer la prioridad de destinos en un destino de carpeta

Para establecer la prioridad de destinos en un destino de carpeta, utilice el siguiente procedimiento:

1.  Haga clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **Espacios de nombres**, haz clic en la carpeta a cuyos destinos deseas asignar una prioridad.

3.  En el panel **Detalles**, en la pestaña **Destinos de carpeta**, haz clic con el botón derecho en el destino de carpeta cuya prioridad deseas cambiar y luego haz clic en **Propiedades**.

4.  En la pestaña **Opciones avanzadas**, haz clic en **Invalidar orden de referencia** y luego haz clic en la prioridad que desees.

> [!NOTE]
> Para establecer prioridades de destino mediante Windows PowerShell, usa los cmdlets [Set-DfsnRootTarget](https://technet.microsoft.com/library/jj884266.aspx) y [Set-DfsnFolderTarget](https://technet.microsoft.com/library/jj884264.aspx) con los parámetros **ReferralPriorityClass** y **ReferralPriorityRank**. Estos cmdlets se introdujeron en Windows Server 2012.

## <a name="see-also"></a>Vea también

-   [Optimización de espacios de nombres DFS](tuning-dfs-namespaces.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)