---
title: Preguntas más frecuentes sobre QoS
description: En este tema se proporcionan respuestas a preguntas acerca de la Directiva de calidad de servicio (QoS) en Windows Server 2016.
ms.topic: article
ms.assetid: 74c97a14-b957-4568-b48e-8963a674fdb3
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f992aa4d538e5af2feefa353393154be5e797409
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953861"
---
# <a name="qos-policy-frequently-asked-questions"></a>Preguntas más frecuentes sobre la directiva QoS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

A continuación se muestran las preguntas más frecuentes, así como respuestas a estas preguntas: para la Directiva de QoS.

1.  **¿Qué sistema operativo debe ejecutar el controlador de dominio para usar la Directiva de QoS?**

     Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008

2.  **¿Qué sistemas operativos admiten la aplicación de la Directiva de QoS al usuario o al equipo?**

     Puede aplicar directivas de QoS a usuarios o equipos que ejecuten Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 y Windows Vista.

3.  **¿Se aplican las directivas de QoS al remitente o al receptor del tráfico?**

     Las directivas de QoS deben aplicarse en el equipo de envío para que afecten al tráfico saliente. Para afectar al tráfico bidireccional de dos equipos, es necesario aplicar las directivas de QoS a ambos equipos.

4.  **¿Qué ocurre si las directivas QoS en conflicto se implementan en el mismo equipo?**

     Si se aplican varias directivas, la directiva QoS más específica tiene prioridad. Por ejemplo, se aplica una directiva que indica una dirección de host (192.168.4.12) en lugar de una dirección de red menos específica (192.168.0.0/16). Si una directiva de nivel de equipo y de nivel de usuario tienen la misma especificidad, se aplica la directiva QoS de nivel de usuario en lugar de la directiva QoS de nivel de equipo.

5.  **¿Está habilitada de forma predeterminada la directiva QoS?**

     No, la directiva QoS no está habilitada de forma predeterminada. Debe crear directivas de QoS manualmente para habilitar QoS.  Para obtener más información, vea [administrar la directiva QoS](qos-policy-manage.md).

En el primer tema de esta guía, vea [Directiva de calidad de servicio (QoS)](qos-policy-top.md).
