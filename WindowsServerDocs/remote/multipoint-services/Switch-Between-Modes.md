---
title: Cambiar entre modos
description: Obtenga información sobre cómo cambiar entre la estación y el modo de consola en Multipoint Services
ms.topic: article
ms.assetid: 5f1b2324-c1b0-4b61-ab51-39af15e7792a
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: efd6a8fd62fa32bc892fcab43935b2a1d8a8f676
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969292"
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

## <a name="see-also"></a>Consulte también
[Administrar tareas del sistema mediante MultiPoint Manager](Manage-System-Tasks-Using-MultiPoint-Manager.md)