---
title: Tuning DFS Namespaces
description: En este artículo se describe cómo optimizar u optimizar los espacios de nombres DFS
ms.date: 6/5/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 348a34e24cf7d22dc376df37607f21f1dceea74a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87939401"
---
# <a name="tuning-dfs-namespaces"></a>Tuning DFS Namespaces

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Una vez que se ha creado un espacio de nombres y se han agregado las carpetas y los destinos, consulte las siguientes secciones para ajustar u optimizar el modo en que el espacio de nombres DFS administra las referencias y sondea Active Directory Domain Services (AD DS) en busca de datos de espacios de nombres actualizados:

-   [Habilitar la enumeración basada en el acceso en un espacio de nombres](enable-access-based-enumeration-on-a-namespace.md)
-   [Habilitar o deshabilitar las referencias y la conmutación por recuperación de clientes](enable-or-disable-referrals-and-client-failback.md)
-   [Cambiar la cantidad de tiempo que los clientes almacenan en caché las referencias](change-the-amount-of-time-that-clients-cache-referrals.md)
-   [Establecer el método para ordenar destinos en las referencias](set-the-ordering-method-for-targets-in-referrals.md)
-   [Establecer la prioridad de destino para anular el orden de referencias](set-target-priority-to-override-referral-ordering.md)
-   [Optimizar el sondeo de los espacios de nombres](optimize-namespace-polling.md)
-   [Usar permisos heredados con enumeración basada en el acceso](using-inherited-permissions-with-access-based-enumeration.md)

> [!NOTE]
> Para buscar carpetas o destinos de carpeta, seleccione un espacio de nombres, haga clic en la ficha **Buscar**, escriba la cadena de búsqueda en el cuadro de texto y, a continuación, haga clic en **Buscar**.