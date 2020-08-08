---
title: Descarga de la red y tecnologías de optimización
description: En este tema se proporciona información general sobre las tecnologías de descarga y optimización en Windows Server 2016, e incluye vínculos a instrucciones adicionales sobre estas tecnologías.
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/20/2018
ms.openlocfilehash: e6f53fa8462a51e23fa9fc00aad89d8a8a3ee445
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947009"
---
# <a name="network-offload-and-optimization-technologies"></a>Descarga de la red y tecnologías de optimización

En este tema se proporciona información general sobre las diferentes características de descarga y optimización de red disponibles en Windows Server 2016 y se explica cómo ayudan a mejorar la eficacia de las redes. Estas tecnologías incluyen solo software (SO) características y tecnologías, características y tecnologías integradas de software y hardware (SH), y tecnologías y características de hardware únicamente (HO).

Las tres categorías de características de red disponibles en Windows Server 2016 son:

1.  [Características y tecnologías de solo software (SO)](hpn-software-only-features.md): estas características se implementan como parte del sistema operativo y son independientes de las NIC subyacentes. A veces, estas características requerirán cierto ajuste de la NIC para una operación óptima. Entre los ejemplos se incluyen características de Hyper-v como vmQoS, ACLs y características que no son de Hyper-V como la formación de equipos NIC.

2.  [Características y tecnologías integradas de software y hardware (SH)](hpn-software-hardware-features.md): estas características tienen componentes de software y hardware. El software está estrechamente ligado a las capacidades de hardware necesarias para que funcione la característica. Entre los ejemplos se incluyen VMMQ, VMQ, descarga de suma de comprobación IPv4 del lado de envío y RSS.

3.  [Características y tecnologías de hardware únicamente (Ho)](hpn-hardware-only-features.md): estas aceleraciones de hardware mejoran el rendimiento de la red junto con el software, pero no forman parte de forma íntima de ninguna característica de software. Algunos ejemplos son moderación de interrupciones, control de flujo y descarga de suma de comprobación IPv4 del lado de recepción.

4. [Propiedades avanzadas de NIC](hpn-nic-advanced-properties.md): puede administrar NIC y todas las características a través de Windows PowerShell mediante el cmdlet NetAdapter.  También puede administrar las NIC y todas las características mediante el panel de control de red (ncpa.cpl).

>[!TIP]
>- Por tanto, las características y tecnologías están disponibles en todas las arquitecturas de hardware, independientemente de la velocidad de la NIC o las capacidades de NIC.
>
>- Las características SH y HO solo están disponibles cuando el adaptador de red es compatible con las características o tecnologías.

---