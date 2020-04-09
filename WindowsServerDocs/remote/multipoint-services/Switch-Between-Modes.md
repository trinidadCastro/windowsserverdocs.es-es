---
title: Cambiar entre modos
description: Obtenga información sobre cómo cambiar entre la estación y el modo de consola en Multipoint Services
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 5f1b2324-c1b0-4b61-ab51-39af15e7792a
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 80b56fcf5aca3530e41fba0125341031bb8112a7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855578"
---
# <a name="switch-between-modes"></a>Cambiar entre modos
Multipoint Manager incluye los siguientes modos para ayudarle a realizar diferentes tipos de administración del sistema Multipoint Services:  
  
-   *Modo de estación*: de forma predeterminada, el sistema MultiPoint Services se inicia en modo de estación. En el modo de estación, las estaciones de MultiPoint Services se comportan como si cada estación fuera un equipo independiente que ejecuta Windows y varios usuarios pueden usar el sistema a la vez. Usted y sus usuarios pueden compartir archivos y llevar a cabo las tareas que deben realizar.  
  
-   *Modo de consola*: cuando el sistema MultiPoint Services está en modo de consola, puede instalar y actualizar el software y los controladores o efectuar otras tareas de mantenimiento. Cuando el sistema está en modo de consola, no está disponible ninguna *estación* para que la usen otros usuarios del equipo. Dichas estaciones no se muestran en Multipoint Manager. Todos los monitores conectados directamente al servidor se tratan como presentaciones de este sistema.   
  
> [!NOTE]
> Puede forzar el inicio del sistema en modo de consola al cambiar el valor predeterminado en la configuración del servidor.  
> ## <a name="to-switch-from-station-mode-to-console-mode"></a>Cambiar del modo de estación al modo de consola  
  
1.  Abra Multipoint Manager en modo de estación y, después, haga clic en la pestaña **Inicio** .  
  
2.  En la columna **Equipo**, haga clic en el equipo del que quiere cambiar los modos.  
  
3.  En *computer name* **tareas**de nombre de equipo, haga clic en **cambiar al modo de consola**. El equipo se reinicia y no hay ninguna estación disponible.  
  
## <a name="to-switch-from-console-mode-to-station-mode"></a>Cambiar del modo de consola al modo de estación  
  
1.  Abra Multipoint Manager en el modo de consola y, a continuación, haga clic en la pestaña **Inicio** .  
  
2.  En la columna **Equipo**, haga clic en el equipo del que quiere cambiar los modos.  
  
3.  En *computer name* **tareas**de nombre de equipo, haga clic en **cambiar al modo de estación**. El equipo se reinicia y todas las estaciones están disponibles.  
  
## <a name="see-also"></a>Consulta también  
[Administrar tareas del sistema mediante MultiPoint Manager](Manage-System-Tasks-Using-MultiPoint-Manager.md)