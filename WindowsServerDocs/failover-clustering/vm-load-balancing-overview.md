---
ms.assetid: f0d4cecc-5a03-448c-bef9-86c4730b4eb0
title: Introducción al equilibrio de carga de máquina virtual
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 8b8ecee16c778ed26953be325fb88748fc458176
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867756"
---
# <a name="virtual-machine-load-balancing-overview"></a>Introducción al equilibrio de carga de máquina virtual

> Se aplica a: Windows Server (canal semianual), Windows Server 2016

Una consideración importante para las implementaciones de nube privada es el gasto de capital (<abbr title="gastos de capital">Gastos de capital</abbr>) necesarios para pasar a producción. Es muy común para agregar redundancia para las implementaciones de nube privada para evitar el exceso de capacidad durante tráfico pico en producción, pero esto aumenta <abbr title="gastos de capital">Gastos de capital</abbr>. La necesidad de redundancia se controla mediante nubes privadas desequilibradas donde algunos nodos hospedan más máquinas virtuales (<abbr title="máquinas virtuales">Las máquinas virtuales</abbr>) y otros usuarios están infrautilizados (por ejemplo, un servidor recién reiniciado).

<strong>Vídeo introductorio rápido</strong><br>(6 minutos)<br>
> [!VIDEO https://channel9.msdn.com/Blogs/windowsserver/Virtual-Machine-Load-Balancing-in-Windows-Server-2016/player]

## <a id="what-is-vm-load-balancing"></a>¿Qué es el equilibrio de carga en la máquina Virtual?
<abbr title="Máquina virtual">VM</abbr> equilibrio de carga es una nueva característica incluida en Windows Server 2016 que permite optimizar el uso de nodos de un clúster de conmutación por error. Identifica nodos sobrecargados y volver a distribuye <abbr title="máquinas virtuales">Las máquinas virtuales</abbr> de dichos nodos a los nodos bajo confirmada. Algunos de los aspectos importantes de esta característica son los siguientes:

* *Es una solución sin tiempo de inactividad*: <abbr title="Máquinas virtuales">Las máquinas virtuales</abbr> son migran en vivo a nodos inactivos.
* *Perfecta integración con el entorno de clúster existente*: Se respetan las directivas de error como antiafinidad, dominios de error y posibles propietarios.
* *Heurística para el equilibrio*: <abbr title="Máquina virtual">VM</abbr> presión de memoria y uso de CPU del nodo.
* *Control granular*: Opción habilitada de forma predeterminada. Se puede activar a petición o en un intervalo periódico.
* *Los umbrales de agresividad*: Tres umbrales disponibles según las características de la implementación.

## <a id="feature-in-action"></a>La característica en acción
### <a id="new-node-added"></a>Se agrega un nuevo nodo al clúster de conmutación por error
![Gráfico de un nuevo nodo que se agrega a su clúster de conmutación por error](media/vm-load-balancing/overview-VM-load-balancing-1.png)

Al agregar una nueva capacidad para el clúster de conmutación por error, el <abbr title="máquina virtual">VM</abbr> característica Equilibrio de carga equilibra automáticamente la capacidad de los nodos existentes, para el nodo recién agregado en el orden siguiente:

1. Se evalúa la presión en los nodos existentes en el clúster de conmutación por error.
2. Se identifican todos los nodos que supere el umbral.
3. Se identifican los nodos con la presión más alto para determinar la prioridad de equilibrio.
4. <abbr title="Máquinas virtuales">Las máquinas virtuales</abbr> son migrar en vivo (sin ningún tiempo de inactividad) desde un nodo cuando se supera el umbral para un nodo recién agregado en el clúster de conmutación por error.

### <a id="recurring-load-balancing"></a>Periódicas de equilibrio de carga
![Gráfico de una máquina virtual periódica equilibrio de carga](media/vm-load-balancing/overview-VM-load-balancing-2.png)

Cuando se configura para el equilibrio periódica, se evalúa la presión en los nodos del clúster de equilibrio cada 30 minutos. Como alternativa, la presión puede ser evaluado y a petición. Este es el flujo de pasos:

1. La presión se evalúa en todos los nodos en la nube privada.
2. Se identifican todos los nodos que se supere el umbral y aquellos por debajo del umbral.
3. Se identifican los nodos con la presión más alto para determinar la prioridad de equilibrio.
4. <abbr title="Máquinas virtuales">Las máquinas virtuales</abbr> son migrar en vivo (sin ningún tiempo de inactividad) desde un nodo cuando se supera el umbral al nodo debajo del umbral mínimo.

## <a name="see-also"></a>Vea también
* [Profundidad de equilibrio de carga de máquina virtual](vm-load-balancing-deep-dive.md)
* [Agrupación en clústeres de conmutación por error](failover-clustering-overview.md)
* [Introducción a Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
