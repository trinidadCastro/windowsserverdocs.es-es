---
title: Cambiar la cantidad de tiempo que los clientes mantienen en la caché las referencias
description: En este artículo se describe cómo cambiar la cantidad de tiempo que los clientes mantienen en la caché las referencias
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c6fcf64dc15404ca59e3ce5552b258f782441cfb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402258"
---
# <a name="change-the-amount-of-time-that-clients-cache-referrals"></a>Cambiar la cantidad de tiempo que los clientes mantienen en la caché las referencias

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Una referencia es una lista ordenada de destinos que un equipo cliente recibe de un controlador de dominio o un servidor de espacio de nombres cuando el usuario tiene acceso a una carpeta o raíz de espacio de nombres con destinos en el espacio de nombres. Puedes ajustar durante cuánto tiempo los clientes mantienen en la caché las referencias antes de solicitar una nueva.

## <a name="to-change-the-amount-of-time-that-clients-cache-namespace-root-referrals"></a>Para cambiar la cantidad de tiempo que los clientes mantienen en la caché las referencias a la raíz del espacio de nombres

1.  Haga clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **Espacios de nombres**, haga clic con el botón secundario en un espacio de nombres y, a continuación, haga clic en **Propiedades**.

3.  En la pestaña **Referencias**, en el cuadro de texto **Duración de la caché (en segundos)** , escriba la cantidad de tiempo (en segundos) que los clientes conservarán en la caché las referencias a la raíz del espacio de nombres. La configuración predeterminada es de 300 segundos (cinco minutos).

> [!TIP]
> Para cambiar la cantidad de tiempo que los clientes mantienen en la caché las referencias a la raíz del espacio de nombres mediante Windows PowerShell, usa el cmdlet [DfsnRoot TimeToLiveSec](https://technet.microsoft.com/library/jj884281.aspx) cmdlet. Estos cmdlets se introdujeron en Windows Server 2012.

## <a name="to-change-the-amount-of-time-that-clients-cache-folder-referrals"></a>Para cambiar la cantidad de tiempo que los clientes mantienen en la caché las referencias a las carpetas

1.  Haz clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de la consola, en el nodo **Espacios de nombres**, haga clic con el botón secundario en una carpeta con destinos y, a continuación, haga clic en **Propiedades**.

3.  En la pestaña **Referencias**, en el cuadro de texto **Duración de la caché (en segundos)** , escriba la cantidad de tiempo (en segundos) que los clientes conservarán en la caché las referencias a la carpeta. La configuración predeterminada es de 1.800 segundos (30 minutos).

## <a name="see-also"></a>Vea también

-   [Ajustar espacios de nombres DFS](tuning-dfs-namespaces.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)


