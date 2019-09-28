---
title: Replicar destinos de carpeta mediante la replicación de DFS
description: En este artículo se describe cómo replicar destinos de carpeta mediante la replicación de DFS
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: ad26b8685539869e9302eafac69df19d11778a1b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386134"
---
# <a name="replicate-folder-targets-using-dfs-replication"></a>Replicar destinos de carpeta mediante la replicación de DFS

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Puedes usar la replicación de DFS para sincronizar el contenido de los destinos de carpeta para que los usuarios vean los mismos archivos independientemente del destino de carpeta al que hace referencia el equipo cliente.

## <a name="to-replicate-folder-targets-using-dfs-replication"></a>Para replicar destinos de carpeta mediante la replicación de DFS

1.  Haga clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de la consola, en el nodo **Espacios de nombres**, haz clic con el botón de derecho en una carpeta con dos o varios destinos de carpeta y luego haz clic en **Replicar carpeta**.

3.  Sigue las instrucciones del Asistente para replicación de carpeta.

> [!NOTE]
> Los cambios en la configuración no se aplican inmediatamente a todos los miembros, excepto al usar los cmdlets [Suspend-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/suspend-dfsreplicationgroup) y [Sync-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/sync-dfsreplicationgroup). La nueva configuración debe replicarse en todos los controladores de dominio, y cada miembro del grupo de replicación debe sondear su controlador de dominio más cercano para obtener los cambios. La cantidad de tiempo que se tarda depende de la latencia de replicación de los servicios de directorio de Active Directory (AD DS) y el intervalo de sondeo largo (60 minutos) en cada miembro. Para sondear inmediatamente los cambios en la configuración, abre una ventana del símbolo del sistema y luego escribe el comando siguiente una vez por cada miembro del grupo de replicación: <br /> dfsrdiag.exe PollAD /Member:DOMAIN\Server1
<br />
Para ello desde una sesión de Windows PowerShell, use el cmdlet [Update-DfsrConfigurationFromAD](https://technet.microsoft.com/itpro/powershell/windows/dfsr/update-dfsrconfigurationfromad) , que se presentó en windows Server 2012 R2.

## <a name="see-also"></a>Vea también

-   [Implementar espacios de nombres DFS](deploying-dfs-namespaces.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [Replicación DFS](../dfs-replication/dfsr-overview.md)