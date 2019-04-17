---
title: Crear una carpeta en un espacio de nombres DFS
description: "En este artículo se describe cómo crear una carpeta en un espacio de nombres DFS"
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 1ca9fa0b87c6e995f3f0c38abec80fef9068df90
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="create-a-folder-in-a-dfs-namespace"></a>Crear una carpeta en un espacio de nombres DFS

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Puedes usar carpetas para crear niveles de jerarquía adicionales en un espacio de nombres. También puedes crear carpetas con destinos de carpeta para agregar carpetas compartidas al espacio de nombres. Las carpetas DFS con destinos de carpeta no pueden incluir otras carpetas DFS, por lo tanto, si quieres agregar un nivel de jerarquía al espacio de nombres, no agregues destinos de carpeta a la carpeta.

Usa el siguiente procedimiento para crear una carpeta en un espacio de nombres mediante la administración DFS:

## <a name="to-create-a-folder-in-a-dfs-namespace"></a>Para crear una carpeta en un espacio de nombres DFS

1.  Haz clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **Espacios de nombres**, haz clic con el botón derecho en un espacio de nombres o en una carpeta de un espacio de nombres y luego haz clic en **Nueva carpeta**.

3.  En el cuadro de texto **Nombre**, escribe el nombre de la nueva carpeta.

4.  Para agregar uno o varios destinos de carpeta a la carpeta, haz clic en **Agregar** y especifica la ruta de acceso de convención de nomenclatura Universal (UNC) del destino de carpeta y luego haz clic en **Aceptar**.


> [!TIP]
> Para crear una carpeta en un espacio de nombres mediante Windows PowerShell, usa el cmdlet [New-DfsnFolder](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnfolder). El módulo de WindowsPowerShell para DFSN se introdujo en WindowsServer2012.


## <a name="see-also"></a>Consulta también

-   [Implementar espacios de nombres DFS](deploying-dfs-namespaces.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)


