---
title: "Personalizar el panel de navegación del centro de administración de Active Directory"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="customize-the-active-directory-administrative-center-navigation-pane"></a>Personalizar el panel de navegación del centro de administración de Active Directory

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

  Puedes buscar en el panel de navegación del centro de administración de Active Directory mediante el uso de la vista de árbol, que es similar al árbol de consola de equipos y usuarios de Active Directory, o mediante el uso de la vista de lista.

 Si usas la vista de árbol o la vista de lista, puedes personalizar el panel de navegación del centro de administración de Active Directory en cualquier momento mediante la adición de varios contenedores desde el dominio local o en cualquier dominio externo \ (es decir, un dominio distinto del dominio local que tiene un establecer la confianza con el domain\ local) en el panel de navegación como nodos separados. Personalizar el panel de navegación del centro de administración de Active Directory puede proporcionar un acceso más rápido a los objetos de Active Directory. Para obtener más información, consulta [administrar distintos dominios en el centro de administración de Active Directory ](manage-different-domains-in-active-directory-administrative-center.md).

 Además, para personalizar aún más el panel de navegación, puedes cambiar el nombre o quitar estos nodos del panel de navegación agregados manualmente, crear duplicados de estos nodos o moverlos hacia arriba o hacia abajo en el panel de navegación.

> [!NOTE]
>  No se puede personalizar el nodo de dominio local de forma predeterminada.

### <a name="to-customize-the-active-directory-administrative-center-navigation-pane"></a>Para personalizar el panel de navegación del centro de administración de Active Directory

1.  En el panel de navegación del centro de administración de Active Directory, right\ y haga clic en el nodo al que quieres modificar. Puedes modificar la posición o el nombre del nodo, o puedes crear un duplicado.

2.  Haz clic en uno de los siguientes comandos:

    -   **Cambiar nombre**

    -   **Duplicar nodo**

    -   **Quitar**

    -   **Desplazar hacia arriba**

    -   **Desplazar hacia abajo**

 Mediante el uso de la vista de lista, puedes sacar provecho de la lista \(MRU\) usados más recientemente. La lista MRU aparece automáticamente en un nodo de navegación cuando visita al menos un contenedor en este nodo de navegación. También puedes ver la lista actual de la lista MRU, amplía la barra de navegación en la parte superior de la ventana del centro de administración de Active Directory. La lista MRU siempre contiene los últimos tres contenedores que ha visitado en un nodo de navegación determinado. Cada vez que selecciones un contenedor particular, este contenedor se agrega a la parte superior de la lista MRU y se quitará el último contenedor de la lista MRU.

  

