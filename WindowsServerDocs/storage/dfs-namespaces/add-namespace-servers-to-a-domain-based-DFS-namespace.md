---
title: Agregar servidores de espacio de nombres a un espacio de nombres DFS basado en dominio
description: "En este artículo se describe cómo especificar los servidores de espacio de nombres adicionales para hospedar un espacio de nombres mediante la administración de DFS."
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 70ab3cac71f5766bc572015c6b23c0937e5252f0
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="add-namespace-servers-to-a-domain-based-dfs-namespace"></a>Agregar servidores de espacio de nombres a un espacio de nombres DFS basado en dominio

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Puedes aumentar la disponibilidad de un espacio de nombres basado en dominio mediante la especificación de servidores de espacio de nombres adicionales para hospedar el espacio de nombres.

## <a name="to-add-a-namespace-server-to-a-domain-based-namespace"></a>Para agregar un servidor de espacio de nombres a un espacio de nombres basado en dominio

Para agregar un servidor de espacio de nombres a un espacio de nombres basado en dominio mediante la administración de DFS, usa el siguiente procedimiento:

1.  Haz clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **Espacios de nombres**, haz clic con el botón derecho en un espacio de nombres basado en dominio y luego haz clic en **Agregar servidor de espacio de nombres**.

3.  Escribe la ruta de acceso a otro servidor o haz clic en **Examinar** para encontrar un servidor.

> [!NOTE]
> Este procedimiento no es aplicable para espacios de nombres independientes, ya que solo admiten un servidor de espacio de nombres único. Para aumentar la disponibilidad de un espacio de nombres independiente, especifica un clúster de conmutación por error como el servidor de espacio de nombres en el Asistente para crear nuevo espacio de nombres.


> [!TIP]
> Para agregar un servidor de espacio de nombres con WindowsPowerShell, utiliza el [cmdlet New-DfsnRootTarget](https://docs.microsoft.com/powershell/module/dfsn/set-dfsnroottarget). El módulo de WindowsPowerShell para DFSN se introdujo en WindowsServer2012.

## <a name="see-also"></a>Consulta también

-   [Implementar espacios de nombres DFS](deploying-dfs-namespaces.md)
-   [Revisar los requisitos del servidor de espacios de nombres DFS](https://technet.microsoft.com/library/cc753448(v=ws.11).aspx)
-   [Crear un nuevo espacio de nombres DFS](create-a-dfs-namespace.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)

