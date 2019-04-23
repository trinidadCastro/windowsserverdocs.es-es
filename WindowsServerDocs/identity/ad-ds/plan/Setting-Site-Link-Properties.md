---
ms.assetid: de054ac2-a386-43ec-a537-c0de21549741
title: Establecer las propiedades de vínculo de sitio
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4fa9a1fa8d2a463fe5f361a5a27ee2b9e3edc0f6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870366"
---
# <a name="setting-site-link-properties"></a>Establecer las propiedades de vínculo de sitio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La replicación entre sitios se produce según las propiedades de los objetos de conexión. Cuando el Comprobador de coherencia de la información (KCC) crea los objetos de conexión, se deriva la programación de replicación de las propiedades de los objetos de vínculo de sitio. Cada objeto de vínculo de sitio representa la conexión de área extensa (WAN) de red entre dos o más sitios.  
  
Establecer propiedades de objeto de vínculo de sitio incluye los siguientes pasos:  
  
-   Determinar el costo asociado a esa ruta de acceso de replicación. El KCC utiliza coste para determinar la ruta menos costosa para la replicación entre dos sitios que se replican en la misma partición de directorio.  
  
-   Puede producirse la determinación de la programación que define los períodos de tiempo durante el que la replicación entre sitios.  
  
-   Determinar el intervalo de replicación que se define con qué frecuencia debe realizarse la replicación durante las horas cuando se permite la replicación, como se define en la programación.  
  
## <a name="in-this-guide"></a>En esta guía  
  
-   [Determinar el costo](../../ad-ds/plan/Determining-the-Cost.md)  
  
-   [Determinación de la programación](../../ad-ds/plan/Determining-the-Schedule.md)  
  
-   [Determinación del intervalo](../../ad-ds/plan/Determining-the-Interval.md)  
  


