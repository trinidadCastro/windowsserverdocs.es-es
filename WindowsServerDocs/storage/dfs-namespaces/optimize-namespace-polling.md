---
title: Optimize Namespace Polling
description: En este artículo se describe cómo optimizar el sondeo de espacio de nombres para mantener un espacio de nombres basado en dominio coherente en todos los servidores de espacio de nombres
ms.date: 6/5/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: cc14dd4f8d6cd833642b87caa32353d4f8940b05
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87936140"
---
# <a name="optimize-namespace-polling"></a>Optimize Namespace Polling

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Para mantener un espacio de nombres basado en el dominio coherente en todos los servidores de espacio de nombres, es necesario que éstos sondeen periódicamente Active Directory Domain Services (AD DS) para obtener los datos de espacio de nombres más recientes.

## <a name="to-optimize-namespace-polling"></a>Para optimizar el sondeo de los espacios de nombres

Utilice el siguiente procedimiento para optimizar el modo en que se realiza el sondeo de espacios de nombres:

1.  Haga clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **espacios de nombres** , haga clic con el botón secundario en un espacio de nombres basado en dominio y, a continuación, haga clic en **propiedades** .

3.  En la pestaña **Opciones avanzadas** , seleccione si desea que el espacio de nombres esté optimizado para la coherencia o la escalabilidad.

    -   Elija **optimizar para coherencia** si hay 16 o menos servidores de espacio de nombres que hospeden el espacio de nombres.
    -   Elija **optimizar para escalabilidad** si hay más de 16 servidores de espacio de nombres. Esto reduce la carga en el emulador del controlador de dominio principal (PDC), pero aumenta el tiempo necesario para que los cambios en el espacio de nombres se repliquen en todos los servidores de espacio de nombres. Hasta que los cambios se repliquen en todos los servidores, es posible que los usuarios tengan una vista incoherente del espacio de nombres.

> [!NOTE]
> Para establecer el modo de sondeo del espacio de nombres mediante Windows PowerShell, use el cmdlet [set-DfsnRoot EnableRootScalability](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771913(v=ws.11)) , que se presentó en windows Server 2012.

## <a name="additional-references"></a>Referencias adicionales

-   [Ajustar espacios de nombres DFS](tuning-dfs-namespaces.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)
