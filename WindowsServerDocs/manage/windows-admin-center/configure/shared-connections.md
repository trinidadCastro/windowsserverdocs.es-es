---
title: Configurar conexiones compartidas para todos los usuarios de la puerta de enlace de Windows Admin Center
description: Obtén información sobre cómo los administradores pueden configurar su puerta de enlace de Windows Admin Center (proyecto Honolulu) una vez para permitir que todos los usuarios a compartir una sola lista de conexiones.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/28/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 1bdafa0d23934666fc0f6985249502e4960dc38e
ms.sourcegitcommit: 74d9eee13386a039186a70cdc97dc0b82e74bbf4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "9271026"
---
# Configurar conexiones compartidas para todos los usuarios de la puerta de enlace de Windows Admin Center

> Se aplica a: Windows Admin Center Preview

Con la capacidad de configurar conexiones compartidas, los administradores de puerta de enlace pueden configurar la lista de conexiones de una vez para todos los usuarios de una puerta de enlace de Windows Admin Center determinado. 

En la pestaña **Conexiones compartidas** de puerta de enlace de Windows Admin Center configuración, los administradores de puerta de enlace pueden agregar servidores, clústeres y conexiones de PC como lo haría en la página de conexiones todas, incluida la posibilidad de conexiones de etiqueta. Cualquier conexiones y etiquetas agregadas en la lista de conexiones compartidas aparecerán para todos los usuarios de esta puerta de enlace de Windows Admin Center, desde sus todas las páginas de conexiones.
    ![](../media/shared-cnxns-1.png)

Cuando los usuarios de Windows Admin Center tiene acceso a la página de "Todas las conexiones" después de que se han configurado las conexiones compartidas, podrán ver las conexiones que se agrupan en dos secciones: conexiones Personal y compartido. El grupo Personal es la lista de conexión de un usuario específico y se conserva entre las sesiones de usuario del explorador. El grupo de conexiones Shared es la misma en todos los usuarios y no se puede modificar desde la página de todas las conexiones.
![](../media/shared-cnxns-2.png)