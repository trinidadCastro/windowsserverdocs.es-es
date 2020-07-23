---
title: Lista de comprobación optimizar un espacio de nombres DFS
description: En este artículo se describe cómo optimizar el modo en que el espacio de nombres DFS administra las referencias y los sondeos AD DS para los datos de espacio de nombres actualizados.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 67e272657c23926adbbf9f0db5174d00f4852137
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86961757"
---
# <a name="checklist-tune-a-dfs-namespace"></a>Lista de comprobación: optimizar un espacio de nombres DFS

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Después de crear un espacio de nombres y de agregar carpetas y destinos, use la siguiente lista de comprobación para optimizar u optimizar la forma en que el espacio de nombres DFS administra las referencias y los sondeos Active Directory Domain Services (AD DS) en busca de datos de espacio de nombres actualizados.

-   Impedir que los usuarios vean carpetas en un espacio de nombres para el que no tienen permisos de acceso. [Habilitar la enumeración basada en el acceso en un espacio de nombres](enable-access-based-enumeration-on-a-namespace.md)
-   Habilitar o impedir que los usuarios se hagan referencia a un espacio de nombres o a una carpeta de destino cuando tienen acceso a una carpeta en el espacio de nombres. [Habilitar o deshabilitar las referencias y la conmutación por recuperación de clientes](enable-or-disable-referrals-and-client-failback.md)
-   Ajuste cuánto tiempo los clientes almacenan en caché una referencia antes de solicitar una nueva. [Cambiar la cantidad de tiempo que los clientes almacenan en caché las referencias](change-the-amount-of-time-that-clients-cache-referrals.md)
-   Optimice el modo en que los servidores de espacio de nombres sondean AD DS para obtener los datos de espacio de nombres más recientes. [Optimizar el sondeo de los espacios de nombres](optimize-namespace-polling.md)
-   Use permisos heredados para controlar qué usuarios pueden ver las carpetas de un espacio de nombres para el que está habilitada la enumeración basada en el acceso. [Usar permisos heredados con enumeración basada en el acceso](using-inherited-permissions-with-access-based-enumeration.md)

Además, al usar la mejora de espacios de nombres DFS llamada prioridad de destino, puede especificar la prioridad de los servidores de manera que un servidor específico siempre se encuentre en primer o último lugar en la lista de servidores (lo que se conoce como referencia) que el cliente recibe al tener acceso a una carpeta con destinos del espacio de nombres.

-   Especifique en qué orden se deben hacer referencia a los usuarios de los destinos de carpeta. [Establecer el método para ordenar destinos en las referencias](set-the-ordering-method-for-targets-in-referrals.md)
-   Invalide el orden de referencia para un servidor de espacio de nombres o destino de carpeta específicos. [Establecer la prioridad de destino para anular el orden de referencias](set-target-priority-to-override-referral-ordering.md)

## <a name="additional-references"></a>Referencias adicionales

-   [Espacios de nombres](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771914(v=ws.11))
-   [Lista de comprobación: Implementar espacios de nombres DFS](checklist-deploy-dfs-namespaces.md)
-   [Ajustar espacios de nombres DFS](tuning-dfs-namespaces.md)
