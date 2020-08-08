---
ms.assetid: f0d4cecc-5a03-448c-bef9-86c4730b4eb0
title: Introducción al equilibrio de carga de máquina virtual
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 60d6c1b10840b5e0f673099c71b72033178aeb2d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87957132"
---
# <a name="virtual-machine-load-balancing-overview"></a>Introducción al equilibrio de carga de máquina virtual

> Se aplica a: Windows Server 2019, Windows Server 2016

Una consideración clave para las implementaciones de la nube privada es el gasto de capital (<abbr title="gastos de capital">Ahorros</abbr>) necesario para entrar en producción. Es muy común agregar redundancia a las implementaciones de la nube privada para evitar menos capacidad durante el tráfico máximo en producción, pero esto aumenta <abbr title="gastos de capital">Ahorros</abbr>. La necesidad de redundancia está controlada por nubes privadas desequilibradas en las que algunos nodos hospedan más Virtual Machines (<abbr title="máquinas virtuales">Máquinas virtuales</abbr>) y otros están infrautilizados (por ejemplo, un servidor recién reiniciado).

<strong>Introducción rápida al vídeo</strong><br>(6 minutos)<br>
> [!VIDEO https://channel9.msdn.com/Blogs/windowsserver/Virtual-Machine-Load-Balancing-in-Windows-Server-2016/player]

## <a name="what-is-virtual-machine-load-balancing"></a><a id="what-is-vm-load-balancing"></a>¿Qué es el equilibrio de carga de máquinas virtuales?
<abbr title="Máquina virtual">máquina virtual</abbr> El equilibrio de carga es una característica integrada en Windows Server 2019 y Windows Server 2016 que permite optimizar el uso de los nodos en un clúster de conmutación por error. Identifica los nodos sobrecargados y vuelve a distribuir <abbr title="máquinas virtuales">Máquinas virtuales</abbr> de esos nodos a los nodos que se han confirmado. Algunos de los aspectos destacados de esta característica son los siguientes:

* *Se trata de una solución sin tiempo de inactividad*: <abbr title="Máquinas virtuales">Máquinas virtuales</abbr> se migran en vivo a los nodos inactivos.
* *Integración perfecta con el entorno de clúster existente*: se respetan las directivas de error como la antiafinidad, los dominios de error y los posibles propietarios.
* *Heurística de equilibrio*: <abbr title="Máquina virtual">máquina virtual</abbr> utilización de la CPU y la presión de memoria del nodo.
* *Control granular*: habilitado de forma predeterminada. Se puede activar a petición o a intervalos periódicos.
* *Umbrales de agresividad*: tres umbrales disponibles en función de las características de la implementación.

## <a name="the-feature-in-action"></a><a id="feature-in-action"></a>Característica en acción
### <a name="a-new-node-is-added-to-your-failover-cluster"></a><a id="new-node-added"></a>Se agrega un nuevo nodo al clúster de conmutación por error
![Gráfico de un nuevo nodo que se va a agregar al clúster de conmutación por error](media/vm-load-balancing/overview-VM-load-balancing-1.png)

Cuando se agrega una nueva capacidad al clúster de conmutación por error, el <abbr title="máquina virtual">máquina virtual</abbr> La característica de equilibrio de carga equilibra automáticamente la capacidad de los nodos existentes, al nodo recién agregado en el siguiente orden:

1. La presión se evalúa en los nodos existentes en el clúster de conmutación por error.
2. Se identifican todos los nodos que superen el umbral.
3. Los nodos con la presión más alta se identifican para determinar la prioridad del equilibrio.
4. <abbr title="Máquinas virtuales">Máquinas virtuales</abbr> se migran en vivo (sin tiempo de inactividad) desde un nodo que supere el umbral a un nodo recién agregado en el clúster de conmutación por error.

### <a name="recurring-load-balancing"></a><a id="recurring-load-balancing"></a>Equilibrio de carga recurrente
![Gráfico de un equilibrio de carga de máquina virtual recurrente](media/vm-load-balancing/overview-VM-load-balancing-2.png)

Cuando se configura para el equilibrio periódico, la presión en los nodos de clúster se evalúa para el equilibrio cada 30 minutos. Como alternativa, la presión se puede evaluar a petición. Este es el flujo de los pasos:

1. La presión se evalúa en todos los nodos de la nube privada.
2. Se identifican todos los nodos que superan el umbral y los umbrales siguientes.
3. Los nodos con la presión más alta se identifican para determinar la prioridad del equilibrio.
4. <abbr title="Máquinas virtuales">Máquinas virtuales</abbr> se migran en vivo (sin tiempo de inactividad) desde un nodo que supere el umbral hasta el nodo en el umbral mínimo.

## <a name="see-also"></a>Consulte también
* [Análisis en profundidad de equilibrio de carga de máquinas virtuales](vm-load-balancing-deep-dive.md)
* [Clústeres de conmutación por error](failover-clustering-overview.md)
* [Introducción a Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
