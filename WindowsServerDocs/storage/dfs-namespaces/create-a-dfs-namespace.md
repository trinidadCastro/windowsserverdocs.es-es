---
title: Create a DFS Namespace
description: En este artículo se describe cómo crear un espacio de nombres DFS.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 60b31b90ed54137898043e79c3c6504afb3f4b7f
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86953367"
---
# <a name="create-a-dfs-namespace"></a>Create a DFS namespace

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Para crear un nuevo espacio de nombres, puede usar Administrador del servidor para crear el espacio de nombres al instalar el servicio de rol espacios de nombres DFS. También puede usar el [cmdlet New-DfsnRoot](/powershell/module/dfsn/new-dfsnroot) desde una sesión de Windows PowerShell.

El módulo de Windows PowerShell DFSN se presentó en Windows Server 2012.

Alernatively, puede utilizar el procedimiento siguiente para crear un espacio de nombres después de instalar el servicio de rol.

## <a name="to-create-a-namespace"></a>Para crear un espacio de nombres

1.  Haga clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, haga clic con el botón secundario en el nodo **espacios de nombres** y, a continuación, haga clic en **nuevo espacio de nombres**.

3.  Siga las instrucciones del **Asistente para nuevo espacio de nombres**.

    Para crear un espacio de nombres independiente en un clúster de conmutación por error, especifique el nombre de una instancia de servidor de archivos en clúster en la página **servidor de espacio** de nombres del **Asistente para nuevo espacio de nombres** .

> [!IMPORTANT]
> No intente crear un espacio de nombres basado en dominio mediante el modo Windows Server 2008 a menos que el nivel funcional del bosque sea Windows Server 2003 o posterior. Si lo hace, se puede producir un espacio de nombres para el que no se pueden eliminar carpetas DFS, lo que genera el siguiente mensaje de error: "no se puede eliminar la carpeta. No se puede completar esta función ".

## <a name="additional-references"></a>Referencias adicionales

-   [Implementar espacios de nombres DFS](deploying-dfs-namespaces.md)
-   [Elegir un tipo de espacio de nombres](choose-a-namespace-type.md)
-   [Agregar servidores de espacio de nombres a un espacio de nombres DFS basado en dominio](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   [Delegue permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md).
