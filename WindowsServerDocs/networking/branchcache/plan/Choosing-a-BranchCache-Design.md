---
title: Elegir un diseño BranchCache
description: En este tema es parte de la BranchCache implementación Guía para Windows Server 2016, que se muestra cómo implementar BranchCache en modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN en sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 86c1ccad-2aa4-40fe-84c1-f77c49eb1216
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4fe40b3d9ece771a46af8ecc70297b8713d65875
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="choosing-a-branchcache-design"></a>Elegir un diseño BranchCache

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información acerca de los modos de BranchCache y seleccionar los modos recomendados para la implementación.  
  
Puedes usar a esta guía para implementar BranchCache en los siguientes modos y combinaciones de modo.  
  
-   Todas las sucursales están configuradas para el modo de caché distribuido.  
  
-   Todas las sucursales están configuradas para el modo de caché hospedada y tengan un servidor de la memoria caché hospedada en el sitio.  
  
-   Algunas sucursales están configurados para el modo de caché distribuida y algunas sucursales dispone de un servidor de la memoria caché hospedada en el sitio y están configurados para el modo de caché hospedada.  
  
La ilustración siguiente muestra una instalación de modo dual, con una sucursal configurado para el modo de caché distribuida y una sucursal configurado para el modo de la memoria caché hospedada.  
  
![Elegir un diseño BranchCache](../../media/Choosing-a-BranchCache-Design/bc_new_modes.jpg)  
  
Antes de implementar BranchCache, selecciona el modo en que prefieras para cada sucursal en la organización.  
  


