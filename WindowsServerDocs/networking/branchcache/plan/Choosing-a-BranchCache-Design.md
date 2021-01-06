---
title: Elegir un diseño de BranchCache
description: Aprenda a obtener información sobre los modos de BranchCache y a seleccionar los mejores modos para su implementación.
ms.topic: how-to
ms.assetid: 86c1ccad-2aa4-40fe-84c1-f77c49eb1216
ms.author: lizross
author: eross-msft
ms.date: 01/05/2021
ms.openlocfilehash: 972f2941afa2544307933ec422eb40578e084672
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946711"
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



