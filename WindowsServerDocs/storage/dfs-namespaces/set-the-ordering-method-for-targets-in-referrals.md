---
title: Establecer el método para ordenar destinos en las referencias
description: En este artículo se describe cómo establecer el método de ordenación para destinos en referencias.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 9b420e311c98477d369c81f10eca274e665dae3a
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475162"
---
# <a name="set-the-ordering-method-for-targets-in-referrals"></a>Establecer el método para ordenar destinos en las referencias

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Una referencia es una lista ordenada de destinos que un equipo cliente recibe de un controlador de dominio o un servidor de espacio de nombres cuando el usuario tiene acceso a una carpeta o raíz de espacio de nombres con destinos. Una vez que el cliente recibe la referencia, intenta tener acceso al primer destino de la lista. Si el destino no está disponible, el cliente intenta tener acceso al siguiente destino.
Los destinos del sitio del cliente siempre aparecen en primer lugar en una referencia. Los destinos que se encuentren fuera del sitio del cliente se mostrarán según el método de ordenación.

Use las secciones siguientes para especificar en qué destinos de orden se debe hacer referencia a los clientes y para entender los distintos métodos de ordenación de las referencias de destino:

## <a name="to-set-the-ordering-method-for-targets-in-namespace-root-referrals"></a>Para establecer el método de ordenación para destinos de referencias a la raíz de espacio de nombres

Use el procedimiento siguiente para establecer el método de ordenación en la raíz del espacio de nombres:

1.  Haga clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **Espacios de nombres**, haga clic con el botón secundario en un espacio de nombres y, a continuación, haga clic en **Propiedades**.

3.  En la pestaña **referencias** , seleccione un método de ordenación.

> [!NOTE]
> Para usar Windows PowerShell para establecer el método de ordenación para destinos en referencias raíz de espacio de nombres, use el cmdlet [set-DfsnRoot](https://technet.microsoft.com/library/jj884281.aspx) con uno de los parámetros siguientes:
>    -   **EnableSiteCosting** especifica el método de **ordenación de costo más bajo**
>    -   **EnableInsiteReferrals** especifica los **destinos de exclusión fuera del método de ordenación de sitios del cliente** .
>    -   Si se omite el parámetro, se especifica el método de ordenación de referencia de **orden aleatorio** .

El módulo de Windows PowerShell DFSN se presentó en Windows Server 2012.

## <a name="to-set-the-ordering-method-for-targets-in-folder-referrals"></a>Para establecer el método de ordenación para destinos de referencias de carpetas

Las carpetas con destinos heredan el método de ordenación desde la raíz del espacio de nombres. Puede invalidar el método de ordenación mediante el procedimiento siguiente:

1.  Haga clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **Espacios de nombres**, haga clic con el botón secundario en una carpeta con destinos y, a continuación, haga clic en **Propiedades**.

3.  En la pestaña **referencias** , active la casilla **excluir destinos fuera del sitio del cliente** .

> [!NOTE]
> Para usar Windows PowerShell para excluir destinos de carpeta fuera del sitio del cliente, use el cmdlet [set-DfsnFolder – EnableInsiteReferrals](https://technet.microsoft.com/library/jj884283.aspx) .

## <a name="target-referral-ordering-methods"></a>Métodos de ordenación de referencias a destinos

Los tres métodos de ordenación son los siguientes:

-   Orden aleatorio
-   Costo más bajo
-   Excluir destinos fuera del sitio del cliente

## <a name="random-order"></a>Orden aleatorio

En este método, los destinos se ordenan del modo siguiente:

1.  Los destinos en el mismo sitio de Active Directory servicios de directorio (AD DS) que el cliente se enumeran en orden aleatorio en la parte superior de la referencia.
2.  Los destinos que se encuentran fuera del sitio del cliente se muestran en orden aleatorio.

Si no hay disponible ningún servidor de destino en el mismo sitio, se hace referencia al equipo cliente a un servidor de destino aleatorio, independientemente del grado de disponibilidad de la conexión o de la distancia del destino.

## <a name="lowest-cost"></a>Costo más bajo

En este método, los destinos se ordenan del modo siguiente:

1.  Los destinos que se encuentran en el mismo sitio que el cliente se enumeran en orden aleatorio en la parte superior de la referencia.
2.  Los destinos que se encuentran fuera del sitio del cliente se muestran en orden de costo más bajo al más alto. Las referencias con el mismo costo se agrupan juntas y los destinos se enumeran en orden aleatorio en cada grupo.

> [!NOTE]
> Los costos de vínculo a sitios no se muestran en el complemento Administración de DFS. Para ver los costos de vínculo de sitio, use el complemento sitios y servicios de Active Directory.

## <a name="exclude-targets-outside-of-the-clients-site"></a>Excluir destinos fuera del sitio del cliente

En este método, la referencia sólo contiene los destinos que están en el mismo sitio que el cliente. Estos destinos que se encuentran en el mismo sitio aparecen enumerados en orden aleatorio. Si no existe ningún destino en el mismo sitio, el cliente no recibe ninguna referencia y no puede tener acceso a esa parte del espacio de nombres.

> [!NOTE]
> Los destinos que tienen la prioridad de destino establecida en "primero entre todos los destinos" o "último entre todos los destinos" siguen apareciendo en la referencia, incluso si el método de ordenación se establece en **excluir destinos fuera del sitio del cliente**.

## <a name="additional-references"></a>Referencias adicionales

-   [Ajustar espacios de nombres DFS](tuning-dfs-namespaces.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)