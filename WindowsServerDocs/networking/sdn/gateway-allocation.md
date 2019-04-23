---
title: Asignación de ancho de banda de puerta de enlace
description: ''
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: 3c259b96e1a8ee27888a5cccc50b285a2f7cb8c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850436"
---
# <a name="gateway-bandwidth-allocation"></a>Asignación de ancho de banda de puerta de enlace

>Se aplica a: Windows Server

En Windows Server 2016, el ancho de banda individuales de túnel IPsec, GRE y L3 era una proporción de la capacidad total de la puerta de enlace. Por lo tanto, los clientes proporcionaría la capacidad de puerta de enlace según el ancho de banda TCP estándar espera esto fuera de la máquina virtual de puerta de enlace.

Además, estaba limitado a (3/20) máximo ancho de banda de túnel de IPsec en la puerta de enlace\*capacidad de puerta de enlace proporcionado por el cliente. Por lo tanto, por ejemplo, si establece la capacidad de puerta de enlace a 100 Mbps, a continuación, la capacidad de túnel IPsec sería 150 Mbps. Las proporciones equivalentes para túneles GRE y L3 son 1/5 y 1/2, respectivamente.

Aunque esto funcionaba para la mayoría de las implementaciones, el modelo de relación fija no era apropiado para entornos de alto rendimiento. Cuando las tasas de transferencia de datos eran altas (es decir, una cantidad superior a 40 Gbps), el rendimiento máximo de túneles de puerta de enlace SDN limitada debido a factores internos.

En Windows Server 2019, para un tipo de túnel, el rendimiento máximo que se ha corregido:

-   IPsec = 5 GB/s

-   GRE = 15 Gbps

-   L3 = 5 Gbps

Por lo tanto, incluso si el host o VM de puerta de enlace admite NIC con mucho un mayor rendimiento, el rendimiento máximo de túnel disponibles se ha corregido. Otro problema que esto se encarga de es arbitrariamente aprovisionamiento en exceso las puertas de enlace, lo que sucede al proporcionar un número muy elevado de la capacidad de puerta de enlace.

## <a name="gateway-capacity-calculation"></a>Cálculo de la capacidad de puerta de enlace

Idealmente, establezca la capacidad de procesamiento de la puerta de enlace en el rendimiento disponible para la máquina virtual de puerta de enlace. Por lo tanto, por ejemplo, si tiene una puerta de enlace única máquina virtual y el rendimiento de la NIC de host subyacente es 25 Gbps, el rendimiento de la puerta de enlace se puede establecer a 25 Gbps también.

Si usa una puerta de enlace solo para las conexiones IPsec, la capacidad fija máxima disponible es 5 Gbps. Por lo tanto, por ejemplo, si se aprovisiona las conexiones IPsec en la puerta de enlace, solo se pueden aprovisionar un ancho de banda (entrante y saliente) como 5 Gbps.

Si usa la puerta de enlace para la conectividad de IPsec y GRE, puede aprovisionar el rendimiento máximo de 5 GB/s de IPsec o el rendimiento máximo de 15 GB/s de GRE. Por lo tanto, por ejemplo, si se aprovisiona el rendimiento de 2 Gbps de IPsec, tiene 3 rendimiento Gbps de IPsec encendidos para aprovisionar la puerta de enlace o 9 rendimiento Gbps de GRE izquierda.

Para poner esto en términos matemáticos más:

- Total de capacidad de la puerta de enlace = 25 Gbps

- Total de capacidad disponible de IPsec = 5 Gbps (fijo)

- Total de capacidad disponible de GRE = 15 GB/s (fijo)

- Coeficiente de rendimiento IPsec para esta puerta de enlace = 25/5 = 5 Gbps

- Coeficiente de rendimiento GRE para esta puerta de enlace = 25/15 = 5/3 GB/s

Por ejemplo, si asigna el rendimiento de 2 Gbps de IPsec a un cliente:

Capacidad disponible en la puerta de enlace restante = capacidad Total de la puerta de enlace: proporción de rendimiento IPsec * rendimiento IPsec asignada (capacidad usada)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;25-5 * 2 = 15 GB/s

Rendimiento de IPsec restante que se puede asignar en la puerta de enlace 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5-2 = 3 GB/s

Rendimiento GRE restante que se puede asignar en la puerta de enlace = capacidad restante de coeficiente de rendimiento de la puerta de enlace/GRE 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;15 * 3/5 = 9 GB/s

La proporción de rendimiento varía según la capacidad total de la puerta de enlace. Hay que destacar es que debe establecer la capacidad total para el ancho de banda TCP disponible para la máquina virtual de puerta de enlace. Si tiene varias máquinas virtuales hospedadas en la puerta de enlace, debe ajustar la capacidad total de la puerta de enlace según corresponda.

Además, si la capacidad de puerta de enlace es menor que la capacidad total de túnel disponibles, la capacidad total de túnel disponible se establece en la capacidad de puerta de enlace. Por ejemplo, si establece la capacidad de puerta de enlace a 4 GB/s, la capacidad total disponible para GRE, IPsec y L3 se establece en 4 Gbps, dejando la proporción de rendimiento para cada túnel a 1 Gbps.

## <a name="windows-server-2016-behavior"></a>Comportamiento de Windows Server 2016

El algoritmo de cálculo de la capacidad de puerta de enlace de Windows Server 2016 permanece sin cambios. En Windows Server 2016, estaba limitado a (3/20) ancho de banda de túnel IPsec máximo\*capacidad de puerta de enlace en una puerta de enlace. Las proporciones equivalentes para túneles GRE y L3 son 1/5 y 1/2, respectivamente.

Si va a actualizar desde Windows Server 2016 para Windows Server 2019:

1.  **Túneles GRE y L3:** La lógica de asignación de Windows Server 2019 surte efecto una vez que los nodos de controladora de red se actualizaran a Windows Server 2019

2.  **Túneles IPSec:** La lógica de asignación de puerta de enlace de Windows Server 2016 continúa funcionando hasta que todas las puertas de enlace en el grupo de puerta de enlace actualizarse a Windows Server 2019. Para todas las puertas de enlace en el grupo de puerta de enlace, debe establecer el servicio de puerta de enlace de Azure en **automática**.

>[!NOTE]
>Es posible que tras actualizar a Windows Server 2019, una puerta de enlace se convierte en un aprovisionamiento excesivo (como la lógica de asignación cambia de Windows Server 2016 para Windows Server 2019). En este caso, las conexiones existentes en la puerta de enlace continúan existiendo. El recurso REST para la puerta de enlace produce una advertencia de que la puerta de enlace es un aprovisionamiento excesivo. En este caso, debe mover algunas conexiones a otra puerta de enlace.

---