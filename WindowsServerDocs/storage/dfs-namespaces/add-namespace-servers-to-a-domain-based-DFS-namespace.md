---
title: Agregar servidores de espacio de nombres a un espacio de nombres basado en el dominio
description: En este artículo se describe cómo especificar más servidores de espacio de nombres para hospedar un espacio de nombres mediante administración de DFS.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: ee2ae5ddf40c6d8b49b9fbd40505f4e8c70fd92c
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85473502"
---
# <a name="add-namespace-servers-to-a-domain-based-dfs-namespace"></a>Agregar servidores de espacio de nombres a un espacio de nombres DFS basado en dominio

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Puede hacer que aumente la disponibilidad de un espacio de nombre basado en dominio especificando más servidores de espacio de nombres para hospedar el espacio de nombres.

## <a name="to-add-a-namespace-server-to-a-domain-based-namespace"></a>Para agregar un servidor de espacio de nombres a un espacio de nombres basado en dominio

Para agregar un servidor de espacio de nombres a un espacio de nombres basado en dominio mediante administración de DFS, utilice el siguiente procedimiento:

1.  Haga clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **espacios de nombres** , haga clic con el botón secundario en un espacio de nombres basado en dominio y, a continuación, haga clic en **Agregar servidor de espacio de nombres**.

3.  Escriba la ruta de acceso a otro servidor o haga clic en **examinar** para buscar un servidor.

> [!NOTE]
> Este procedimiento no es aplicable a los espacios de nombres independientes porque solo admiten un único servidor de espacio de nombres. Para aumentar la disponibilidad de un espacio de nombres independiente, especifique un clúster de conmutación por error como el servidor de espacio de nombres en el Asistente para crear nuevo espacio de nombres.


> [!TIP]
> Para agregar un servidor de espacio de nombres mediante Windows PowerShell, use el [cmdlet New-DfsnRootTarget](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnroottarget). El módulo de Windows PowerShell DFSN se presentó en Windows Server 2012.

## <a name="additional-references"></a>Referencias adicionales

-   [Implementar espacios de nombres DFS](deploying-dfs-namespaces.md)
-   [Review DFS Namespaces Server Requirements](https://technet.microsoft.com/library/cc753448(v=ws.11).aspx)
-   [Crear un espacio de nombres DFS](create-a-dfs-namespace.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)

