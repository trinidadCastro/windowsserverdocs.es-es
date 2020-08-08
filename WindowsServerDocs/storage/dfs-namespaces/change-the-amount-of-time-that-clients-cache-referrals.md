---
title: Change the Amount of Time that Clients Cache Referrals
description: En este artículo se describe cómo cambiar la cantidad de tiempo que los clientes almacenan en caché las referencias
ms.date: 6/5/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 837b1016b1e601eb765d20877980ea75c2b8c70b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87957783"
---
# <a name="change-the-amount-of-time-that-clients-cache-referrals"></a>Change the amount of time that clients cache referrals

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Una referencia es una lista ordenada de destinos que un equipo cliente recibe de un controlador de dominio o un servidor de espacio de nombres cuando el usuario tiene acceso a una carpeta o raíz de espacio de nombres con destinos en el espacio de nombres. Puede ajustar el tiempo que los clientes mantienen en caché una referencia antes de solicitar una nueva.

## <a name="to-change-the-amount-of-time-that-clients-cache-namespace-root-referrals"></a>Para cambiar la cantidad de tiempo que los clientes mantienen en la memoria caché las referencias a la raíz del espacio de nombres

1.  Haga clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **Espacios de nombres**, haga clic con el botón secundario en un espacio de nombres y, a continuación, haga clic en **Propiedades**.

3.  En la pestaña **Referencias**, en el cuadro de texto **Duración de la caché (en segundos)**, escriba la cantidad de tiempo (en segundos) que los clientes conservarán en la memoria caché las referencias a la raíz del espacio de nombres. La configuración predeterminada es de 300 segundos (cinco minutos).

> [!TIP]
> Para cambiar la cantidad de tiempo que los clientes almacenan en caché las referencias a la raíz del espacio de nombres mediante Windows PowerShell, use el cmdlet [set-DfsnRoot TimeToLiveSec](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753448(v=ws.11)) . Estos cmdlets se introdujeron en Windows Server 2012.

## <a name="to-change-the-amount-of-time-that-clients-cache-folder-referrals"></a>Para cambiar la cantidad de tiempo que los clientes mantienen en la memoria caché las referencias a las carpetas

1.  Haga clic en **Inicio** , seleccione **herramientas administrativas**y, a continuación, haga clic en **Administración de DFS**.

2.  En el árbol de la consola, en el nodo **Espacios de nombres**, haga clic con el botón secundario en una carpeta con destinos y, a continuación, haga clic en **Propiedades**.

3.  En la pestaña **Referencias**, en el cuadro de texto **Duración de la caché (en segundos)**, escriba la cantidad de tiempo (en segundos) que los clientes conservarán en la memoria caché las referencias a la carpeta. La configuración predeterminada es de 1.800 segundos (30 minutos).

## <a name="additional-references"></a>Referencias adicionales

-   [Ajustar espacios de nombres DFS](tuning-dfs-namespaces.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)
