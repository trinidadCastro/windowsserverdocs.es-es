---
title: Delegar permisos de administración para espacios de nombres DFS
description: En este artículo se describe cómo delegar permisos de administración para espacios de nombres DFS y qué grupos pueden ejecutar tareas de espacio de nombres de manera predeterminada
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 5bf23498c95d4b44d5c17aecd216921dc70819a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402221"
---
# <a name="delegate-management-permissions-for-dfs-namespaces"></a>Delegar permisos de administración para espacios de nombres DFS

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

En la siguiente tabla se describen los grupos que pueden realizar tareas básicas de espacio de nombres de forma predeterminada y el método para delegar la capacidad para realizar estas tareas:

|Tarea | Grupos que pueden realizar esta tarea de manera predeterminada | Método de delegación |
|---|---|---|
|Crear un espacio de nombres basado en dominio|Grupo de administradores de dominio en el dominio donde se ha configurado el espacio de nombres|Haz clic con el botón derecho en el nodo **Espacios de nombres** en el árbol de consola y luego haz clic en **Delegar permisos de administración**. O usa [Set-DfsnRoot GrantAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot) y [Set-DfsnRoot RevokeAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot). Cmdlets de Windows PowerShell (que se introdujeron en Windows Server 2012). También debe agregar el usuario al grupo Administradores local en el servidor de espacio de nombres.|
|Agregar un servidor de espacio de nombres a un espacio de nombres basado en dominio|Grupo de administradores de dominio en el dominio donde se ha configurado el espacio de nombres| Haz clic con el botón derecho en el espacio de nombres basado en dominio en el árbol de consola y luego haz clic en **Delegar permisos de administración**. O usa [Set-DfsnRoot GrantAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot) y [Set-DfsnRoot RevokeAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot). Cmdlets de Windows PowerShell (que se introdujeron en Windows Server 2012). También debes agregar el usuario al grupo Administradores local en el servidor de espacio de nombres que se agregará.|
|Administrar un espacio de nombres basado en dominio|Grupo Administradores local en cada servidor de espacio de nombres| Haz clic con el botón derecho en el espacio de nombres basado en dominio en el árbol de consola y luego haz clic en **Delegar permisos de administración**. |
|Crear un espacio de nombres independiente|Grupo Administradores local en el servidor de espacio de nombres| Agrega el usuario al grupo Administradores local en el servidor de espacio de nombres. |
|Administrar un espacio de nombres independiente*|Grupo Administradores local en el servidor de espacio de nombres| Haz clic con el botón derecho en el espacio de nombres independiente en el árbol de consola y luego haz clic en **Delegar permisos de administración**. O usa [Set-DfsnRoot GrantAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot) y [Set-DfsnRoot RevokeAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot). Cmdlets de Windows PowerShell (que se introdujeron en Windows Server 2012).|
|Crear un grupo de replicación o habilitar la replicación DFS en una carpeta|Grupo de administradores de dominio en el dominio donde se ha configurado el espacio de nombres| Haz clic con el botón derecho en el nodo Replicación en el árbol de consola y luego haz clic en **Delegar permisos de administración**. |

<br />

@no__t permisos de administración de 0Delegating para administrar un espacio de nombres independiente no concede al usuario la posibilidad de ver y administrar la seguridad mediante la pestaña **delegación** a menos que el usuario sea miembro del grupo Administradores local en el servidor de espacio de nombres. Este problema se produce porque el complemento de administración DFS no puede recuperar las listas de control de acceso discrecional (DACL) para el espacio de nombres independiente desde el registro. Para habilitar el complemento para que muestre información de delegación, debe seguir los pasos descritos en el artículo de Microsoft<sup>®</sup> Knowledge Base: @NO__T 0KB314837: Cómo administrar el acceso remoto al registro @ no__t-0