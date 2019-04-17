---
title: Ajustar espacios de nombres DFS
description: "En este artículo se describe cómo ajustar u optimizar espacios de nombres DFS"
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 4614441fc54913ba5a8b547bbf1ad3e8ce7ee69b
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="tuning-dfs-namespaces"></a>Ajustar espacios de nombres DFS

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Después de crear un espacio de nombres y agregar carpetas y destinos, consulta las siguientes secciones para ajustar u optimizar el modo en que el nombre de espacios DFS controla las referencias y sondea Active Directory Domain Services (AD DS) para obtener datos de espacios de nombres actualizados:

-   [Habilitar la enumeración basada en el acceso en un espacio de nombres](enable-access-based-enumeration-on-a-namespace.md)
-   [Habilitar o deshabilitar las referencias y la conmutación por recuperación de clientes](enable-or-disable-referrals-and-client-failback.md)
-   [Cambiar la cantidad de tiempo que los clientes mantienen en la caché las referencias](change-the-amount-of-time-that-clients-cache-referrals.md)
-   [Establecer el método para ordenar destinos en las referencias](set-the-ordering-method-for-targets-in-referrals.md)
-   [Establecer la prioridad de destino para anular el orden de referencias](set-target-priority-to-override-referral-ordering.md)
-   [Optimizar el sondeo de los espacios de nombres](optimize-namespace-polling.md)
-   [Usar permisos heredados con la enumeración basada en el acceso](using-inherited-permissions-with-access-based-enumeration.md)

> [!NOTE]
> Para buscar carpetas o destinos de carpeta, selecciona un espacio de nombres, haz clic en la pestaña **Buscar**, escribe la cadena de búsqueda en el cuadro de texto y luego haz clic en **Buscar**.