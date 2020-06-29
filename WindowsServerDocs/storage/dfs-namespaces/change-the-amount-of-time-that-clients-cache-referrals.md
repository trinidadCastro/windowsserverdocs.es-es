---
title: Change the Amount of Time that Clients Cache Referrals
description: En este artículo se describe cómo cambiar la cantidad de tiempo que los clientes almacenan en caché las referencias
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: d421a14c2a6021d45cd16f30c526ff1670ae62e3
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475202"
---
# <a name="change-the-amount-of-time-that-clients-cache-referrals"></a>Change the amount of time that clients cache referrals

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Una referencia es una lista ordenada de destinos que un equipo cliente recibe de un controlador de dominio o un servidor de espacio de nombres cuando el usuario tiene acceso a una carpeta o raíz de espacio de nombres con destinos en el espacio de nombres. Puede ajustar el tiempo que los clientes mantienen en caché una referencia antes de solicitar una nueva.

## <a name="to-change-the-amount-of-time-that-clients-cache-namespace-root-referrals"></a>Para cambiar la cantidad de tiempo que los clientes mantienen en la memoria caché las referencias a la raíz del espacio de nombres

1.  Haga clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **Espacios de nombres**, haga clic con el botón secundario en un espacio de nombres y, a continuación, haga clic en **Propiedades**.

3.  En la pestaña **Referencias**, en el cuadro de texto **Duración de la caché (en segundos)**, escriba la cantidad de tiempo (en segundos) que los clientes conservarán en la memoria caché las referencias a la raíz del espacio de nombres. La configuración predeterminada es de 300 segundos (cinco minutos).

> [!TIP]
> Para cambiar la cantidad de tiempo que los clientes almacenan en caché las referencias a la raíz del espacio de nombres mediante Windows PowerShell, use el cmdlet [set-DfsnRoot TimeToLiveSec](https://technet.microsoft.com/library/jj884281.aspx) . Estos cmdlets se introdujeron en Windows Server 2012.

## <a name="to-change-the-amount-of-time-that-clients-cache-folder-referrals"></a>Para cambiar la cantidad de tiempo que los clientes mantienen en la memoria caché las referencias a las carpetas

1.  Haga clic en **Inicio** , seleccione **herramientas administrativas**y, a continuación, haga clic en **Administración de DFS**.

2.  En el árbol de la consola, en el nodo **Espacios de nombres**, haga clic con el botón secundario en una carpeta con destinos y, a continuación, haga clic en **Propiedades**.

3.  En la pestaña **Referencias**, en el cuadro de texto **Duración de la caché (en segundos)**, escriba la cantidad de tiempo (en segundos) que los clientes conservarán en la memoria caché las referencias a la carpeta. La configuración predeterminada es de 1.800 segundos (30 minutos).

## <a name="additional-references"></a>Referencias adicionales

-   [Ajustar espacios de nombres DFS](tuning-dfs-namespaces.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)


