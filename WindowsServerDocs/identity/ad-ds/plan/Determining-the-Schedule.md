---
ms.assetid: 28ecaf5c-9131-406c-b211-a230162e462e
title: Determinar la programación
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dee63ce0fb687b2b722ce64614c54388fc544433
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839006"
---
# <a name="determining-the-schedule"></a>Determinar la programación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede controlar la disponibilidad de vínculo de sitio estableciendo una programación para los vínculos a sitios. Cuando la replicación entre dos sitios atraviesa varios vínculos a sitios, la intersección de las programaciones de replicación en todos los vínculos relevantes determina la programación de la conexión entre los dos sitios.  
  
Para planear la configuración de la programación de vínculo de sitio, crear dos programaciones superpuestas entre los vínculos a sitios que contienen los controladores de dominio que se replican directamente entre sí. Usar la programación predeterminada (100 por ciento disponible) en esos vínculos a menos que desee bloquear el tráfico de replicación durante las horas punta. Al bloquear la duplicación, dar prioridad a otro tráfico, pero también aumenta la latencia de replicación.  
  
Los controladores de dominio almacenan la hora en hora Universal coordinada (UTC). Configuración de hora en las programaciones de objeto de vínculo de sitio se ajusta a la hora local del sitio y el equipo en el que se establece la programación. Cuando un controlador de dominio pone en contacto con un equipo que está en un sitio diferente y la zona horaria, la programación en el controlador de dominio muestra el valor del tiempo según la hora local para el sitio del equipo.  
  


