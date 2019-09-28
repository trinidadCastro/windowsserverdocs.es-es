---
title: Elegir un diseño de BranchCache
description: Este tema forma parte de la guía de implementación de BranchCache para Windows Server 2016, que muestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso del ancho de banda WAN en las sucursales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 86c1ccad-2aa4-40fe-84c1-f77c49eb1216
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2a1b4615dfda3989f0321725fd27da066fcb8a5e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406321"
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
  


