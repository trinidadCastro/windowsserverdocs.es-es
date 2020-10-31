---
ms.assetid: 28ecaf5c-9131-406c-b211-a230162e462e
title: Determinar la programación
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 9a882ab616fae5a5ab649fb5a988aa90e8bc7a9c
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93068747"
---
# <a name="determining-the-schedule"></a>Determinar la programación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede controlar la disponibilidad de los vínculos a sitios mediante la configuración de una programación de vínculos a sitios. Cuando la replicación entre dos sitios atraviesa varios vínculos a sitios, la intersección de las programaciones de replicación en todos los vínculos pertinentes determina la programación de conexión entre los dos sitios.

Para planear la configuración de la programación de vínculos a sitios, cree dos programaciones superpuestas entre los vínculos de sitios que contienen controladores de dominio que se replican directamente entre sí. Use la programación predeterminada (100-porcentaje disponible) en esos vínculos a menos que desee bloquear el tráfico de replicación durante las horas punta. Al bloquear la replicación, da prioridad a otro tráfico, pero también aumenta la latencia de replicación.

Los controladores de dominio almacenan la hora en hora universal coordinada (UTC). La configuración de tiempo de las programaciones de objetos de vínculo de sitio se ajusta a la hora local del sitio y el equipo en el que se establece la programación. Cuando un controlador de dominio se pone en contacto con un equipo que se encuentra en otro sitio y zona horaria, la programación del controlador de dominio muestra la configuración de tiempo según la hora local del sitio del equipo.



