---
title: Personalizar el panel de navegación del centro de administración de Active Directory
ms.prod: windows-server-threshold
description: Seguridad de Windows Server
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.assetid: c9933d16-e153-435a-b5b7-3e594db42d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: e7b1128d93912f724225905bedd38131f8aab0b2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854846"
---
# <a name="customize-the-active-directory-administrative-center-navigation-pane"></a>Personalizar el panel de navegación del centro de administración de Active Directory

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

  Puede examinar el panel de navegación del centro de administración de Active Directory mediante el uso de la vista de árbol, que es similar al árbol de consola de equipos y usuarios de Active Directory, o mediante el uso de la vista de lista.

 Si usa la vista de árbol o la vista de lista, puede personalizar el panel de navegación del centro de administración de Active Directory en cualquier momento mediante la adición de varios contenedores desde el dominio local o de cualquier dominio externo \(es decir, un dominio distinto del dominio local que tenga una confianza establecida con el dominio local\) al panel de navegación como nodos independientes. Personalizar el panel de navegación del centro de administración de Active Directory puede proporcionar un acceso más rápido a los objetos de Active Directory. Para obtener más información, consulte [administrar dominios diferentes en el centro de administración de Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

 Además, para personalizar aún más el panel de navegación, puede cambiar el nombre o quitar estos nodos del panel de navegación agregados manualmente, puede crear duplicados de estos nodos o moverlos arriba o abajo dentro del panel de navegación.

> [!NOTE]
>  No puede personalizar el nodo de dominio local predeterminado.

### <a name="to-customize-the-active-directory-administrative-center-navigation-pane"></a>Para personalizar el panel de navegación del centro de administración de Active Directory

1.  En el panel de navegación del centro de administración de Active Directory, a la derecha\-haga clic en el nodo que desea modificar. Puede modificar la posición o el nombre del nodo, o puede crear un duplicado.

2.  Haga clic en uno de los siguientes comandos:

    -   **Rename**

    -   **Duplicar nodo**

    -   **Quitar**

    -   **Mover hacia arriba**

    -   **Mover hacia abajo**

 Mediante el uso de la vista de lista, puede sacar partido de los usados más recientemente \(MRU\) lista. La lista de elementos utilizados Recientemente aparece automáticamente en un nodo de navegación cuando visita al menos un contenedor en este nodo de navegación. También puede ver la lista MRU actual mediante la expansión de la barra de ruta de navegación en la parte superior de la ventana del centro de administración de Active Directory. La lista de elementos utilizados Recientemente siempre contiene los últimos tres contenedores que visitó en un nodo de navegación determinado. Cada vez que selecciona un contenedor determinado, éste se agrega al principio de la lista de elementos utilizados recientemente y se quita el último contenedor de dicha lista.

  

