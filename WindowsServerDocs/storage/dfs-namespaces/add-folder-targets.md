---
title: Add Folder Targets
description: En este tema se describe cómo agregar destinos de carpeta (rutas de acceso UNC)
ms.author: jgerend
manager: brianlic
ms.topic: article
author: jasongerend
ms-date: 06/05/2017
ms.openlocfilehash: bc229360a616e7fa56231e6211b4b909d45ec2de
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87957823"
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
