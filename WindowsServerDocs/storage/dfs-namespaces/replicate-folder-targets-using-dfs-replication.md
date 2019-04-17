---
title: "Replicar destinos de carpeta mediante la replicación de DFS"
description: "En este artículo se describe cómo replicar destinos de carpeta mediante la replicación de DFS"
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: ef13c338d8b13c24a02efb0468f06d5fef803cea
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="replicate-folder-targets-using-dfs-replication"></a>Replicar destinos de carpeta mediante la replicación de DFS

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Puedes usar la replicación de DFS para sincronizar el contenido de los destinos de carpeta para que los usuarios vean los mismos archivos independientemente del destino de carpeta al que hace referencia el equipo cliente.

## <a name="to-replicate-folder-targets-using-dfs-replication"></a>Para replicar destinos de carpeta mediante la replicación de DFS

1.  Haz clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de la consola, en el nodo **Espacios de nombres**, haz clic con el botón de derecho en una carpeta con dos o varios destinos de carpeta y luego haz clic en **Replicar carpeta**.

3.  Sigue las instrucciones del Asistente para replicación de carpeta.

> [!NOTE]
> Los cambios en la configuración no se aplican inmediatamente a todos los miembros, excepto al usar los cmdlets [Suspend-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/suspend-dfsreplicationgroup) y [Sync-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/sync-dfsreplicationgroup). La nueva configuración debe replicarse en todos los controladores de dominio, y cada miembro del grupo de replicación debe sondear su controlador de dominio más cercano para obtener los cambios. El tiempo requerido para esto depende de la latencia de replicación de Active Directory Directory Services (ADDS) y el intervalo de sondeo largo (60 minutos) en cada miembro. Para sondear inmediatamente los cambios en la configuración, abre una ventana del símbolo del sistema y luego escribe el comando siguiente una vez por cada miembro del grupo de replicación: <br /> dfsrdiag.exe PollAD /Member:DOMAIN\Server1
<br />
Para hacer esto desde una sesión de Windows PowerShell, usa el cmdlet [Update-DfsrConfigurationFromAD](https://technet.microsoft.com/itpro/powershell/windows/dfsr/update-dfsrconfigurationfromad), que se incluyó en Windows Server 2012 R2.

## <a name="see-also"></a>Consulta también

-   [Implementar espacios de nombres DFS](deploying-dfs-namespaces.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [Replicación](https://technet.microsoft.com/library/cc770278(v=ws.11).aspx)