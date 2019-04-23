---
title: Agregar servidores de espacio de nombres a un espacio de nombres DFS basado en dominio
description: En este artículo se describe cómo especificar los servidores de espacio de nombres adicionales para hospedar un espacio de nombres mediante la administración de DFS.
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: bb3b98e1ea687b68bbb87d0da413f9624d336370
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853336"
---
# <a name="add-namespace-servers-to-a-domain-based-dfs-namespace"></a>Agregar servidores de espacio de nombres a un espacio de nombres DFS basado en dominio

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Puedes aumentar la disponibilidad de un espacio de nombres basado en dominio mediante la especificación de servidores de espacio de nombres adicionales para hospedar el espacio de nombres.

## <a name="to-add-a-namespace-server-to-a-domain-based-namespace"></a>Para agregar un servidor de espacio de nombres a un espacio de nombres basado en dominio

Para agregar un servidor de espacio de nombres a un espacio de nombres basado en dominio mediante la administración de DFS, usa el siguiente procedimiento:

1.  Haga clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **Espacios de nombres**, haz clic con el botón derecho en un espacio de nombres basado en dominio y luego haz clic en **Agregar servidor de espacio de nombres**.

3.  Escribe la ruta de acceso a otro servidor o haz clic en **Examinar** para encontrar un servidor.

> [!NOTE]
> Este procedimiento no es aplicable para espacios de nombres independientes, ya que solo admiten un servidor de espacio de nombres único. Para aumentar la disponibilidad de un espacio de nombres independiente, especifica un clúster de conmutación por error como el servidor de espacio de nombres en el Asistente para crear nuevo espacio de nombres.


> [!TIP]
> Para agregar un servidor de espacio de nombres con Windows PowerShell, utiliza el [cmdlet New-DfsnRootTarget](https://docs.microsoft.com/powershell/module/dfsn/set-dfsnroottarget). El módulo de Windows PowerShell para DFSN se introdujo en Windows Server 2012.

## <a name="see-also"></a>Vea también

-   [Implementación de espacios de nombres DFS](deploying-dfs-namespaces.md)
-   [Revise los requisitos del servidor de espacios de nombres DFS](https://technet.microsoft.com/library/cc753448(v=ws.11).aspx)
-   [Crear un Namespace DFS](create-a-dfs-namespace.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)

