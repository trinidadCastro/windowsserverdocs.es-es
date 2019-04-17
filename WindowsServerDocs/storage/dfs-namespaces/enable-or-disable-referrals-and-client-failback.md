---
title: "Habilitar o deshabilitar las referencias y la conmutación por recuperación de clientes"
description: "En este artículo se describe cómo habilitar o deshabilitar las referencias y la conmutación por recuperación de clientes."
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: be827ba52beb65219dad30e8c182963054cfbd16
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="enable-or-disable-referrals-and-client-failback"></a>Habilitar o deshabilitar las referencias y la conmutación por recuperación de clientes

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Una referencia es una lista ordenada de servidores que un equipo cliente recibe de un controlador de dominio o un servidor de espacio de nombres cuando el usuario tiene acceso a una raíz de espacio de nombres o carpeta DFS con destinos. Una vez que el equipo recibe la referencia, intenta tener acceso al primer servidor de la lista. Si el servidor no está disponible, el equipo cliente intenta tener acceso al siguiente servidor. Si un servidor no está disponible, puedes configurar los clientes para que conmuten por recuperación al servidor preferido en cuanto esté disponible.

Las secciones siguientes proporcionan información sobre cómo habilitar o deshabilitar las referencias o habilitar la conmutación por recuperación de clientes:

## <a name="enable-or-disable-referrals"></a>Habilitar o deshabilitar referencias

Puedes impedir que se dirija a los usuarios a un servidor de espacio de nombres o un destino de carpeta deshabilitando una referencia a dicho servidor de espacio de nombres o destino de carpeta. Esto resulta útil si necesitas desconectar temporalmente un servidor para tareas de mantenimiento.

-   Para habilitar o deshabilitar las referencias a un destino de carpeta, realiza los siguientes pasos:

    1.  En el árbol de consola de Administración de DFS, en el nodo **Espacios de nombres**, haz clic en una carpeta que incluya destinos y luego haz clic en la pestaña **Destinos de carpeta** en el panel **Detalles**.
    2.  Haz clic con el botón derecho en el destino de carpeta y luego haz clic en **Deshabilitar destino de carpeta** o en **Habilitar destino de carpeta**, según corresponda.

-   Para habilitar o deshabilitar las referencias a un servidor de espacio de nombres, realiza los siguientes pasos:

    1.  En el árbol de consola de Administración de DFS, selecciona el espacio de nombres adecuado y después haz clic en la pestaña **Servidores de espacio de nombres**.
    2.  Haz clic con el botón derecho en el servidor de espacio de nombres y luego haz clic en **Deshabilitar servidor de espacio de nombres** o en **Habilitar servidor de espacio de nombres**, según corresponda.


> [!TIP]
> Para habilitar o deshabilitar las referencias mediante Windows PowerShell, usa los cmdlets [Set-DfsnRootTarget –State](https://technet.microsoft.com/library/jj884266.aspx) o [Set-DfsnServerConfiguration](https://technet.microsoft.com/library/jj884277.aspx), que se incluyeron en Windows Server 2012.

## <a name="enable-client-failback"></a>Habilitar la conmutación por recuperación de clientes

Si un destino no está disponible, puedes configurar los clientes para que conmuten por recuperación al destino en cuanto este se restaure. Para que la conmutación por recuperación funcione, los equipos cliente deben cumplir con los requisitos enumerados en el siguiente tema: [Revisión de los requisitos del cliente de espacio de nombres DFS](https://technet.microsoft.com/library/cc771913(v=ws.11).aspx).


> [!NOTE]
> Para habilitar la conmutación por recuperación de clientes en una raíz de espacio de nombres mediante Windows PowerShell, usa el cmdlet [Set-DfsnRoot](https://technet.microsoft.com/library/jj884281.aspx). Para habilitar la conmutación por recuperación de clientes en una carpeta DFS, usa el cmdlet [Set-DfsnFolder](https://technet.microsoft.com/library/jj884283.aspx).


## <a name="to-enable-client-failback-for-a-namespace-root"></a>Para habilitar la conmutación por recuperación de clientes para una raíz de espacio de nombres

1.  Haz clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **Espacios de nombres**, haz clic con el botón derecho en un espacio de nombres y luego haz clic en **Propiedades**.

3.  En la pestaña **Referencias**, selecciona la casilla **Conmutación por recuperación de los clientes a los destinos preferidos**.

Las carpetas con destinos heredan la configuración de conmutación por recuperación de clientes desde la raíz del espacio de nombres. Si la conmutación por recuperación de clientes en la raíz del espacio de nombres está deshabilitada, puedes usar el siguiente procedimiento para habilitarla en una carpeta con destinos.

## <a name="to-enable-client-failback-for-a-folder-with-targets"></a>Para habilitar la conmutación por recuperación de clientes en una carpeta con destinos

1.  Haz clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **Espacios de nombres**, haz clic con el botón derecho en una carpeta con destinos y luego haz clic en **Propiedades**.

3.  En la pestaña **Referencias**, haz clic en la casilla **Conmutación por recuperación de los clientes a los destinos preferidos**.

## <a name="see-also"></a>Consulta también 

-   [Ajustar espacios de nombres DFS](tuning-dfs-namespaces.md)
-   [Revisar los requisitos del cliente de espacios de nombres DFS](https://technet.microsoft.com/library/cc771913(v=ws.11).aspx)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)