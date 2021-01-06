---
title: Add Folder Targets
description: En este tema se describe cómo agregar destinos de carpeta (rutas de acceso UNC)
ms.author: jgerend
manager: brianlic
ms.topic: article
author: jasongerend
ms.date: 12/10/2020
ms.openlocfilehash: 354047e2882a14d33b43b1bef609120b06202e47
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946951"
---
# <a name="add-folder-targets"></a>Add folder targets

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Un destino de carpeta es la ruta de acceso UNC (convención de nomenclatura universal) de una carpeta compartida o de otro espacio de nombres que se asocia con una carpeta en un espacio de nombres. La adición de varios destinos de carpeta permite aumentar la disponibilidad de la carpeta en el espacio de nombres.

## <a name="to-add-a-folder-target"></a>Para agregar un destino de carpeta

Para agregar un destino de carpeta mediante administración de DFS, utilice el siguiente procedimiento:

1.  Haga clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **espacios de nombres** , haga clic con el botón secundario en una carpeta y, a continuación, haga clic en **Agregar destino de carpeta**.

3.  Escriba la ruta de acceso al destino de la carpeta o haga clic en **examinar** para buscar el destino de la carpeta.

4.  Si la carpeta se replica mediante Replicación DFS, puede especificar si desea agregar el nuevo destino de carpeta al grupo de replicación.

> [!TIP]
> Para agregar un destino de carpeta mediante Windows PowerShell, use el cmdlet [New-DfsnFolderTarget](/powershell/module/dfsn/new-dfsnfoldertarget) . El módulo de Windows PowerShell DFSN se presentó en Windows Server 2012.

> [!NOTE]
> Las carpetas pueden contener destinos de carpeta u otras carpetas DFS, pero no ambos en el mismo nivel de la jerarquía de carpetas.

## <a name="additional-references"></a>Referencias adicionales

-   [Implementar espacios de nombres DFS](deploying-dfs-namespaces.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [Replicar destinos de carpeta mediante Replicación DFS](replicate-folder-targets-using-dfs-replication.md)
