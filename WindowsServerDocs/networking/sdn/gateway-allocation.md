---
title: Asignación de ancho de banda de puerta de enlace
description: ''
manager: dougkim
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: fc59fc9d7dc22b9c5567bae314b4312d76fcff19
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406122"
---
# <a name="gateway-bandwidth-allocation"></a>Asignación de ancho de banda de puerta de enlace

>Se aplica a: Windows Server

En Windows Server 2016, el ancho de banda de túnel individual para IPsec, GRE y L3 era una proporción de la capacidad total de la puerta de enlace. Por lo tanto, los clientes proporcionarían la capacidad de puerta de enlace basada en el ancho de banda TCP estándar que esperaba esta máquina virtual de puerta de enlace.

Además, el ancho de banda de túnel IPsec máximo en la puerta de enlace se limitó a (3/20) la capacidad de \*Gateway proporcionada por el cliente. Por lo tanto, por ejemplo, si establece la capacidad de la puerta de enlace en 100 Mbps, la capacidad del túnel IPsec sería de 150 Mbps. Las proporciones equivalentes para los túneles GRE y L3 son 1/5 y 1/2, respectivamente.

Aunque esto funcionaba para la mayoría de las implementaciones, el modelo de proporción fija no era adecuado para entornos de alto rendimiento. Incluso cuando las tasas de transferencia de datos eran altas (por ejemplo, más de 40 Gbps), el rendimiento máximo de los túneles de puerta de enlace de SDN se limita debido a factores internos.

En Windows Server 2019, para un tipo de túnel, el rendimiento máximo es fijo:

-   IPsec = 5 Gbps

-   GRE = 15 Gbps

-   L3 = 5 Gbps

Por lo tanto, incluso si el host o la máquina virtual de la puerta de enlace admiten NIC con un rendimiento mucho mayor, se fija el rendimiento máximo del túnel disponible. Otro problema que se encarga de ello es arbitrariamente las puertas de enlace en exceso de aprovisionamiento, lo que sucede cuando se proporciona un número muy alto para la capacidad de la puerta de enlace.

## <a name="gateway-capacity-calculation"></a>Cálculo de la capacidad de puerta de enlace

Idealmente, establezca la capacidad de rendimiento de la puerta de enlace en el rendimiento disponible para la máquina virtual de puerta de enlace. Por lo tanto, por ejemplo, si tiene una sola máquina virtual de puerta de enlace y el rendimiento de la NIC de host subyacente es de 25 Gbps, el rendimiento de la puerta de enlace también se puede establecer en 25 Gbps.

Si usa una puerta de enlace solo para las conexiones IPsec, el máximo de capacidad fija disponible es de 5 Gbps. Por lo tanto, por ejemplo, si aprovisiona conexiones IPsec en la puerta de enlace, solo puede aprovisionar en un ancho de banda agregado (entrante y saliente) como 5 Gbps.

Si usa la puerta de enlace para la conectividad de IPsec y GRE, puede aprovisionar un máximo de 5 Gbps de rendimiento de IPsec o un máximo de 15 Gbps de rendimiento de GRE. Por lo tanto, por ejemplo, si aprovisiona 2 Gbps de rendimiento de IPsec, tiene 3 Gbps de rendimiento de IPsec para aprovisionar en la puerta de enlace o 9 Gbps de rendimiento GRE restante.

Para colocarlo en términos más matemáticos:

- Capacidad total de la puerta de enlace = 25 Gbps

- Capacidad total de IPsec disponible = 5 Gbps (fijo)

- Capacidad GRE total disponible = 15 Gbps (fijo)

- Relación de rendimiento de IPsec para esta puerta de enlace = 25/5 = 5 Gbps

- Proporción de rendimiento GRE para esta puerta de enlace = 25/15 = 5/3 Gbps

Por ejemplo, si asigna 2 Gbps de rendimiento de IPsec a un cliente:

Capacidad disponible restante en la puerta de enlace = capacidad total de la puerta de enlace: proporción de rendimiento de IPsec * rendimiento de IPsec asignado (capacidad usada)

&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-525 – 5 * 2 = 15 Gbps

Rendimiento de IPsec restante que se puede asignar en la puerta de enlace 

&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-55-2 = 3 Gbps

Rendimiento GRE restante que se puede asignar en la puerta de enlace = capacidad restante de la relación de rendimiento de puerta de enlace/GRE 

&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-515 * 3/5 = 9 Gbps

La proporción de rendimiento varía en función de la capacidad total de la puerta de enlace. Hay que tener en cuenta que debe establecer la capacidad total en el ancho de banda de TCP disponible para la máquina virtual de puerta de enlace. Si tiene varias máquinas virtuales hospedadas en la puerta de enlace, debe ajustar la capacidad total de la puerta de enlace en consecuencia.

Además, si la capacidad de la puerta de enlace es menor que la capacidad total del túnel disponible, la capacidad total del túnel disponible se establece en la capacidad de la puerta de enlace. Por ejemplo, si establece la capacidad de la puerta de enlace en 4 Gbps, la capacidad total disponible para IPsec, L3 y GRE se establece en 4 Gbps, lo que permite que la proporción de rendimiento de cada túnel sea de 1 Gbps.

## <a name="windows-server-2016-behavior"></a>Comportamiento de Windows Server 2016

El algoritmo de cálculo de la capacidad de puerta de enlace para Windows Server 2016 permanece sin cambios. En Windows Server 2016, el ancho de banda de túnel IPsec máximo se limitó a (3/20) la capacidad de \*gateway en una puerta de enlace. Las proporciones equivalentes para los túneles de GRE y L3 eran 1/5 y 1/2, respectivamente.

Si está actualizando de Windows Server 2016 a Windows Server 2019:

1.  **Túneles GRE y L3:** La lógica de asignación de Windows Server 2019 surte efecto una vez que los nodos de la controladora de red se actualizan a Windows Server 2019

2.  **Túneles IPSec:** La lógica de asignación de puerta de enlace de Windows Server 2016 continúa funcionando hasta que todas las puertas de enlace del grupo de puerta de enlace se actualicen a Windows Server 2019. Para todas las puertas de enlace del grupo de puerta de enlace, debe establecer el servicio de puerta de enlace de Azure en **automático**.

>[!NOTE]
>Es posible que después de actualizar a Windows Server 2019, una puerta de enlace se vuelva a aprovisionar (a medida que la lógica de asignación cambie de Windows Server 2016 a Windows Server 2019). En este caso, las conexiones existentes en la puerta de enlace continúan existiendo. El recurso REST para la puerta de enlace genera una advertencia que indica que la puerta de enlace está en exceso de aprovisionamiento. En este caso, debe trasladar algunas conexiones a otra puerta de enlace.

---