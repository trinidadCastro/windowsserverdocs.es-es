---
title: Delegar permisos de administración para espacios de nombres DFS
description: En este artículo se describe cómo delegar permisos de administración para espacios de nombres DFS y qué grupos pueden ejecutar tareas de espacio de nombres de forma predeterminada.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 838b8c716618bf10b900c12b118e940e318b56a2
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965877"
---
# <a name="delegate-management-permissions-for-dfs-namespaces"></a>Delegación de permisos de administración para espacios de nombres DFS

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

En la tabla siguiente se describen los grupos que pueden realizar tareas básicas de espacio de nombres de forma predeterminada y el método para delegar la capacidad de realizar estas tareas:

|Tarea | Grupos que pueden realizar esta tarea de forma predeterminada | Método de delegación |
|---|---|---|
|Crear un espacio de nombres basado en dominio|El grupo Admins. del dominio del dominio donde está configurado el espacio de nombres|Haga clic con el botón secundario en el nodo **espacios de nombres** en el árbol de consola y, a continuación, haga clic en **delegar permisos de administración**. O bien [, use set-DfsnRoot GrantAdminAccounts](/powershell/module/dfsn/set-dfsnroot?view=win10-ps) y [set-DfsnRoot RevokeAdminAccounts](/powershell/module/dfsn/set-dfsnroot?view=win10-ps). Cmdlets de Windows PowerShell (introducidos en Windows Server 2012). También debe agregar el usuario al grupo Administradores local en el servidor de espacio de nombres.|
|Agregar un servidor de espacio de nombres a un espacio de nombres basado en dominio|El grupo Admins. del dominio del dominio donde está configurado el espacio de nombres| Haga clic con el botón secundario en el espacio de nombres basado en dominio en el árbol de consola y, a continuación, haga clic en **delegar permisos de administración**. O bien [, use set-DfsnRoot GrantAdminAccounts](/powershell/module/dfsn/set-dfsnroot?view=win10-ps) y [set-DfsnRoot RevokeAdminAccounts](/powershell/module/dfsn/set-dfsnroot?view=win10-ps). Cmdlets de Windows PowerShell (introducidos en Windows Server 2012). También debe agregar el usuario al grupo Administradores local en el servidor de espacio de nombres que se va a agregar.|
|Administrar un espacio de nombres basado en dominio|El grupo Administradores local en cada servidor de espacio de nombres| Haga clic con el botón secundario en el espacio de nombres basado en dominio en el árbol de consola y, a continuación, haga clic en **delegar permisos de administración**. |
|Crear un espacio de nombres independiente|El grupo Administradores local en el servidor de espacio de nombres| Agregue el usuario al grupo Administradores local en el servidor de espacio de nombres. |
|Administrar un espacio de nombres independiente*|El grupo Administradores local en el servidor de espacio de nombres| Haga clic con el botón secundario en el espacio de nombres independiente en el árbol de consola y, a continuación, haga clic en **delegar permisos de administración**. O bien [, use set-DfsnRoot GrantAdminAccounts](/powershell/module/dfsn/set-dfsnroot?view=win10-ps) y [set-DfsnRoot RevokeAdminAccounts](/powershell/module/dfsn/set-dfsnroot?view=win10-ps). Cmdlets de Windows PowerShell (introducidos en Windows Server 2012).|
|Crear un grupo de replicación o habilitar la replicación DFS en una carpeta|El grupo Admins. del dominio del dominio donde está configurado el espacio de nombres| Haga clic con el botón secundario en el nodo Replicación en el árbol de consola y, a continuación, haga clic en **delegar permisos de administración**. |

<br />

\*La delegación de permisos de administración para administrar un espacio de nombres independiente no concede al usuario la posibilidad de ver y administrar la seguridad mediante la pestaña **delegación** a menos que el usuario sea miembro del grupo Administradores local en el servidor de espacio de nombres. Este problema se produce porque el complemento Administración de DFS no puede recuperar las listas de control de acceso discrecional (DACL) para el espacio de nombres independiente desde el Registro. Para habilitar el complemento para que muestre información de delegación, debe seguir los pasos descritos en el artículo de Microsoft<sup>®</sup> Knowledge Base: [KB314837: How to Manage Remote Access to the Registry](https://go.microsoft.com/fwlink?linkid=46803)
