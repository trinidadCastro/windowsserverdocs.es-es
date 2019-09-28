---
title: Ajustar espacios de nombres DFS
description: En este artículo se describe cómo ajustar u optimizar espacios de nombres DFS
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 011512deaeb99ded7d0bfc32a48f19ab3b622475
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386154"
---
# <a name="tuning-dfs-namespaces"></a>Ajustar espacios de nombres DFS

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Después de crear un espacio de nombres y de agregar carpetas y destinos, consulte las secciones siguientes para optimizar u optimizar el modo en que el espacio de nombres DFS administra las referencias y los sondeos Active Directory Domain Services (AD DS) en busca de datos de espacio de nombres actualizados:

-   [Habilitar la enumeración basada en el acceso en un espacio de nombres](enable-access-based-enumeration-on-a-namespace.md)
-   [Habilitar o deshabilitar las referencias y la conmutación por recuperación de clientes](enable-or-disable-referrals-and-client-failback.md)
-   [Cambiar la cantidad de tiempo que los clientes almacenan en caché las referencias](change-the-amount-of-time-that-clients-cache-referrals.md)
-   [Establecer el método para ordenar destinos en las referencias](set-the-ordering-method-for-targets-in-referrals.md)
-   [Establecer la prioridad de destino para anular el orden de referencias](set-target-priority-to-override-referral-ordering.md)
-   [Optimizar el sondeo de los espacios de nombres](optimize-namespace-polling.md)
-   [Usar permisos heredados con enumeración basada en el acceso](using-inherited-permissions-with-access-based-enumeration.md)

> [!NOTE]
> Para buscar carpetas o destinos de carpeta, selecciona un espacio de nombres, haz clic en la pestaña **Buscar**, escribe la cadena de búsqueda en el cuadro de texto y luego haz clic en **Buscar**.