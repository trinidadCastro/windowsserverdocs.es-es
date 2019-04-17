---
ms.assetid: 28ecaf5c-9131-406c-b211-a230162e462e
title: "Determinar la programación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0ec953b34475c50e62553a9ba95e4d45d9904bf3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="determining-the-schedule"></a>Determinar la programación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puedes controlar disponibilidad vínculos a sitios estableciendo una programación para vínculos a sitios. Cuando la replicación entre dos sitios atraviesa varios vínculos a sitios, la intersección de los programas de replicación en todos los vínculos relevantes determina la programación de conexión entre los dos sitios.  
  
Para planear la configuración de la programación de vínculo del sitio, crear dos programaciones superpuestas entre vínculos a sitios que contienen los controladores de dominio que replican directamente entre sí. Usar la programación predeterminada (100% disponible) en esos vínculos a menos que quieras bloquear el tráfico de replicación durante las horas pico. Al bloquear la duplicación, dar prioridad a otro tráfico, pero también aumenta la latencia de replicación.  
  
Controladores de dominio de la tienda tiempo en hora Universal coordinada (UTC). Configuración de tiempo en los planes de objeto de enlace de sitio se ajusta a la hora local del sitio y el equipo en el que se establece la programación. Cuando un equipo que está en un sitio diferente y la zona horaria en contacto con un controlador de dominio, la programación en el controlador de dominio muestra la configuración de tiempo según la hora local para el sitio del equipo.  
  


