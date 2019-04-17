---
title: "Lista de comprobación: implementar espacios de nombres DFS"
description: "En este artículo se describe cómo configurar e implementar espacios de nombres DFS."
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 8f1dd7a8a44f5427d6464a6be2057a529638eca7
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="checklist-deploy-dfs-namespaces"></a>Lista de comprobación: implementar espacios de nombres DFS

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Los espacios de nombres del sistema de archivos distribuido (DFS) y la replicación DFS pueden usarse para publicar documentos, software y datos de línea de negocio para usuarios de toda una organización. Aunque la replicación DFS solo es suficiente para distribuir los datos, puedes usar espacios de nombres DFS para configurar el espacio de nombres, de modo que varios servidores hospedan una carpeta y cada uno de ellos contiene una copia actualizada de dicha carpeta. Esto aumenta la disponibilidad de los datos y distribuye la carga de clientes en los servidores.

Al examinar una carpeta en el espacio de nombres, los usuarios no son conscientes de que la carpeta está hospedada por varios servidores. Cuando un usuario abre la carpeta, el equipo cliente se remite automáticamente a un servidor en el sitio. Si no hay servidores en el mismo sitio disponibles, puedes configurar el espacio de nombres para remitir al cliente a un servidor que tiene el menor coste de conexión, tal y como se define en Active Directory Directory Services (AD DS).

Para implementar espacios de nombres DFS, realiza las siguientes tareas:

-   Revisa los conceptos y requisitos de espacios de nombres DFS.
[Información general de los espacios de nombres DFS](dfs-overview.md)
-   [Elegir un tipo de espacio de nombres](choose-a-namespace-type.md)
-   [Crear un nuevo espacio de nombres DFS](create-a-dfs-namespace.md) 
-   Migra los espacios de nombres existentes basados en dominio a espacios de nombres basados en dominio en modo Windows Server 2008. [Migrar un espacio de nombres basado en dominio al modo Windows Server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md) 
-   Aumenta la disponibilidad al agregar servidores de espacio de nombres a un espacio de nombres basado en dominio. [Agregar servidores de espacio de nombres a un espacio de nombres DFS basado en dominio](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   Agrega carpetas a un espacio de nombres. [Crear una carpeta en un espacio de nombres DFS](create-a-folder-in-a-dfs-namespace.md)
-   Agrega destinos de carpeta a carpetas de un espacio de nombres. [Agregar destinos de carpeta](add-folder-targets.md)
-   Replica el contenido entre destinos de carpeta mediante la replicación DFS (opcional). [Replicar destinos de carpeta mediante la replicación DFS](replicate-folder-targets-using-dfs-replication.md)


## <a name="see-also"></a>Consulta también

-   [Espacios de nombres](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [Lista de comprobación: Ajustar un espacio de nombre DFS](checklist-tune-a-dfs-namespace.md)
-   [Replicación](https://technet.microsoft.com/library/cc770278(v=ws.11).aspx)


