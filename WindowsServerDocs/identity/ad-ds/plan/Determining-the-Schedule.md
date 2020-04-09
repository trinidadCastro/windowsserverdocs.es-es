---
ms.assetid: 28ecaf5c-9131-406c-b211-a230162e462e
title: Determinar la programación
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d5c1c7c77304317bab2e80223dd745c88bc79efd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822548"
---
# <a name="determining-the-schedule"></a>Determinar la programación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede controlar la disponibilidad de los vínculos a sitios mediante la configuración de una programación de vínculos a sitios. Cuando la replicación entre dos sitios atraviesa varios vínculos a sitios, la intersección de las programaciones de replicación en todos los vínculos pertinentes determina la programación de conexión entre los dos sitios.  
  
Para planear la configuración de la programación de vínculos a sitios, cree dos programaciones superpuestas entre los vínculos de sitios que contienen controladores de dominio que se replican directamente entre sí. Use la programación predeterminada (100-porcentaje disponible) en esos vínculos a menos que desee bloquear el tráfico de replicación durante las horas punta. Al bloquear la replicación, da prioridad a otro tráfico, pero también aumenta la latencia de replicación.  
  
Los controladores de dominio almacenan la hora en hora universal coordinada (UTC). La configuración de tiempo de las programaciones de objetos de vínculo de sitio se ajusta a la hora local del sitio y el equipo en el que se establece la programación. Cuando un controlador de dominio se pone en contacto con un equipo que se encuentra en otro sitio y zona horaria, la programación del controlador de dominio muestra la configuración de tiempo según la hora local del sitio del equipo.  
  


