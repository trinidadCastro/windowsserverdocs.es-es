---
title: Crear un espacio de nombres DFS
description: "En este artículo se describe cómo crear un espacio de nombres DFS."
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b01a83f165e0ef01c6413dcf8785435c8f3aca5a
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="create-a-dfs-namespace"></a>Crear un nuevo espacio de nombres DFS

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Para crear un nuevo espacio de nombres, puedes usar el Administrador de servidores para crear el espacio de nombres al instalar el servicio de rol de espacios de nombres DFS. También puedes usar el [cmdlet New-DfsnRoot](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnroot) desde una sesión de Windows PowerShell. 

El módulo de WindowsPowerShell para DFSN se introdujo en WindowsServer2012. 

También puedes usar el siguiente procedimiento para crear un espacio de nombres después de instalar el servicio de rol.

## <a name="to-create-a-namespace"></a>Para crear un espacio de nombres

1.  Haz clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, haz clic con el botón derecho el nodo **Espacios de nombres** y luego haz clic en **Nuevo espacio de nombres**.

3.  Siga las instrucciones del **Asistente para crear nuevo espacio de nombres**.

    Para crear un espacio de nombres independiente en un clúster de conmutación por error, especifica el nombre de una instancia de servidor de archivos agrupados en clúster en la página **Servidor de espacio de nombres** del **Asistente para crear nuevo espacio de nombres**.

> [!IMPORTANT]
> No intentes crear un espacio de nombres basado en dominio con el modo Windows Server 2008, a menos que el nivel funcional del bosque sea Windows Server 2003 o superior. Si lo haces, puedes provocar que un espacio de nombres para el que no puedes eliminar las carpetas DFS, muestre el siguiente mensaje de error: "No se puede eliminar la carpeta. No se puede completar esta función".

## <a name="see-also"></a>Consulta también

-   [Implementar espacios de nombres DFS](deploying-dfs-namespaces.md)
-   [Elegir un tipo de espacio de nombres](choose-a-namespace-type.md)
-   [Agregar servidores de espacio de nombres a un espacio de nombres DFS basado en dominio](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md).


