---
title: Create a Folder in a DFS Namespace
description: En este artículo se describe cómo crear una carpeta en un espacio de nombres DFS
ms.date: 6/5/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: d854cfcb02288f6262ee380edc0614baf9e29878
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87957703"
---
# <a name="create-a-folder-in-a-dfs-namespace"></a>Create a folder in a DFS namespace

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Puede usar las carpetas para crear niveles adicionales de jerarquía en un espacio de nombres. Además, puede crear carpetas con destinos de carpeta para agregar carpetas compartidas al espacio de nombres. Dado que las carpetas DFS con destinos de carpeta no pueden contener otras carpetas DFS, si desea agregar un nivel de jerarquía al espacio de nombres, no agregue destinos de carpeta a la carpeta.

Use el procedimiento siguiente para crear una carpeta en un espacio de nombres mediante administración de DFS:

## <a name="to-create-a-folder-in-a-dfs-namespace"></a>Para crear una carpeta en un espacio de nombres DFS

1.  Haga clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **espacios de nombres** , haga clic con el botón secundario en un espacio de nombres o una carpeta de un espacio de nombres y, a continuación, haga clic en **nueva carpeta**.

3.  En el cuadro de texto **nombre** , escriba el nombre de la nueva carpeta.

4.  Para agregar uno o más destinos de carpeta a la carpeta, haga clic en **Agregar** y especifique la ruta de acceso UNC (Convención de nomenclatura universal) del destino de carpeta y, a continuación, haga clic en **Aceptar** .


> [!TIP]
> Para crear una carpeta en un espacio de nombres con Windows PowerShell, use el cmdlet [New-DfsnFolder](/powershell/module/dfsn/new-dfsnfolder) . El módulo de Windows PowerShell DFSN se presentó en Windows Server 2012.


## <a name="additional-references"></a>Referencias adicionales

-   [Implementar espacios de nombres DFS](deploying-dfs-namespaces.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)
