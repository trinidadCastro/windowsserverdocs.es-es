---
title: "Lista de comprobación: ajustar un espacio de nombres DFS"
description: "En este artículo se describe cómo optimizar el modo en que el espacio de nombres DFS controla las referencias y sondea AD DS para obtener datos de espacios de nombres actualizados"
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 827484c2ffcbb2617626129011a8b510473fe669
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="checklist-tune-a-dfs-namespace"></a>Lista de comprobación: Ajustar un espacio de nombre DFS

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Después de crear un espacio de nombres y agregar carpetas y destinos, usa la siguiente lista de comprobación para ajustar u optimizar el modo en que el nombre de espacios DFS controla las referencias y sondea Active Directory Domain Services (AD DS) para obtener datos de espacios de nombres actualizados.

-   Impide que los usuarios vean carpetas de un espacio de nombres al que no tienen permiso para acceder. [Habilitar la enumeración basada en el acceso en un espacio de nombres](enable-access-based-enumeration-on-a-namespace.md) 
-   Permite o impide que los usuarios sean remitidos a un espacio de nombre o carpeta de destino cuando tienen acceso a una carpeta en el espacio de nombres. [Habilitar o deshabilitar las referencias y la conmutación por recuperación de clientes](enable-or-disable-referrals-and-client-failback.md) 
-   Ajusta durante cuánto tiempo los clientes mantienen en la caché las referencias antes de solicitar una nueva. [Cambiar la cantidad de tiempo que los clientes mantienen en la caché las referencias](change-the-amount-of-time-that-clients-cache-referrals.md)
-   Optimiza cómo los servidores de espacio de nombres sondean AD DS para obtener los datos del espacio de nombres más recientes. [Optimizar el sondeo de los espacios de nombres](optimize-namespace-polling.md)
-   Usa permisos heredados para controlar qué usuarios pueden ver las carpetas de un espacio de nombres para el que está habilitada la enumeración basada en el acceso. [Usar permisos heredados con la enumeración basada en el acceso](using-inherited-permissions-with-access-based-enumeration.md)

Además, mediante el uso de una mejora de los espacios de nombres DFS que se conoce como prioridad de destino, puedes especificar la prioridad de servidores para que un servidor determinado siempre esté colocado en primer o último lugar en la lista de servidores (que se conoce como referencia) que el cliente recibe cuando esta accede a una carpeta con destinos en el espacio de nombres.

-   Especifica el orden en que los usuarios deben remitirse a destinos de carpeta. [Establecer el método para ordenar destinos en las referencias](set-the-ordering-method-for-targets-in-referrals.md)
-   Anula el orden de referencias para un servidor de espacio de nombres o destino de carpeta específicos. [Establecer la prioridad de destino para anular el orden de referencias](set-target-priority-to-override-referral-ordering.md)

## <a name="see-also"></a>Consulta también

-   [Espacios de nombres](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [Lista de comprobación: implementar espacios de nombres DFS](checklist-deploy-dfs-namespaces.md)
-   [Ajustar espacios de nombres DFS](tuning-dfs-namespaces.md)


