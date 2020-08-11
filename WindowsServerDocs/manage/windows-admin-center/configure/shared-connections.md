---
title: Configuración de conexiones compartidas para todos los usuarios de la puerta de enlace de Windows Admin Center
description: Descubre cómo los administradores pueden configurar su puerta de enlace de Windows Admin Center (proyecto Honolulu) una vez para que todos los usuarios puedan compartir una única lista de conexiones.
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/28/2019
ms.localizationpriority: medium
ms.openlocfilehash: eea0e467629a6110d38ed3081a1320e002496cfb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970882"
---
# <a name="configure-shared-connections-for-all-users-of-the-windows-admin-center-gateway"></a>Configuración de conexiones compartidas para todos los usuarios de la puerta de enlace de Windows Admin Center

> Se aplica a: Versión preliminar de Windows Admin Center, Windows Admin Center

Con la capacidad de configurar conexiones compartidas, los administradores de puertas de enlace pueden configurar la lista de conexiones una vez para todos los usuarios de una puerta de enlace determinada de Windows Admin Center. Esta característica solo está disponible en el modo de servicio de Windows Admin Center.

En la pestaña **Conexiones compartidas** de la Configuración de la puerta de enlace de Windows Admin Center, los administradores de puertas de enlace pueden agregar servidores, clústeres y conexiones de PC del mismo modo que desde la página Todas las conexiones, incluida la posibilidad de etiquetar conexiones. Todas las conexiones y etiquetas que se agreguen en la lista Conexiones compartidas se mostrarán para todos los usuarios de esta puerta de enlace de Windows Admin Center desde su página Todas las conexiones.

![Windows Admin Center: página Conexiones compartidas](../media/shared-cnxns-1.png)

Cuando cualquier usuario de Windows Admin Center acceda a la página "Todas las conexiones" una vez configuradas las Conexiones compartidas, verá sus conexiones agrupadas en dos secciones: personales y compartidas. El grupo Personales es la lista de conexiones de un usuario específico y se conserva entre sesiones del explorador del usuario. El grupo de conexiones Compartidas es el mismo para todos los usuarios y no se puede modificar desde la página Todas las conexiones.

![Windows Admin Center: página Todas las conexiones](../media/shared-cnxns-2.png)