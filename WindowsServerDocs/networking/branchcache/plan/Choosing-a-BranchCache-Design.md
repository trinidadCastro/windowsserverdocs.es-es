---
title: Elegir un diseño de BranchCache
description: Aprenda a obtener información sobre los modos de BranchCache y a seleccionar los mejores modos para su implementación.
ms.topic: get-started-article
ms.assetid: 86c1ccad-2aa4-40fe-84c1-f77c49eb1216
ms.author: lizross
author: eross-msft
ms.openlocfilehash: cfc4632d9e4c8b7904134a9222f9b2d87ea6cdd0
ms.sourcegitcommit: 029b1e19ce11160d5f988046e04a83e8ab5a60dc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97904400"
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



