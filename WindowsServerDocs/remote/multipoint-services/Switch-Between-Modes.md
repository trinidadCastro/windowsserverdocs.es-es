---
title: Cambiar entre modos
description: Obtenga información sobre cómo cambiar entre el modo de estación y la consola en MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5f1b2324-c1b0-4b61-ab51-39af15e7792a
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: a9c00d65ad1c59e91f4011ab932bf9f921957c59
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446150"
---
# <a name="switch-between-modes"></a>Cambiar entre modos
MultiPoint Manager incluye los siguientes modos para ayudarle a realizar diferentes tipos de administración del sistema MultiPoint Services:  
  
-   *Modo de estación*: De forma predeterminada, el sistema MultiPoint Services se inicia en modo de estación. En el modo de estación, las estaciones de MultiPoint Services se comportan como si cada estación fuera un equipo independiente que ejecuta Windows y varios usuarios pueden usar el sistema a la vez. Usted y sus usuarios pueden compartir archivos y llevar a cabo las tareas que deben realizar.  
  
-   *Modo de consola*: Cuando el sistema MultiPoint Services está en modo de consola, puede instalar y actualizar el software y los controladores o realizar otras tareas de mantenimiento. Cuando el sistema está en modo de consola, no está disponible ninguna *estación* para que la usen otros usuarios del equipo. No se muestran estas estaciones en MultiPoint Manager. Todos los monitores conectados directamente al servidor se tratan como pantallas de este equipo.   
  
> [!NOTE]
> Puede forzar el inicio del sistema en modo de consola al cambiar el valor predeterminado en la configuración del servidor.  
> ## <a name="to-switch-from-station-mode-to-console-mode"></a>Cambiar del modo de estación al modo de consola  
  
1.  Abra MultiPoint Manager en modo de estación y, a continuación, haga clic en el **inicio** ficha.  
  
2.  En la columna **Equipo**, haga clic en el equipo del que quiere cambiar los modos.  
  
3.  En *nombre_equipo* **tareas**, haga clic en **cambiar al modo de consola**. El equipo se reinicia y no hay ninguna estación disponible.  
  
## <a name="to-switch-from-console-mode-to-station-mode"></a>Cambiar del modo de consola al modo de estación  
  
1.  Abra MultiPoint Manager en modo de consola y, a continuación, haga clic en el **inicio** ficha.  
  
2.  En la columna **Equipo**, haga clic en el equipo del que quiere cambiar los modos.  
  
3.  En *nombre_equipo* **tareas**, haga clic en **cambiar al modo de estación**. El equipo se reinicia y todas las estaciones están disponibles.  
  
## <a name="see-also"></a>Vea también  
[Administrar tareas del sistema mediante MultiPoint Manager](Manage-System-Tasks-Using-MultiPoint-Manager.md)