---
title: Lista de comprobación implementar espacios de nombres DFS
description: En este artículo se describe cómo configurar e implementar espacios de nombres DFS.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 389d02ae7269ef48ef77d066db3064759d9253c1
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86961967"
---
# <a name="checklist-deploy-dfs-namespaces"></a>Lista de comprobación: Implementar espacios de nombres DFS

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Los espacios de nombres y Replicación DFS de Sistema de archivos distribuido (DFS) se pueden usar para publicar documentos, software y datos de línea de negocio a los usuarios de toda la organización. Aunque la replicación DFS por sí misma es suficiente para distribuir datos, puede usar los espacios de nombres DFS para configurar el espacio de nombres de modo que una carpeta se hospede en varios servidores, cada uno de ellos con una copia actualizada de la carpeta. De esta manera, aumenta la disponibilidad de los datos y se distribuye la carga de clientes en los servidores.

Al examinar una carpeta en el espacio de nombres, los usuarios no son conscientes de que la carpeta se encuentra hospedada en varios servidores. Si el usuario abre la carpeta, se hace referencia automáticamente a un servidor del sitio para el equipo cliente. Si no hay disponibles servidores del mismo sitio, puede configurar el espacio de nombres para que remita el cliente a un servidor que tenga el costo de conexión más bajo, tal como se define en Active Directory servicios de directorio (AD DS).

Para implementar espacios de nombres DFS, realice las siguientes tareas:

-   Revise los conceptos y los requisitos de los espacios de nombres DFS.
[Overview of DFS Namespaces](dfs-overview.md)
-   [Elegir un tipo de espacio de nombres](choose-a-namespace-type.md)
-   [Crear un espacio de nombres DFS](create-a-dfs-namespace.md)
-   Migre los espacios de nombres basados en dominio existentes a los espacios de nombres basados en dominio de modo Windows Server 2008. [Migrar un espacio de nombres basado en dominio al modo Windows Server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md)
-   Aumente la disponibilidad agregando servidores de espacio de nombres a un espacio de nombres basado en dominio. [Agregar servidores de espacio de nombres a un espacio de nombres DFS basado en dominio](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   Agregue carpetas a un espacio de nombres. [Crear una carpeta en un espacio de nombres DFS](create-a-folder-in-a-dfs-namespace.md)
-   Agregar destinos de carpeta a las carpetas de un espacio de nombres. [Agregar destinos de carpeta](add-folder-targets.md)
-   Replique el contenido entre destinos de carpeta mediante Replicación DFS (opcional). [Replicar destinos de carpeta mediante Replicación DFS](replicate-folder-targets-using-dfs-replication.md)


## <a name="additional-references"></a>Referencias adicionales

-   [Espacios de nombres](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771914(v=ws.11))
-   [Lista de comprobación: Optimizar un nuevo espacio de nombres DFS](checklist-tune-a-dfs-namespace.md)
-   [Replicación](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770278(v=ws.11))
