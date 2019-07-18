---
title: Configurar conexiones compartidas para todos los usuarios de la puerta de enlace del centro de administración de Windows
description: Obtenga información acerca de cómo los administradores pueden configurar su puerta de enlace del centro de administración de Windows (Project Honolulu) una vez para permitir que todos los usuarios compartan una sola lista de conexiones.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/28/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 46075154a225590376458881768ae86f74a572ad
ms.sourcegitcommit: 286e3181ebd2cb9d7dc7fe651858a4e0d61d153f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2019
ms.locfileid: "68300724"
---
# <a name="configure-shared-connections-for-all-users-of-the-windows-admin-center-gateway"></a>Configurar conexiones compartidas para todos los usuarios de la puerta de enlace del centro de administración de Windows

> Se aplica a: Vista previa del centro de administración de Windows, centro de administración de Windows

Con la capacidad de configurar conexiones compartidas, los administradores de la puerta de enlace pueden configurar la lista de conexiones una vez para todos los usuarios de una puerta de enlace determinada del centro de administración de Windows. 

En la pestaña **conexiones** compartidas de la configuración de puerta de enlace del centro de administración de Windows, los administradores de puerta de enlace pueden agregar servidores, clústeres y conexiones de PC como lo haría en la página todas las conexiones, incluida la capacidad de etiquetar conexiones. Todas las conexiones y etiquetas agregadas en la lista conexiones compartidas se mostrarán para todos los usuarios de esta puerta de enlace del centro de administración de Windows, en la página todas las conexiones.
    ![](../media/shared-cnxns-1.png)

Cuando cualquier usuario del centro de administración de Windows tiene acceso a la página "todas las conexiones" una vez configuradas las conexiones compartidas, verá sus conexiones agrupadas en dos secciones: Conexiones personales y compartidas. El grupo personal es la lista de conexiones de un usuario específico y se conserva en las sesiones del explorador del usuario. El grupo conexiones compartidas es el mismo en todos los usuarios y no se puede modificar en la página todas las conexiones.
![](../media/shared-cnxns-2.png)