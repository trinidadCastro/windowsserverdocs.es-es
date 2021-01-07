---
title: Personalización del panel de navegación Centro de administración de Active Directory
description: Seguridad de Windows Server
ms.assetid: c9933d16-e153-435a-b5b7-3e594db42d5c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.topic: article
ms.openlocfilehash: 1ae8f97a981296f14ede5f00b46196ba4b5e2465
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950491"
---
# <a name="customize-the-active-directory-administrative-center-navigation-pane"></a>Personalización del panel de navegación Centro de administración de Active Directory

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

  Puede examinar el panel de navegación Centro de administración de Active Directory mediante la vista de árbol, que es similar al árbol de consola de usuarios y equipos de Active Directory, o mediante la vista de lista.

 Tanto si usa la vista de árbol como la vista de lista, puede personalizar el panel de navegación Centro de administración de Active Directory en cualquier momento si agrega varios contenedores desde el dominio local o cualquier dominio externo \( , es decir, un dominio que no sea el dominio local y que tenga una confianza establecida con el dominio local \) en el panel de navegación como nodos independientes. La personalización del panel de navegación Centro de administración de Active Directory puede proporcionar un acceso más rápido a los objetos de Active Directory. Para obtener más información, vea [Administración de dominios diferentes en centro de administración de Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

 Además, para personalizar aún más el panel de navegación, puede cambiar el nombre o quitar estos nodos del panel de navegación agregados manualmente, puede crear duplicados de estos nodos o moverlos arriba o abajo dentro del panel de navegación.

> [!NOTE]
>  No puede personalizar el nodo de dominio local predeterminado.

### <a name="to-customize-the-active-directory-administrative-center-navigation-pane"></a>Para personalizar el panel de navegación del Centro de administración de Active Directory

1. En el panel de navegación Centro de administración de Active Directory, haga clic con el botón secundario \- en el nodo que desea modificar. Puede modificar la posición o el nombre del nodo, o puede crear un duplicado.

2. Haga clic en uno de los siguientes comandos:

   -   **Cambiar nombre**

   -   **Duplicar nodo**

   -   **Remove**

   -   **Subir**

   -   **Bajar**

   Mediante la vista de lista, puede aprovechar las ventajas de la lista MRU utilizada más recientemente \( \) . La lista MRU aparece automáticamente bajo un nodo de navegación al visitar al menos un contenedor en este nodo de navegación. También puede ver la lista MRU actual expandiendo la barra de ruta de navegación en la parte superior de la ventana de Centro de administración de Active Directory. La lista MRU siempre contiene los tres últimos contenedores que visitó en un nodo de navegación determinado. Cada vez que selecciona un contenedor determinado, éste se agrega al principio de la lista de elementos utilizados recientemente y se quita el último contenedor de dicha lista.



