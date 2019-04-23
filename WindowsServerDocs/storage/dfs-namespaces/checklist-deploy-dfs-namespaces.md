---
title: 'Lista de comprobación: implementar espacios de nombres DFS'
description: En este artículo se describe cómo configurar e implementar espacios de nombres DFS.
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 7f7cca6b67ff6fa8d81e88323381866315f07f33
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842126"
---
# <a name="checklist-deploy-dfs-namespaces"></a>Lista de comprobación: Implementar espacios de nombres DFS

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Espacios de nombres de sistema de archivos (DFS) distribuido y replicación DFS pueden utilizarse para publicar documentos, software y datos de línea de negocio para los usuarios en toda la organización. Aunque la replicación DFS por sí sola es suficiente para distribuir los datos, puede usar espacios de nombres DFS para configurar el espacio de nombres para que una carpeta se hospede en varios servidores, cada uno de los cuales contiene una copia actualizada de la carpeta. Esto aumenta la disponibilidad de los datos y distribuye la carga de clientes en los servidores.

Al examinar una carpeta en el espacio de nombres, los usuarios no son conscientes de que la carpeta está hospedada por varios servidores. Cuando un usuario abre la carpeta, el equipo cliente se remite automáticamente a un servidor en el sitio. Si hay disponible ningún servidor de mismo sitio, puede configurar el espacio de nombres para hacer referencia al cliente a un servidor que tenga el menor costo como se define en los servicios de directorio de Active Directory (AD DS) de conexión.

Para implementar espacios de nombres DFS, realiza las siguientes tareas:

-   Revisa los conceptos y requisitos de espacios de nombres DFS.
[Información general de espacios de nombres DFS](dfs-overview.md)
-   [Elija un tipo de espacio de nombres](choose-a-namespace-type.md)
-   [Crear un espacio de nombres DFS](create-a-dfs-namespace.md) 
-   Migra los espacios de nombres existentes basados en dominio a espacios de nombres basados en dominio en modo Windows Server 2008. [Migrar un Namespace basado en dominio al modo Windows Server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md) 
-   Aumenta la disponibilidad al agregar servidores de espacio de nombres a un espacio de nombres basado en dominio. [Agregar servidores de Namespace para un Namespace DFS basado en dominio](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   Agrega carpetas a un espacio de nombres. [Cree una carpeta en un Namespace DFS](create-a-folder-in-a-dfs-namespace.md)
-   Agrega destinos de carpeta a carpetas de un espacio de nombres. [Agregar destinos de carpeta](add-folder-targets.md)
-   Replica el contenido entre destinos de carpeta mediante la replicación DFS (opcional). [Replicar destinos de carpeta mediante la replicación DFS](replicate-folder-targets-using-dfs-replication.md)


## <a name="see-also"></a>Vea también

-   [espacios de nombres](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [Lista de comprobación: Optimizar un Namespace DFS](checklist-tune-a-dfs-namespace.md)
-   [Replicación](https://technet.microsoft.com/library/cc770278(v=ws.11).aspx)


