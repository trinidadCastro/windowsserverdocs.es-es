---
ms.assetid: f0d4cecc-5a03-448c-bef9-86c4730b4eb0
title: "Información general de equilibrio de carga de máquina virtual"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 0a106db25d476088898b914481e6041f20ce2e9e
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="virtual-machine-load-balancing-overview"></a>Información general de equilibrio de carga de máquina virtual

> Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Un aspecto fundamental en las implementaciones de nube privada es la capital de gastos (<abbr title="gastos de capital">Capital</abbr>) necesarios para ir a producción. Es muy común para agregar redundancia a las implementaciones de nube privada para evitar la capacidad de debajo del durante tráfico máximo en producción, pero esto aumenta <abbr title="gastos de capital">Capital</abbr>. La necesidad de redundancia está controlada por no equilibradas nubes privadas, donde algunos nodos aloja más máquinas virtuales (<abbr title="máquinas virtuales">Máquinas virtuales</abbr>) u otros son poco utilizados (por ejemplo, un servidor recién reiniciado).

## <a id="what-is-vm-load-balancing"></a>¿Qué es el equilibrio de carga de máquina Virtual?
<abbr title="Máquina virtual">VM</abbr> equilibrio de carga es una nueva característica de integrados en Windows Server 2016 que permite optimizar el uso de los nodos de un clúster de conmutación por error. Identifica los nodos exceso comprometidos y vuelven a distribuir <abbr title="máquinas virtuales">Máquinas virtuales</abbr> de los nodos a los nodos bajo confirmada. Algunos de los aspectos destacados de esta característica son los siguientes:

* *Es una solución sin interrupción*: <abbr title="máquinas virtuales">Máquinas virtuales</abbr> son live-migra a los nodos de inactividad.
* *Integración completa con el entorno de clúster existente*: se aceptan las directivas de error como anti- afinidad, dominios de error y posibles propietarios.
* *Heurística equilibrio*: <abbr title="Máquina Virtual">VM</abbr> presión de memoria y uso de CPU del nodo.
* *Control pormenorizado*: habilitada de manera predeterminada. Se puede activar a petición o en un intervalo periódico.
* *Los umbrales de agresividad*: tres umbrales disponibles se basan en las características de la implementación.

## <a id="feature-in-action"></a>La característica en acción
### <a id="new-node-added"></a>Se agrega un nodo nuevo al clúster de conmutación por error
![Gráfico de un nuevo nodo que se agregan al clúster de conmutación por error](media/vm-load-balancing/overview-VM-load-balancing-1.png)

Cuando se agrega nueva capacidad al clúster de conmutación por error, la <abbr title="máquina virtual">VM</abbr> característica Equilibrio de carga automáticamente Equilibrio entre la capacidad de los nodos existentes, en el nodo recién agregado en el siguiente orden:

1. Se evalúa la presión en los nodos del clúster de conmutación por error existentes.
2. Se identifican todos los nodos que superen el umbral.
3. Los nodos con la presión más alto se identifican para determinar la prioridad de equilibrio.
4. <abbr title="Máquinas virtuales">Máquinas virtuales</abbr> son Live migrado (con no tiempo de inactividad) desde un nodo supera el umbral a un nodo del clúster de conmutación por error recién agregado.

### <a id="recurring-load-balancing"></a>Equilibrio de carga de periódica
![Gráfico de un periódico VM de equilibrio de carga](media/vm-load-balancing/overview-VM-load-balancing-2.png)

Cuando se configura para equilibrio periódico, se evalúa la presión en los nodos del clúster de equilibrio de cada 30 minutos. Como alternativa, la presión puede evaluar a petición. Este es el flujo de los pasos:

1. Se evalúa la presión en todos los nodos de la nube privada.
2. Se identifican todos los nodos que se supere el umbral y aquellos por debajo del umbral.
3. Los nodos con la presión más alto se identifican para determinar la prioridad de equilibrio.
4. <abbr title="Máquinas virtuales">Máquinas virtuales</abbr> son Live migrado (con no tiempo de inactividad) desde un nodo supera el umbral al nodo bajo umbral mínimo.

## <a name="see-also"></a>Consulta también
* [Análisis más profundo de equilibrio de carga de máquina virtual](vm-load-balancing-deep-dive.md)
* [Clústeres de conmutación por error](failover-clustering-overview.md)
* [Información general de Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
