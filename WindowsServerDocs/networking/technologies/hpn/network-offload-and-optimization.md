---
title: Descarga de la red y tecnologías de optimización
description: En este tema proporciona información general sobre la descarga y las tecnologías de optimización en Windows Server 2016 e incluye vínculos a guías adicionales acerca de estas tecnologías.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/20/2018
ms.openlocfilehash: 9cd63ae557eab6e8e4f69fedd20619d4e7af9184
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847856"
---
# <a name="network-offload-and-optimization-technologies"></a>Descarga de la red y tecnologías de optimización

En este tema, nos ofrecerle una visión general de las características de descarga y la optimización de red diferentes disponibles en Windows Server 2016 y comentar cómo ayudar a que las redes más eficaz. Estas tecnologías incluyen las características de solo Software (SO) y las tecnologías de Software y Hardware (SH) integra las características y tecnologías y características sólo Hardware (HO) y tecnologías.

Las tres categorías de características de redes en Windows Server 2016 son: 

1.  [Solo de software (SO) las características y tecnologías](hpn-software-only-features.md): Estas características se implementan como parte del sistema operativo y son independientes de la NIC subyacente. A veces, estas características requerirán algún ajuste de la NIC para un funcionamiento óptimo. Estos ejemplos de características de Hyper-v como vmQoS, las ACL y las características de Hyper-V como formación de equipos NIC.   

2.  [Software y Hardware (SH) integran las características y tecnologías](hpn-software-hardware-features.md): Estas características tienen componentes de hardware y software. El software está estrechamente vinculado a las capacidades de hardware que son necesarias para que funcione la característica. Ejemplos de estas incluyen VMMQ, VMQ, la descarga de suma de comprobación de IPv4 de envío y RSS.   

3.  [Tecnologías y características de hardware sólo (HO)](hpn-hardware-only-features.md): Estos aceleración de hardware, mejora el rendimiento de red junto con el software pero íntimamente no forman parte de cualquier característica de software. Ejemplos de estas incluyen la moderación de interrupciones, flujo de Control y la descarga de suma de comprobación de IPv4 de lado de recepción. 

4. [Propiedades avanzadas de NIC](hpn-nic-advanced-properties.md): Puede administrar las NIC y todas las características a través de PowerShell de Windows mediante el cmdlet NetAdapter.  También puede administrar las NIC y todas las características mediante el Panel de Control de red (ncpa.cpl). 

>[!TIP]
>- Por lo que las características y tecnologías están disponibles en todas las arquitecturas de hardware, independientemente de la velocidad de NIC o capacidades NIC.
>
>- SH y HO características solo están disponibles cuando el adaptador de red es compatible con las características o tecnologías.

---