---
title: Replicar destinos de carpeta mediante la replicación de DFS
description: En este artículo se describe cómo replicar destinos de carpeta mediante Replicación DFS
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 3a4f76e43c2dbf5296afd163055adeafb36a5576
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474402"
---
# <a name="replicate-folder-targets-using-dfs-replication"></a>Replicate folder targets using DFS Replication

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Puede usar la replicación DFS para mantener sincronizado el contenido de los destinos de carpeta, de manera que los usuarios puedan ver los mismos archivos independientemente del destino de carpeta al que haga referencia el equipo cliente.

## <a name="to-replicate-folder-targets-using-dfs-replication"></a>Para replicar destinos de carpeta mediante la replicación DFS

1.  Haga clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **espacios de nombres** , haga clic con el botón secundario en una carpeta que tenga dos o más destinos de carpeta y, a continuación, haga clic en **Replicar carpeta**.

3.  Siga las instrucciones del Asistente para replicación de carpeta.

> [!NOTE]
> Los cambios de configuración no se aplican inmediatamente a todos los miembros excepto cuando se usan los cmdlets [Suspend-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/suspend-dfsreplicationgroup) y [Sync-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/sync-dfsreplicationgroup) . La nueva configuración debe replicarse en todos los controladores de dominio, y cada miembro del grupo de replicación debe sondear su controlador de dominio más cercano para obtener los cambios. La cantidad de tiempo que se tarda depende de la latencia de replicación de los servicios de directorio de Active Directory (AD DS) y el intervalo de sondeo largo (60 minutos) en cada miembro. Para sondear inmediatamente los cambios de configuración, abra una ventana del símbolo del sistema y, a continuación, escriba el siguiente comando una vez para cada miembro del grupo de replicación: <br /> dfsrdiag.exe PollAD /Member:DOMAIN\Server1
<br />
Para ello desde una sesión de Windows PowerShell, use el cmdlet [Update-DfsrConfigurationFromAD](https://technet.microsoft.com/itpro/powershell/windows/dfsr/update-dfsrconfigurationfromad) , que se presentó en windows Server 2012 R2.

## <a name="additional-references"></a>Referencias adicionales

-   [Implementar espacios de nombres DFS](deploying-dfs-namespaces.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [Replicación DFS](../dfs-replication/dfsr-overview.md)