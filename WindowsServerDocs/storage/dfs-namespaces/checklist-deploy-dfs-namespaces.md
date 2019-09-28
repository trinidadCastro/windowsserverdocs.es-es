---
title: 'Lista de comprobación: implementar espacios de nombres DFS'
description: En este artículo se describe cómo configurar e implementar espacios de nombres DFS.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: ab38c41c32ec88285a69fb94e62abc1453ddc3d1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402269"
---
# <a name="checklist-deploy-dfs-namespaces"></a>Lista de comprobación: Implementar espacios de nombres DFS

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Los espacios de nombres y Replicación DFS de Sistema de archivos distribuido (DFS) se pueden usar para publicar documentos, software y datos de línea de negocio a los usuarios de toda la organización. Aunque Replicación DFS solo es suficiente para distribuir los datos, puede usar espacios de nombres DFS para configurar el espacio de nombres de modo que una carpeta esté hospedada en varios servidores, cada uno de los cuales contiene una copia actualizada de la carpeta. Esto aumenta la disponibilidad de los datos y distribuye la carga de clientes en los servidores.

Al examinar una carpeta en el espacio de nombres, los usuarios no son conscientes de que la carpeta está hospedada por varios servidores. Cuando un usuario abre la carpeta, el equipo cliente se remite automáticamente a un servidor en el sitio. Si no hay disponibles servidores del mismo sitio, puede configurar el espacio de nombres para que remita el cliente a un servidor que tenga el costo de conexión más bajo, tal como se define en Active Directory servicios de directorio (AD DS).

Para implementar espacios de nombres DFS, realiza las siguientes tareas:

-   Revisa los conceptos y requisitos de espacios de nombres DFS.
[Información general de los espacios de nombres DFS](dfs-overview.md)
-   [Elegir un tipo de espacio de nombres](choose-a-namespace-type.md)
-   [Crear un espacio de nombres DFS](create-a-dfs-namespace.md) 
-   Migra los espacios de nombres existentes basados en dominio a espacios de nombres basados en dominio en modo Windows Server 2008. [Migrar un espacio de nombres basado en dominio al modo Windows Server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md) 
-   Aumenta la disponibilidad al agregar servidores de espacio de nombres a un espacio de nombres basado en dominio. [Agregar servidores de espacio de nombres a un espacio de nombres DFS basado en dominio](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   Agrega carpetas a un espacio de nombres. [Crear una carpeta en un espacio de nombres DFS](create-a-folder-in-a-dfs-namespace.md)
-   Agrega destinos de carpeta a carpetas de un espacio de nombres. [Agregar destinos de carpeta](add-folder-targets.md)
-   Replica el contenido entre destinos de carpeta mediante la replicación DFS (opcional). [Replicar destinos de carpeta mediante Replicación DFS](replicate-folder-targets-using-dfs-replication.md)


## <a name="see-also"></a>Vea también

-   [Espacios](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [Lista de comprobación: Optimizar un nuevo espacio de nombres DFS](checklist-tune-a-dfs-namespace.md)
-   [Replicación](https://technet.microsoft.com/library/cc770278(v=ws.11).aspx)


