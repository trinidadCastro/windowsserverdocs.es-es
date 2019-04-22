---
title: Elegir un diseño de BranchCache
description: En este tema forma parte de BranchCache Deployment Guide para Windows Server 2016, que demuestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN de sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 86c1ccad-2aa4-40fe-84c1-f77c49eb1216
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 330dcbee26f52ff69cd85ef8dc78d2e161b943d1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59811916"
---
# <a name="choosing-a-branchcache-design"></a>Elegir un diseño de BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información acerca de los modos de BranchCache y seleccionar los modos mejor para su implementación.  
  
Puede usar a esta guía para implementar BranchCache en los siguientes modos y combinaciones de modo.  
  
-   Todas las sucursales están configuradas para el modo caché distribuida.  
  
-   Todas las sucursales están configuradas para el modo caché hospedada y tienen un servidor de caché hospedada en el sitio.  
  
-   Algunas sucursales están configurados para el modo caché distribuida y algunas sucursales tienen un servidor de caché hospedada en el sitio y se configuran en modo de caché hospedada.  
  
La siguiente ilustración muestra una instalación de modo dual, con una sucursal configurado para el modo caché distribuida y una sucursal configurado para el modo caché hospedada.  
  
![Elegir un diseño de BranchCache](../../media/Choosing-a-BranchCache-Design/bc_new_modes.jpg)  
  
Antes de implementar BranchCache, seleccione el modo que prefiera para cada sucursal de su organización.  
  


