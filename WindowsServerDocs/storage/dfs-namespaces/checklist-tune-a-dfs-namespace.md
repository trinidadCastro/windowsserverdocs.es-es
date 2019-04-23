---
title: 'Lista de comprobación: ajustar un espacio de nombres DFS'
description: En este artículo se describe cómo optimizar el modo en que el espacio de nombres DFS controla las referencias y sondea AD DS para obtener datos de espacios de nombres actualizados
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 5e2d43f75a64a6a7950539c14386c6a037bc3c45
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872646"
---
# <a name="checklist-tune-a-dfs-namespace"></a>Lista de comprobación: Ajustar un espacio de nombres DFS

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Después de crear un espacio de nombres y agregar carpetas y los destinos, utilice la siguiente lista de comprobación para ajustar u optimizar la forma en el espacio de nombres DFS administra las referencias y sondea Active Directory Domain Services (AD DS) para los datos de espacios de nombres actualizados.

-   Impide que los usuarios vean carpetas de un espacio de nombres al que no tienen permiso para acceder. [Habilitar enumeración basada en acceso en un Namespace](enable-access-based-enumeration-on-a-namespace.md) 
-   Permite o impide que los usuarios sean remitidos a un espacio de nombre o carpeta de destino cuando tienen acceso a una carpeta en el espacio de nombres. [Habilitar o deshabilitar referencias y la conmutación por recuperación de cliente](enable-or-disable-referrals-and-client-failback.md) 
-   Ajusta durante cuánto tiempo los clientes mantienen en la caché las referencias antes de solicitar una nueva. [Cambiar la cantidad de tiempo que los clientes de caché las referencias](change-the-amount-of-time-that-clients-cache-referrals.md)
-   Optimiza cómo los servidores de espacio de nombres sondean AD DS para obtener los datos del espacio de nombres más recientes. [Optimizar el sondeo de Namespace](optimize-namespace-polling.md)
-   Usa permisos heredados para controlar qué usuarios pueden ver las carpetas de un espacio de nombres para el que está habilitada la enumeración basada en el acceso. [Usar permisos heredados con enumeración basada en acceso](using-inherited-permissions-with-access-based-enumeration.md)

Además, mediante el uso de una mejora de espacios de nombres DFS que se conoce como la prioridad de destino, puede especificar la prioridad de los servidores para que un servidor específico se coloca siempre primero o último lugar en la lista de servidores (que se conoce como referencia) que el cliente recibe cuando tiene acceso a una carpeta con los destinos en el espacio de nombres.

-   Especifica el orden en que los usuarios deben remitirse a destinos de carpeta. [Establecer el método de ordenación para destinos de las referencias](set-the-ordering-method-for-targets-in-referrals.md)
-   Anula el orden de referencias para un servidor de espacio de nombres o destino de carpeta específicos. [Establecer la prioridad de destino para invalidar el orden de referencia](set-target-priority-to-override-referral-ordering.md)

## <a name="see-also"></a>Vea también

-   [espacios de nombres](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [Lista de comprobación: Implementar espacios de nombres DFS](checklist-deploy-dfs-namespaces.md)
-   [Optimización de espacios de nombres DFS](tuning-dfs-namespaces.md)


