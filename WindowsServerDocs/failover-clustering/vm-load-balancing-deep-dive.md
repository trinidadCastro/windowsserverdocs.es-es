---
ms.assetid: 5b5bab7a-727b-47ce-8efa-1d37a9639cba
title: Equilibrio de carga en la máquina virtual en profundidad
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 50213cf47c2c59f1775ae704e82ed51794715ac0
ms.sourcegitcommit: 276a480b470482cba4682caa3df4cd07ba5b7801
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66198553"
---
# <a name="virtual-machine-load-balancing-deep-dive"></a>Equilibrio de carga en la máquina virtual en profundidad

> Se aplica a: Windows Server 2019, Windows Server 2016

El [característica Equilibrio de carga en la máquina Virtual](vm-load-balancing-overview.md) optimiza el uso de nodos de un clúster de conmutación por error. Este documento describe cómo configurar y controlar el equilibrio de carga en la máquina virtual. 

## <a id="heuristics-for-balancing"></a>Heurística para el equilibrio
Equilibrio de carga en la máquina virtual se evalúa como la carga de un nodo en función de la heurística siguiente:
1. Actual **presión de memoria**: La memoria es la restricción de recursos más comunes en un host de Hyper-V
2. CPU **utilización** del nodo promediado de un período de 5 minutos: Mitiga un nodo del clúster que se está convirtiendo en sobrecargado

## <a id="controlling-aggressiveness-of-balancing"></a>Controlar la agresividad de equilibrio
La agresividad de equilibrio en función de la heurística de memoria y CPU se puede configurar mediante la por la propiedad común del clúster 'AutoBalancerLevel'. Para controlar la agresividad ejecute el siguiente comando en PowerShell:

```PowerShell
(Get-Cluster).AutoBalancerLevel = <value>
```

| AutoBalancerLevel | Agresividad | Comportamiento |
|-------------------|----------------|----------|
| 1 (predeterminado) | Bajo | Mover al host es superior al 80% de carga |
| 2 | Medio | Mover al host es más del 70% de carga |
| 3 | Alto | Promedio de nodos y mover al host es más del 5% por encima del promedio | 

![Gráfico de un PowerShell de la configuración de la agresividad de equilibrio](media/vm-load-balancing/detailed-VM-load-balancing-1.jpg)

## <a name="controlling-vm-load-balancing"></a>Controlar el equilibrio de carga de máquina virtual
Equilibrio de carga en la máquina virtual está habilitado de forma predeterminada y cuando se produce equilibrio de carga puede configurarse mediante la propiedad común del clúster 'AutoBalancerMode'. Para controlar cuándo equilibra la imparcialidad de nodo del clúster:

### <a name="using-failover-cluster-manager"></a>Mediante el Administrador de clústeres de conmutación por error:
1. Haga doble clic en el nombre del clúster y seleccione la opción "Propiedades".  
    ![Gráfico de selección de propiedad para el clúster mediante el Administrador de clústeres de conmutación por error](media/vm-load-balancing/detailed-VM-load-balancing-2.jpg)

2.  Seleccione el panel "Equilibrador"  
    ![Gráfico de seleccionar la opción equilibrador mediante el Administrador de clústeres de conmutación por error](media/vm-load-balancing/detailed-VM-load-balancing-3.jpg)

### <a name="using-powershell"></a>Uso de PowerShell:
Ejecute lo siguiente:
```powershell
(Get-Cluster).AutoBalancerMode = <value>
```

|AutoBalancerMode |Comportamiento| 
|:----------------:|:----------:|
|0| Deshabilitada| 
|1| Equilibrar la carga en la combinación de nodo| 
|2 (valor predeterminado)| En la combinación de nodo y cada 30 minutos de equilibrio de carga |

## <a name="vm-load-balancing-vs-system-center-virtual-machine-manager-dynamic-optimization"></a>Equilibrio de carga en la máquina virtual de vs. Optimización de System Center Virtual Machine Manager dinámicos
La característica de imparcialidad de nodo, proporciona la funcionalidad en el equipo que va dirigida a las implementaciones sin System Center Virtual Machine Manager (SCVMM). Optimización dinámica de SCVMM es el mecanismo recomendado para el equilibrio de carga de la máquina virtual en el clúster para las implementaciones de SCVMM. SCVMM deshabilita automáticamente el equilibrio de carga de Windows Server VM cuando está habilitada la optimización dinámica.

## <a name="see-also"></a>Vea también
* [Introducción al equilibrio de carga de máquina virtual](vm-load-balancing-overview.md)
* [Clúster de conmutación por error](failover-clustering-overview.md)
* [Introducción a Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
