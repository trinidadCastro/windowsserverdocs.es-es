---
ms.assetid: 5b5bab7a-727b-47ce-8efa-1d37a9639cba
title: Análisis en profundidad de equilibrio de carga de máquinas virtuales
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: cebdc8c192abd737478c3b7a0c3db3e4a2bc8091
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87957152"
---
# <a name="virtual-machine-load-balancing-deep-dive"></a>Análisis en profundidad de equilibrio de carga de máquinas virtuales

> Se aplica a: Windows Server 2019, Windows Server 2016

La [característica de equilibrio de carga de la máquina virtual](vm-load-balancing-overview.md) optimiza el uso de los nodos en un clúster de conmutación por error. En este documento se describe cómo configurar y controlar el equilibrio de carga de máquinas virtuales.

## <a name="heuristics-for-balancing"></a><a id="heuristics-for-balancing"></a>Heurística de equilibrio
El equilibrio de carga de máquinas virtuales evalúa la carga de un nodo en función de la heurística siguiente:
1. **Presión de memoria**actual: la memoria es la restricción de recursos más común en un host de Hyper-V
2. **Uso** de CPU del nodo en promedio en un período de 5 minutos: mitiga un nodo en el clúster que se está sobrecargando

## <a name="controlling-the-aggressiveness-of-balancing"></a><a id="controlling-aggressiveness-of-balancing"></a>Controlar la agresividad del equilibrio
La agresividad del equilibrio basado en la heurística de memoria y CPU se puede configurar mediante la propiedad común del clúster "AutoBalancerLevel". Para controlar la agresividad, ejecute lo siguiente en PowerShell:

```PowerShell
(Get-Cluster).AutoBalancerLevel = <value>
```

| AutoBalancerLevel | Agresividad | Comportamiento |
|-------------------|----------------|----------|
| 1 (predeterminado) | Bajo | Cambiar cuando el host supera el 80% de carga |
| 2 | Media | Cambiar cuando el host supera el 70% de carga |
| 3 | Alto | Promedio de nodos y movimiento cuando el host supera el 5% por encima del promedio |

![Gráfico de un PowerShell de configuración de la agresividad del equilibrio](media/vm-load-balancing/detailed-VM-load-balancing-1.jpg)

## <a name="controlling-vm-load-balancing"></a>Controlar el equilibrio de carga de máquinas virtuales
El equilibrio de carga de máquina virtual está habilitado de forma predeterminada y, cuando se produce el equilibrio de carga, la propiedad común del clúster "AutoBalancerMode" puede configurarla. Para controlar cuándo el nodo equidad equilibra el clúster:

### <a name="using-failover-cluster-manager"></a>Usar Administrador de clústeres de conmutación por error:
1. Haga clic con el botón derecho en el nombre del clúster y seleccione el gráfico de la opción "propiedades" ![ de seleccionar propiedad para el clúster a través de administrador de clústeres de conmutación por error](media/vm-load-balancing/detailed-VM-load-balancing-2.jpg)

2.  Seleccione el gráfico del panel "equilibrador" al ![ seleccionar la opción de equilibrador a través de administrador de clústeres de conmutación por error](media/vm-load-balancing/detailed-VM-load-balancing-3.jpg)

### <a name="using-powershell"></a>Con PowerShell:
Ejecute lo siguiente:
```powershell
(Get-Cluster).AutoBalancerMode = <value>
```

|AutoBalancerMode |Comportamiento|
|:----------------:|:----------:|
|0| Disabled|
|1| Equilibrio de carga en la combinación de nodos|
|2 (predeterminado)| Equilibrio de carga en la Unión de nodo y cada 30 minutos |

## <a name="vm-load-balancing-vs-system-center-virtual-machine-manager-dynamic-optimization"></a>Equilibrio de carga de VM frente a System Center Virtual Machine Manager optimización dinámica
La característica de equidad de nodos, proporciona funcionalidad integrada, orientada a las implementaciones sin System Center Virtual Machine Manager (SCVMM). La optimización dinámica de SCVMM es el mecanismo recomendado para equilibrar la carga de la máquina virtual en el clúster para las implementaciones de SCVMM. SCVMM deshabilita automáticamente el equilibrio de carga de la máquina virtual de Windows Server cuando está habilitada la optimización dinámica.

## <a name="see-also"></a>Consulte también
* [Introducción al equilibrio de carga de máquinas virtuales](vm-load-balancing-overview.md)
* [Clústeres de conmutación por error](failover-clustering-overview.md)
* [Introducción a Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
