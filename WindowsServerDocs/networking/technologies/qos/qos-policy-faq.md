---
title: Preguntas más frecuentes sobre la calidad de servicio
description: Este tema proporciona respuestas a preguntas acerca de la directiva de calidad de servicio (QoS) en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 74c97a14-b957-4568-b48e-8963a674fdb3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f9fb54cc3bda9a259188ae02fb2ef2836dd8718d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833756"
---
# <a name="qos-policy-frequently-asked-questions"></a>Preguntas más frecuentes sobre la directiva de QoS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Siguiente es frecuentes preguntas y respuestas a estas preguntas: para la directiva de QoS.
  
1.  **¿Qué sistema operativo necesita estar ejecutándose para poder usar la directiva de QoS mi controlador de dominio?**
  
     Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008

2.  **¿Qué sistemas operativos admite la aplicación de directiva de QoS para el usuario o equipo?**

     Puede aplicar las directivas de QoS a usuarios o equipos que ejecutan Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 y Windows Vista.

3.  **¿Se aplican las directivas de QoS para el remitente o receptor de tráfico?**

     Las directivas de QoS se deben aplicar en el equipo de envío para influir en su tráfico saliente. Para afectar el tráfico bidireccional de dos equipos, las directivas de QoS que se aplica a ambos equipos.

4.  **¿Qué ocurre si se implementan directivas de QoS en conflicto en el mismo equipo?**  
  
     Si se aplican varias directivas, la directiva de QoS más específica tiene prioridad. Por ejemplo, se aplica una directiva que indica una dirección de host (192.168.4.12) en lugar de una dirección de red menos específica (192.168.0.0/16). Si una directiva de nivel de usuario y equipo tiene el mismo grado de especificidad, se aplica la directiva de QoS de nivel de usuario en lugar de la directiva QoS de nivel de equipo. 

5.  **¿Directiva QoS de forma predeterminada está habilitado?**

     No, la directiva de QoS no está habilitada de forma predeterminada. Debe crear las directivas de QoS manualmente para habilitar la calidad de servicio.  Para obtener más información, consulte [administrar Directiva de QoS](qos-policy-manage.md).

Para el primer tema de esta guía, consulte [directiva de calidad de servicio (QoS)](qos-policy-top.md).
