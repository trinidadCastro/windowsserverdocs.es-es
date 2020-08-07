---
ms.assetid: de054ac2-a386-43ec-a537-c0de21549741
title: Establecer las propiedades de vínculo de sitio
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: e576d28048a7ad1a5d2bc53d692ef9b02a6a7ebd
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87945595"
---
# <a name="setting-site-link-properties"></a>Establecer las propiedades de vínculo de sitio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La replicación entre sitios se produce de acuerdo con las propiedades de los objetos de conexión. Cuando el comprobador de coherencia de la información (KCC) crea objetos de conexión, deriva la programación de replicación de las propiedades de los objetos de vínculo a sitios. Cada objeto de vínculo a sitios representa la conexión de red de área extensa (WAN) entre dos o más sitios.

La configuración de las propiedades del objeto de vínculo de sitio incluye los pasos siguientes:

-   Determinar el costo asociado a esa ruta de replicación. El KCC usa el costo para determinar la ruta menos costosa para la replicación entre dos sitios que replican la misma partición de directorio.

-   Determinar la programación que define los momentos en los que se puede producir la replicación entre sitios.

-   Determinar el intervalo de replicación que define la frecuencia con que se debe producir la replicación durante las horas en que se permite la replicación, tal como se define en la programación.

## <a name="in-this-guide"></a>En esta guía

-   [Determinar el costo](../../ad-ds/plan/Determining-the-Cost.md)

-   [Determinar la programación](../../ad-ds/plan/Determining-the-Schedule.md)

-   [Determinar el intervalo](../../ad-ds/plan/Determining-the-Interval.md)



