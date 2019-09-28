---
title: Optimizar el sondeo de los espacios de nombres
description: En este artículo se describe cómo optimizar el sondeo de espacios de nombres para mantener un espacio de nombres coherente basado en dominio en servidores de espacio de nombres
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 8b5a80324851674c8d980bc1b6cda3a5ea51483f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402159"
---
# <a name="optimize-namespace-polling"></a>Optimizar el sondeo de los espacios de nombres

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Para mantener un espacio de nombres basado en el dominio coherente en todos los servidores de espacio de nombres, es necesario que éstos sondeen periódicamente Active Directory Domain Services (AD DS) para obtener los datos de espacio de nombres más recientes. 

## <a name="to-optimize-namespace-polling"></a>Para optimizar el sondeo de los espacios de nombres

Usa el siguiente procedimiento para optimizar el modo en que se produce el sonde de espacios de nombres:

1.  Haga clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **Espacios de nombres**, haz clic con el botón derecho en un espacio de nombres basado en el dominio y luego haz clic en **Propiedades**.

3.  En la pestaña **Opciones avanzadas**, selecciona si deseas que el espacio de nombres se optimice para lograr una mayor coherencia o escalabilidad.

    -   Elige **Optimizar para coherencia** si hay 16 o menos servidores de espacio de nombres que hospeden el espacio de nombres.
    -   Elige **Optimizar para escalabilidad** si hay más de 16 servidores de espacio de nombres. Esto reduce la carga del emulador del controlador de dominio principal (PDC), pero aumenta el tiempo necesario para realizar cambios en el espacio de nombres para replicar en todos los servidores de espacio de nombres. Hasta que los cambios se repliquen en todos los servidores, es posible que los usuarios tengan una vista incoherente del espacio de nombres.

> [!NOTE]
> Para establecer el modo de sondeo del espacio de nombres mediante Windows PowerShell, use el cmdlet [set-DfsnRoot EnableRootScalability](https://technet.microsoft.com/library/jj884281.aspx) , que se presentó en windows Server 2012.

## <a name="see-also"></a>Vea también

-   [Ajustar espacios de nombres DFS](tuning-dfs-namespaces.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)