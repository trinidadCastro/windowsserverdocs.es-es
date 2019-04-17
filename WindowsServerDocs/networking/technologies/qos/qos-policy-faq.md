---
title: Preguntas más frecuentes sobre QoS
description: Este tema proporciona respuestas a preguntas acerca de la directiva de calidad de servicio (QoS) en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 74c97a14-b957-4568-b48e-8963a674fdb3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e63cd00a2016411e9f2c532c0e4c301dabdfd816
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-frequently-asked-questions"></a>Preguntas más frecuentes sobre la directiva de QoS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

A continuación es frecuentes preguntas y respuestas a estas preguntas: para la directiva QoS.
  
1.  **¿Qué sistema operativo tiene el controlador de dominio que se estén ejecutando para usar la directiva de QoS?**
  
     Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008

2.  **¿Qué sistemas operativos compatible con la aplicación de directiva de QoS al usuario o equipo?**

     Puedes aplicar las directivas de QoS a usuarios o equipos que ejecutan Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 y Windows Vista.

3.  **¿Se aplican las directivas de QoS al remitente o receptor de tráfico?**

     Las directivas de QoS deben aplicarse en el equipo de envío para modificar el tráfico saliente. Para afectar el tráfico bidireccional de dos equipos, las directivas de QoS deben aplicarse en ambos equipos.

4.  **¿Qué sucede si se implementan directivas de QoS en conflicto en el mismo equipo?**  
  
     Si aplican varias directivas, la directiva de QoS más específica tiene prioridad. Por ejemplo, una directiva que Estados un host dirección (192.168.4.12) se aplica en lugar de un menos específica red dirección (192.168.0.0/16). Si una directiva de equipo y de nivel de usuario tiene el mismo especificidad, se aplica la directiva de QoS de nivel de usuario en lugar de la directiva de QoS de nivel de equipo. 

5.  **¿Directiva QoS está habilitada de manera predeterminada?**

     No, la directiva de QoS no está habilitada de manera predeterminada. Debes crear directivas de QoS manualmente para habilitar QoS.  Para obtener más información, consulta [administrar la directiva de QoS](qos-policy-manage.md).

El primer tema de esta guía, encontrarás [directiva de calidad de servicio (QoS)](qos-policy-top.md).
