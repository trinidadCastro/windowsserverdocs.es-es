---
title: Add Folder Targets
description: En este tema se describe cómo agregar destinos de carpeta (rutas UNC)
ms.prod: windows-server
ms.author: jgerend
manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms-date: 06/05/2017
ms.openlocfilehash: d2f3845a612556a51692aaf51d256bbedd518e7a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854108"
---
# <a name="add-folder-targets"></a>Add folder targets

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Un destino de carpeta es la ruta de acceso UNC (convención de nomenclatura universal) de una carpeta compartida o de otro espacio de nombres que se asocia con una carpeta en un espacio de nombres. La adición de varios destinos de carpeta permite aumentar la disponibilidad de la carpeta en el espacio de nombres.

## <a name="to-add-a-folder-target"></a>Para agregar un destino de carpeta

Para agregar un destino de carpeta mediante la administración de DFS, usa el siguiente procedimiento:

1.  Haga clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **Espacios de nombres**, haz clic con el botón derecho en una carpeta y luego haz clic en **Agregar destino de carpeta**.

3.  Escribe la ruta de acceso al destino de carpeta o haz clic en **Examinar** para buscar la carpeta de destino.

4.  Si la carpeta se replica mediante la replicación DFS, puedes especificar si el nuevo destino de carpeta se agrega al grupo de replicación.

> [!TIP]
> Para agregar un destino de carpeta mediante Windows PowerShell, usa el cmdlet [New-DfsnFolderTarget](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnfoldertarget). El módulo de Windows PowerShell para DFSN se introdujo en Windows Server 2012.

> [!NOTE]
> Las carpetas pueden contener destinos de carpeta u otras carpetas DFS, pero no ambos en el mismo nivel de la jerarquía de carpetas.

## <a name="see-also"></a>Vea también

-   [Implementar espacios de nombres DFS](deploying-dfs-namespaces.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [Replicar destinos de carpeta mediante Replicación DFS](replicate-folder-targets-using-dfs-replication.md)