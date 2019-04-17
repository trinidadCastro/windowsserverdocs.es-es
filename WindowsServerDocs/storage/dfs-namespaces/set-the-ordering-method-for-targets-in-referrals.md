---
title: "Establecer el método para ordenar destinos en las referencias"
description: "En este artículo se describe cómo establecer el método para ordenar los destinos en las referencias."
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 6c67be4b35dd986f14bf7d588d0f3baa88e19171
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="set-the-ordering-method-for-targets-in-referrals"></a>Establecer el método para ordenar destinos en las referencias

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Una referencia es una lista ordenada de destinos que un equipo cliente recibe de un controlador de dominio o un servidor de espacio de nombres cuando el usuario tiene acceso a una carpeta o raíz de espacio de nombres con destinos. Una vez que el cliente recibe la referencia, intenta tener acceso al primer destino de la lista. Si el destino no está disponible, el cliente intenta tener acceso al siguiente destino.
Los destinos en el sitio del cliente siempre se muestran primero en una referencia. Los destinos fuera del sitio del cliente se muestran según el método para ordenar.

Usa las siguientes secciones para especificar en qué orden los destinos deben hacer referencia a los clientes y para comprender los distintos métodos para ordenar referencias de destino:

## <a name="to-set-the-ordering-method-for-targets-in-namespace-root-referrals"></a>Para establecer el método para ordenar los destinos en las referencias de raíz del espacio de nombres

Usa el siguiente procedimiento para establecer el método para ordenar en la raíz del espacio de nombres:

1.  Haz clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **Espacios de nombres**, haz clic con el botón derecho en un espacio de nombres y luego haz clic en **Propiedades**.

3.  En la pestaña **Referencias** selecciona un método para ordenar.

> [!NOTE]
> Para usar Windows PowerShell para establecer el método para ordenar de destinos en las referencias de raíz del espacio de nombres, usa el cmdlet [Set-DfsnRoot](https://technet.microsoft.com/library/jj884281.aspx) con uno de los siguientes parámetros:
   -   **EnableSiteCosting** especifica el método **Ordenación de menor costo**
   -   **EnableInsiteReferrals** especifica el método para ordenar **Excluir destinos fuera del sitio del cliente**
   -   Si omites algún parámetro, especifica el método para ordenar referencias **Orden aleatorio**. 

El módulo de WindowsPowerShell para DFSN se introdujo en WindowsServer2012.
   
## <a name="to-set-the-ordering-method-for-targets-in-folder-referrals"></a>Para establecer el método para ordenar destinos en las referencias a las carpetas

Las carpetas con destinos heredan el método para ordenar de la raíz del espacio de nombres. Puedes anular el método para ordenar mediante el siguiente procedimiento:

1.  Haz clic sucesivamente en **Inicio**, **Herramientas administrativas** y **Administración de DFS**.

2.  En el árbol de consola, en el nodo **Espacios de nombres**, haz clic con el botón derecho en una carpeta con destinos y luego haz clic en **Propiedades**.

3.  En la pestaña **Referencias**, selecciona la casilla de verificación **Excluir destinos fuera del sitio del cliente**.

> [!NOTE]
> Para que Windows PowerShell excluya destinos de carpeta fuera del sitio del cliente, usa el cmdlet [Set-DfsnFolder –EnableInsiteReferrals](https://technet.microsoft.com/library/jj884283.aspx).

## <a name="target-referral-ordering-methods"></a>Métodos para ordenar referencias de destinos

Los tres métodos para ordenar son:

-   Orden aleatorio
-   Menor coste
-   Excluir destinos fuera del sitio del cliente

## <a name="random-order"></a>Orden aleatorio

En este método, los destinos se ordenan tal y como se indica a continuación:

1.  Los destinos en el mismo sitio de Active Directory Directory Services (AD DS) que el cliente se enumeran en orden aleatorio en la parte superior de la referencia.
2.  Los destinos fuera del sitio del cliente se enumeran en orden aleatorio.

Si no hay disponible ningún servidor de destino del mismo sitio, el equipo cliente hace referencia a un servidor de destino aleatorio independientemente del coste de la conexión o la distancia del destino.

## <a name="lowest-cost"></a>Menor coste

En este método, los destinos se ordenan tal y como se indica a continuación:

1.  Los destinos en el mismo sitio que el cliente se enumeran en orden aleatorio en la parte superior de la referencia.
2.  Los destinos fuera del sitio del cliente se enumeran en orden de coste menor a mayor. Las referencias con el mismo coste se agrupan y los destinos se enumeran en orden aleatorio en de cada grupo.

> [!NOTE]
> Los costes de los vínculos de sitio no se muestran en el complemento de administración de DFS. Para ver los costes de los vínculos de sitio, usa el complemento Sitios y servicios de Active Directory.

## <a name="exclude-targets-outside-of-the-clients-site"></a>Excluir destinos fuera del sitio del cliente

En este método, la referencia incluye únicamente los destinos que están en el mismo sitio que el cliente. Estos objetivos del mismo sitio se enumeran en orden aleatorio. Si no existe ningún destino del mismo sitio, el cliente no recibe ninguna referencia y no puede acceder a esa parte del espacio de nombres.

> [!NOTE]
> Los destinos que tienen la prioridad de destino establecida en "Primero de todos los destinos" o "Último de todos los destinos" siguen apareciendo en la referencia, incluso si el método para ordenar se establece en **Excluir destinos fuera del sitio del cliente**.

## <a name="see-also"></a>Consulta también 

-   [Ajustar espacios de nombres DFS](tuning-dfs-namespaces.md)
-   [Delegar permisos de administración para espacios de nombres DFS](delegate-management-permissions-for-dfs-namespaces.md)