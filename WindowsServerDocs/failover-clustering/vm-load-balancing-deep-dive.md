---
ms.assetid: 5b5bab7a-727b-47ce-8efa-1d37a9639cba
title: Análisis en profundidad de equilibrio de carga de máquinas virtuales
ms.prod: windows-server
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 972e86d5f49f92d090eed1d4130544d0269c1309
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392221"
---
# <a name="virtual-machine-load-balancing-deep-dive"></a>Análisis en profundidad de equilibrio de carga de máquinas virtuales

> Se aplica a: Windows Server 2019 y Windows Server 2016

La [característica de equilibrio de carga de la máquina virtual](vm-load-balancing-overview.md) optimiza el uso de los nodos en un clúster de conmutación por error. En este documento se describe cómo configurar y controlar el equilibrio de carga de máquinas virtuales. 

## <a id="heuristics-for-balancing"></a>Heurística de equilibrio
El equilibrio de carga de máquinas virtuales evalúa la carga de un nodo en función de la heurística siguiente:
1. **Presión de memoria**actual: La memoria es la restricción de recursos más común en un host de Hyper-V
2. **Uso** de CPU del nodo de promedio en un período de 5 minutos: Mitiga un nodo del clúster que se está confirmando

## <a id="controlling-aggressiveness-of-balancing"></a>Controlar la agresividad del equilibrio
La agresividad del equilibrio basado en la heurística de memoria y CPU se puede configurar mediante la propiedad común del clúster "AutoBalancerLevel". Para controlar la agresividad, ejecute lo siguiente en PowerShell:

```PowerShell
(Get-Cluster).AutoBalancerLevel = <value>
```

| AutoBalancerLevel | Agresividad | Comportamiento |
|-------------------|----------------|----------|
| 1 (predeterminado) | Bajo | Cambiar cuando el host supera el 80% de carga |
| 2 | Medio | Cambiar cuando el host supera el 70% de carga |
| 3 | Alto | Promedio de nodos y movimiento cuando el host supera el 5% por encima del promedio | 

![Gráfico de un PowerShell de configuración de la agresividad del equilibrio](media/vm-load-balancing/detailed-VM-load-balancing-1.jpg)

## <a name="controlling-vm-load-balancing"></a>Controlar el equilibrio de carga de máquinas virtuales
El equilibrio de carga de máquina virtual está habilitado de forma predeterminada y, cuando se produce el equilibrio de carga, la propiedad común del clúster "AutoBalancerMode" puede configurarla. Para controlar cuándo el nodo equidad equilibra el clúster:

### <a name="using-failover-cluster-manager"></a>Usar Administrador de clústeres de conmutación por error:
1. Haga clic con el botón derecho en el nombre del clúster y seleccione la opción "propiedades".  
    ![Gráfico de selección de propiedad para el clúster a través de Administrador de clústeres de conmutación por error](media/vm-load-balancing/detailed-VM-load-balancing-2.jpg)

2.  Seleccionar el panel "equilibrador"  
    ![Gráfico de selección de la opción de equilibrador a través de Administrador de clústeres de conmutación por error](media/vm-load-balancing/detailed-VM-load-balancing-3.jpg)

### <a name="using-powershell"></a>Con PowerShell:
Ejecute lo siguiente:
```powershell
(Get-Cluster).AutoBalancerMode = <value>
```

|AutoBalancerMode |Comportamiento| 
|:----------------:|:----------:|
|0| Deshabilitada| 
|1| Equilibrio de carga en la combinación de nodos| 
|2 (valor predeterminado)| Equilibrio de carga en la Unión de nodo y cada 30 minutos |

## <a name="vm-load-balancing-vs-system-center-virtual-machine-manager-dynamic-optimization"></a>Equilibrio de carga de VM frente a Optimización dinámica System Center Virtual Machine Manager
La característica de equidad de nodos, proporciona funcionalidad integrada, orientada a las implementaciones sin System Center Virtual Machine Manager (SCVMM). La optimización dinámica de SCVMM es el mecanismo recomendado para equilibrar la carga de la máquina virtual en el clúster para las implementaciones de SCVMM. SCVMM deshabilita automáticamente el equilibrio de carga de la máquina virtual de Windows Server cuando está habilitada la optimización dinámica.

## <a name="see-also"></a>Vea también
* [Introducción al equilibrio de carga de máquinas virtuales](vm-load-balancing-overview.md)
* [Clúster de conmutación por error](failover-clustering-overview.md)
* [Introducción a Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
