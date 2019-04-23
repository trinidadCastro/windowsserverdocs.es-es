---
title: Crear un espacio de nombres DFS
description: En este artículo se describe cómo crear un espacio de nombres DFS.
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 4256e124e75be72f94cbd35c182edfe38e92bc90
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847506"
---
# <a name="create-a-dfs-namespace"></a>Crear un nuevo espacio de nombres DFS

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Para crear un nuevo espacio de nombres, puedes usar el Administrador de servidores para crear el espacio de nombres al instalar el servicio de rol de espacios de nombres DFS. También puedes usar el [cmdlet New-DfsnRoot](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnroot) desde una sesión de Windows PowerShell. 

El módulo de Windows PowerShell para DFSN se introdujo en Windows Server 2012. 

También puedes usar el siguiente procedimiento para crear un espacio de nombres después de instalar el servicio de rol.

## <a name="to-create-a-namespace"></a>Para crear un espacio de nombres

1.  Haga clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, haz clic con el botón derecho el nodo **Espacios de nombres** y luego haz clic en **Nuevo espacio de nombres**.

3.  Siga las instrucciones del **Asistente para crear nuevo espacio de nombres**.

    Para crear un espacio de nombres independiente en un clúster de conmutación por error, especifica el nombre de una instancia de servidor de archivos agrupados en clúster en la página **Servidor de espacio de nombres** del **Asistente para crear nuevo espacio de nombres**.

> [!IMPORTANT]
> No intente crear un espacio de nombres basado en dominio con el modo de Windows Server 2008, a menos que el nivel funcional del bosque es Windows Server 2003 o superior. Si lo hace, puede dar lugar a un espacio de nombres para el que no se puede eliminar carpetas DFS, lo que produce el siguiente mensaje de error: "No se puede eliminar la carpeta. No se puede completar esta función".

## <a name="see-also"></a>Vea también

-   [Implementación de espacios de nombres DFS](deploying-dfs-namespaces.md)
-   [Elija un tipo de Namespace](choose-a-namespace-type.md)
-   [Agregar servidores de Namespace para un Namespace DFS basado en dominio](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md).


