---
title: Agregar destinos de carpeta
description: En este tema se describe cómo agregar destinos de carpeta (rutas UNC)
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms-date: 06/05/2017
ms.openlocfilehash: 8cc61189076669d5c24244294b2f0eee2b783517
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831006"
---
# <a name="add-folder-targets"></a>Agregar destinos de carpeta

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Un destino de carpeta es la ruta de acceso UNC (convención de nomenclatura universal) de una carpeta compartida o de otro espacio de nombres que se asocia con una carpeta en un espacio de nombres. Agregar varios destinos de carpeta aumenta la disponibilidad de la carpeta en el espacio de nombres.

## <a name="to-add-a-folder-target"></a>Para agregar un destino de carpeta

Para agregar un destino de carpeta mediante la administración de DFS, usa el siguiente procedimiento:

1.  Haga clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **Espacios de nombres**, haz clic con el botón derecho en una carpeta y luego haz clic en **Agregar destino de carpeta**.

3.  Escribe la ruta de acceso al destino de carpeta o haz clic en **Examinar** para buscar la carpeta de destino.

4.  Si la carpeta se replica mediante la replicación DFS, puedes especificar si el nuevo destino de carpeta se agrega al grupo de replicación.

> [!TIP]
> Para agregar un destino de carpeta mediante Windows PowerShell, usa el cmdlet [New-DfsnFolderTarget](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnfoldertarget). El módulo de Windows PowerShell para DFSN se introdujo en Windows Server 2012.

> [!NOTE]
> Las carpetas pueden incluir destinos de carpeta u otras carpetas DFS, pero no ambas, en el mismo nivel en la jerarquía de carpetas.

## <a name="see-also"></a>Vea también

-   [Implementación de espacios de nombres DFS](deploying-dfs-namespaces.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [Replicar destinos de carpeta mediante la replicación DFS](replicate-folder-targets-using-dfs-replication.md)