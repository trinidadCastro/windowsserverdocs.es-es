---
title: Elegir un diseño de BranchCache
description: Este tema forma parte de la guía de implementación de BranchCache para Windows Server 2016, que muestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso del ancho de banda WAN en las sucursales.
manager: brianlic
ms.topic: get-started-article
ms.assetid: 86c1ccad-2aa4-40fe-84c1-f77c49eb1216
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f64d27f3ef36c587e2ec5c08f51723942ba6a28c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87937514"
---
# <a name="choosing-a-branchcache-design"></a>Elegir un diseño de BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre los modos de BranchCache y para seleccionar los mejores modos para su implementación.

Puede usar esta guía para implementar BranchCache en los siguientes modos y combinaciones de modo.

-   Todas las sucursales están configuradas para el modo caché distribuida.

-   Todas las sucursales están configuradas para el modo caché hospedada y tienen un servidor de caché hospedada en el sitio.

-   Algunas sucursales están configuradas para el modo caché distribuida y algunas sucursales tienen un servidor de caché hospedada en el sitio y están configuradas para el modo caché hospedada.

En la ilustración siguiente se muestra una instalación en modo dual, con una sucursal configurada para el modo caché distribuida y una sucursal configurada para el modo caché hospedada.

![Elegir un diseño de BranchCache](../../media/Choosing-a-BranchCache-Design/bc_new_modes.jpg)

Antes de implementar BranchCache, seleccione el modo que prefiera para cada sucursal de su organización.



