---
title: Ajustar espacios de nombres DFS
description: En este artículo se describe cómo ajustar u optimizar espacios de nombres DFS
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c11bbf65c3baebebe1e5143a5e694ca752500aca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844636"
---
# <a name="tuning-dfs-namespaces"></a>Ajustar espacios de nombres DFS

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Después de crear un espacio de nombres y agregar carpetas y los destinos, consulte las siguientes secciones para ajustar u optimizar la forma Namespace DFS administra las referencias y sondea Active Directory Domain Services (AD DS) para los datos de espacios de nombres actualizados:

-   [Habilitar enumeración basada en acceso en un Namespace](enable-access-based-enumeration-on-a-namespace.md)
-   [Habilitar o deshabilitar referencias y la conmutación por recuperación de cliente](enable-or-disable-referrals-and-client-failback.md)
-   [Cambiar la cantidad de tiempo que los clientes de caché las referencias](change-the-amount-of-time-that-clients-cache-referrals.md)
-   [Establecer el método de ordenación para destinos de las referencias](set-the-ordering-method-for-targets-in-referrals.md)
-   [Establecer la prioridad de destino para invalidar el orden de referencia](set-target-priority-to-override-referral-ordering.md)
-   [Optimizar el sondeo de Namespace](optimize-namespace-polling.md)
-   [Usar permisos heredados con enumeración basada en acceso](using-inherited-permissions-with-access-based-enumeration.md)

> [!NOTE]
> Para buscar carpetas o destinos de carpeta, selecciona un espacio de nombres, haz clic en la pestaña **Buscar**, escribe la cadena de búsqueda en el cuadro de texto y luego haz clic en **Buscar**.