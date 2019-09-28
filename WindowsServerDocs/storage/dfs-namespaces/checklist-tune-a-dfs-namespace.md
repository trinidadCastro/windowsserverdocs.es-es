---
title: 'Lista de comprobación: ajustar un espacio de nombres DFS'
description: En este artículo se describe cómo optimizar el modo en que el espacio de nombres DFS controla las referencias y sondea AD DS para obtener datos de espacios de nombres actualizados
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 8a4c581801cbb1dcfe3db2813fb66fb4514d6434
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402182"
---
# <a name="checklist-tune-a-dfs-namespace"></a>Lista de comprobación: Optimizar un espacio de nombres DFS

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Después de crear un espacio de nombres y de agregar carpetas y destinos, use la siguiente lista de comprobación para optimizar u optimizar la forma en que el espacio de nombres DFS administra las referencias y los sondeos Active Directory Domain Services (AD DS) en busca de datos de espacio de nombres actualizados.

-   Impide que los usuarios vean carpetas de un espacio de nombres al que no tienen permiso para acceder. [Habilitar la enumeración basada en el acceso en un espacio de nombres](enable-access-based-enumeration-on-a-namespace.md) 
-   Permite o impide que los usuarios sean remitidos a un espacio de nombre o carpeta de destino cuando tienen acceso a una carpeta en el espacio de nombres. [Habilitar o deshabilitar las referencias y la conmutación por recuperación de clientes](enable-or-disable-referrals-and-client-failback.md) 
-   Ajusta durante cuánto tiempo los clientes mantienen en la caché las referencias antes de solicitar una nueva. [Cambiar la cantidad de tiempo que los clientes almacenan en caché las referencias](change-the-amount-of-time-that-clients-cache-referrals.md)
-   Optimiza cómo los servidores de espacio de nombres sondean AD DS para obtener los datos del espacio de nombres más recientes. [Optimizar el sondeo de los espacios de nombres](optimize-namespace-polling.md)
-   Usa permisos heredados para controlar qué usuarios pueden ver las carpetas de un espacio de nombres para el que está habilitada la enumeración basada en el acceso. [Usar permisos heredados con enumeración basada en el acceso](using-inherited-permissions-with-access-based-enumeration.md)

Además, mediante el uso de una mejora de los espacios de nombres DFS conocida como prioridad de destino, puede especificar la prioridad de los servidores para que un servidor específico siempre se coloque en primer o último lugar en la lista de servidores (lo que se conoce como referencia) que el cliente recibe cuando tiene acceso a una carpeta. con destinos en el espacio de nombres.

-   Especifica el orden en que los usuarios deben remitirse a destinos de carpeta. [Establecer el método para ordenar destinos en las referencias](set-the-ordering-method-for-targets-in-referrals.md)
-   Anula el orden de referencias para un servidor de espacio de nombres o destino de carpeta específicos. [Establecer la prioridad de destino para anular el orden de referencias](set-target-priority-to-override-referral-ordering.md)

## <a name="see-also"></a>Vea también

-   [Espacios](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [Lista de comprobación: Implementar espacios de nombres DFS](checklist-deploy-dfs-namespaces.md)
-   [Ajustar espacios de nombres DFS](tuning-dfs-namespaces.md)


