---
title: Delegar permisos de administración para espacios de nombres DFS
description: En este artículo se describe cómo delegar permisos de administración para espacios de nombres DFS y qué grupos pueden ejecutar tareas de espacio de nombres de manera predeterminada
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 7895432ca16dd13c6425d966f99104fc03db100d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829496"
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

\*Delegar permisos de administración para administrar un espacio de nombres independiente no concede al usuario la capacidad de ver y administrar la seguridad mediante el uso de la **delegación** pestaña a menos que el usuario es miembro del grupo Administradores local en el servidor de espacio de nombres. Este problema se produce porque el complemento de administración DFS no puede recuperar las listas de control de acceso discrecional (DACL) para el espacio de nombres independiente desde el registro. Para habilitar el complemento mostrar información sobre la delegación, debe seguir los pasos descritos en el Microsoft<sup>®</sup> artículo de Knowledge Base: [KB314837: Cómo administrar el acceso remoto en el registro](https://go.microsoft.com/fwlink?linkid=46803)