---
title: Create a Folder in a DFS Namespace
description: En este artículo se describe cómo crear una carpeta en un espacio de nombres DFS
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 18e0f1ad19e8c6ce2b6dbffe0d25c940c4f8f985
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474282"
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
> Para crear una carpeta en un espacio de nombres con Windows PowerShell, use el cmdlet [New-DfsnFolder](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnfolder) . El módulo de Windows PowerShell DFSN se presentó en Windows Server 2012.


## <a name="additional-references"></a>Referencias adicionales

-   [Implementar espacios de nombres DFS](deploying-dfs-namespaces.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)


