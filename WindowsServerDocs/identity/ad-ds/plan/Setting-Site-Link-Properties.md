---
ms.assetid: de054ac2-a386-43ec-a537-c0de21549741
title: Establecer propiedades de enlace de sitio
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 495ed006ecac5458877191a14060c5fd4b746d96
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="setting-site-link-properties"></a>Establecer propiedades de enlace de sitio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se produce la replicación según las propiedades de los objetos de conexión. Cuando el Comprobador de coherencia de la información (KCC) crea objetos de conexión, se deriva la programación de replicación de propiedades de los objetos de vínculos de sitio. Cada objeto de enlace de sitio representa la conexión de área extensa (WAN) de red entre dos o más sitios.  
  
Establecer propiedades de objeto de enlace de sitio incluye los siguientes pasos:  
  
-   Determinar el costo asociado a esa ruta de acceso de replicación. El KCC usa costo para determinar la ruta menos costosa para la replicación entre dos sitios que duplicar la misma partición de directorio.  
  
-   Determinar el plan que define los períodos de tiempo durante el que la replicación entre sitios puede producirse.  
  
-   Determinar el intervalo de replicación que define la frecuencia con la replicación debe producirse durante las horas cuando se permite la duplicación, como se define en la programación.  
  
## <a name="in-this-guide"></a>En esta guía  
  
-   [Determinar el costo](../../ad-ds/plan/Determining-the-Cost.md)  
  
-   [Determinar la programación](../../ad-ds/plan/Determining-the-Schedule.md)  
  
-   [Determinar el intervalo de conexión](../../ad-ds/plan/Determining-the-Interval.md)  
  


