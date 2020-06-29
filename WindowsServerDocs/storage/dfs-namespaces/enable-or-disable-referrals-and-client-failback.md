---
title: Habilitar o deshabilitar las referencias y la conmutación por recuperación de clientes
description: En este artículo se describe cómo habilitar o deshabilitar las referencias y la conmutación por recuperación de cliente.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 15c02897df0710653a6a3663ce6f8e87bfa416b6
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85471810"
---
# <a name="enable-or-disable-referrals-and-client-failback"></a>Habilitar o deshabilitar las referencias y la conmutación por recuperación de clientes

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Una referencia es una lista ordenada de servidores que un equipo cliente recibe de un controlador de dominio o un servidor de espacio de nombres cuando el usuario tiene acceso a una raíz de espacio de nombres o a una carpeta DFS con destinos. Una vez que el equipo recibe la referencia, intenta tener acceso al primer servidor de la lista. Si el servidor no está disponible, el equipo cliente intenta tener acceso al siguiente servidor. Si un servidor deja de estar disponible, puede configurar los clientes para que conmuten por recuperación al servidor preferido una vez que esté disponible.

En las secciones siguientes se proporciona información sobre cómo habilitar o deshabilitar las referencias o habilitar la conmutación por recuperación de clientes:

## <a name="enable-or-disable-referrals"></a>Habilitación o deshabilitación de referencias

Al deshabilitar la referencia de un servidor de espacio de nombres o un destino de carpeta, puede impedir que los usuarios se dirijan a ese servidor de espacio de nombres o destino de carpeta. Esto resulta útil si necesita desconectar temporalmente un servidor para tareas de mantenimiento.

-   Para habilitar o deshabilitar las referencias a un destino de carpeta, siga estos pasos:

    1.  En el árbol de la consola de administración de DFS, en el nodo **espacios de nombres** , haga clic en una carpeta que contenga destinos y, a continuación, haga clic en la pestaña **destinos de carpeta** en el panel de **detalles** .
    2.  Haga clic con el botón secundario en el destino de carpeta y, a continuación, haga clic en **deshabilitar destino de carpeta** o **Habilitar destino de carpeta**.

-   Para habilitar o deshabilitar las referencias a un servidor de espacio de nombres, siga estos pasos:

    1.  En el árbol de consola de administración de DFS, seleccione el espacio de nombres adecuado y, a continuación, haga clic en la pestaña **servidores de espacio de nombres** .
    2.  Haga clic con el botón secundario en el servidor de espacio de nombres adecuado y seleccione **deshabilitar servidor** de espacio de nombres o **Habilitar servidor de espacio de nombres**.


> [!TIP]
> Para habilitar o deshabilitar las referencias mediante Windows PowerShell, use los cmdlets [set-DfsnRootTarget – State](https://technet.microsoft.com/library/jj884266.aspx) o [set-DfsnServerConfiguration](https://technet.microsoft.com/library/jj884277.aspx) , que se introdujeron en Windows Server 2012.

## <a name="enable-client-failback"></a>Habilitar la conmutación por recuperación de clientes

Si un destino no está disponible, puede configurar los clientes para que conmuten por recuperación al destino en cuanto éste se restaure. Para que la conmutación por recuperación funcione, los equipos cliente deben cumplir los requisitos enumerados en el siguiente tema: [revisar los requisitos del cliente de los espacios de nombres DFS](https://technet.microsoft.com/library/cc771913(v=ws.11).aspx).


> [!NOTE]
> Para habilitar la conmutación por recuperación de cliente en una raíz de espacio de nombres mediante Windows PowerShell, use el cmdlet [set-DfsnRoot](https://technet.microsoft.com/library/jj884281.aspx) . Para habilitar la conmutación por recuperación de cliente en una carpeta DFS, use el cmdlet [set-DfsnFolder](https://technet.microsoft.com/library/jj884283.aspx) .


## <a name="to-enable-client-failback-for-a-namespace-root"></a>Para habilitar la conmutación por recuperación de clientes para una raíz de espacio de nombres

1.  Haga clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **Espacios de nombres**, haga clic con el botón secundario en un espacio de nombres y, a continuación, haga clic en **Propiedades**.

3.  En la pestaña **Referencias**, seleccione la casilla **Conmutación por recuperación de los clientes a los destinos preferidos**.

Las carpetas con destinos heredan la configuración de conmutación por recuperación de clientes desde la raíz del espacio de nombres. Si la conmutación por recuperación de clientes en la raíz del espacio de nombres está deshabilitada, puede usar el siguiente procedimiento para habilitarla en una carpeta con destinos.

## <a name="to-enable-client-failback-for-a-folder-with-targets"></a>Para habilitar la conmutación por recuperación de clientes en una carpeta con destinos

1.  Haga clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **Espacios de nombres**, haga clic con el botón secundario en una carpeta con destinos y, a continuación, haga clic en **Propiedades**.

3.  En la pestaña **Referencias**, haga clic en la casilla **Conmutación por recuperación de los clientes a los destinos preferidos**.

## <a name="additional-references"></a>Referencias adicionales

-   [Ajustar espacios de nombres DFS](tuning-dfs-namespaces.md)
-   [Revisión de los requisitos del cliente de espacio de nombres DFS](https://technet.microsoft.com/library/cc771913(v=ws.11).aspx)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)