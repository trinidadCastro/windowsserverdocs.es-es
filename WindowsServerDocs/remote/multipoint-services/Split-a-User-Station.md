---
title: Dividir una estación de usuario
description: Obtenga información sobre cómo dividir una pantalla en Multipoint Services para que dos usuarios puedan usar la misma estación
ms.custom: na
ms.date: 07/08/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f0d1fc9c-f5ea-45bc-a8da-623c5d081cdf
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 5067df3f5902570d56ee130264c751d66b5b0d3f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394949"
---
# <a name="split-a-user-station"></a>Dividir una estación de usuario
Cualquier monitor de estación de MultiPoint Services que tenga una resolución mayor de 1024x768 se puede dividir en dos estaciones mediante la tarea **Dividir estación** en la pestaña **Estaciones**. El escritorio que está presente en el monitor en el momento en el que tiene lugar la división se mueve a la mitad izquierda del monitor y se crea una nueva estación en la mitad derecha del mismo monitor. La nueva estación se debe asignar a un teclado, mouse y concentrador USB para completar la creación. Una vez que se haya dividido la estación, un usuario puede iniciar sesión en la estación izquierda mientras que otro usuario lo hace en la estación derecha.  
  
Las ventajas del uso de una estación de pantalla dividida pueden incluir:  
  
-   Reducir coste y espacio al albergar a más estudiantes en un sistema MultiPoint Services  
  
-   Permitir que dos estudiantes colaboren juntos, en paralelo en un proyecto  
  
-   Permitir al profesor demostrar un procedimiento en una estación mientras un estudiante sigue la explicación en otra estación  
   
> [!NOTE]  
> Cuando divide una estación, la sesión activa de la estación se suspende. El usuario debe iniciar sesión en la estación de nuevo para reanudar el trabajo una vez que ha tenido lugar la división.  
  
**Para dividir una estación:**  
  
1.  En Multipoint Manager, en modo de estación, haga clic en la pestaña **estaciones** .  
  
2.  En la columna **Station** (Estación), haga clic en el nombre de la estación que quiera dividir.  
  
3.  En **Stations Tasks** (Tareas de estación), haga clic en **Split station** (Dividir estación).  
  
**Para devolver una estación dividida a una sola estación:**  
  
1.  En Multipoint Manager, en modo de estación, haga clic en la pestaña **estaciones** .  
  
2.  En la columna **Station** (Estación), haga clic en el nombre de la estación de la que quiere finalizar la división.  
  
3.  En **Station Tasks** (Tareas de estación), haga clic en **Unsplit station** (Unir estación).  
  
## <a name="see-also"></a>Vea también  
[Administrar estaciones de usuario](Manage-User-Stations.md)