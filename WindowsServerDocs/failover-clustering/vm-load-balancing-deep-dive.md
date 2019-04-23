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
ms.openlocfilehash: f90802a8e77ade0b9f282730e7cb61c73a246018
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853426"
---
# <a name="virtual-machine-load-balancing-deep-dive"></a>Equilibrio de carga en la máquina virtual en profundidad

> Se aplica a: Windows Server (canal semianual), Windows Server 2016

Windows Server 2016 incorpora la [característica Equilibrio de carga en la máquina Virtual](vm-load-balancing-overview.md) para optimizar el uso de nodos de un clúster de conmutación por error. Este documento describe cómo configurar y controlar <abbr title="máquina virtual">VM</abbr> equilibrio de carga. 

## <a id="heuristics-for-balancing"></a>Heurística para el equilibrio
<abbr title="Máquina virtual">VM</abbr> equilibrio de carga en la máquina Virtual se evalúa como la carga de un nodo en función de la heurística siguiente:
1. Actual **presión de memoria**: La memoria es la restricción de recursos más comunes en un host de Hyper-V
2. <abbr title="Unidad central de procesamiento">CPU</abbr> **utilización** del nodo promediado de un período de 5 minutos: Mitiga un nodo del clúster que se está convirtiendo en sobrecargado

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

## <a name="controlling-abbr-titlevirtual-machinevmabbr-load-balancing"></a>Controlar <abbr title="Máquina Virtual">VM</abbr> equilibrio de carga
<abbr title="Máquina virtual">VM</abbr> equilibrio de carga está habilitado de forma predeterminada y se puede configurar cuando se produce equilibrio de carga mediante la propiedad común del clúster 'AutoBalancerMode'. Para controlar cuándo equilibra la imparcialidad de nodo del clúster:

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

## <a name="abbr-titlevirtual-machinevmabbr-load-balancing-vs-system-center-virtual-machine-manager-dynamic-optimization"></a><abbr title="Máquina virtual">VM</abbr> vs equilibrio de carga. Optimización de System Center Virtual Machine Manager dinámicos
La característica de imparcialidad de nodo, proporciona la funcionalidad en el equipo que va dirigida a las implementaciones sin System Center Virtual Machine Manager (<abbr title="System Center Virtual Machine Manager">SCVMM</abbr>). <abbr title="System Center Virtual Machine Manager">SCVMM</abbr> optimización dinámica es el mecanismo recomendado para el equilibrio de carga de la máquina virtual en el clúster para <abbr title="System Center Virtual Machine Manager">SCVMM</abbr> implementaciones. <abbr title="System Center Virtual Machine Manager">SCVMM</abbr> se deshabilita automáticamente el servidor Windows <abbr title="máquina virtual">VM</abbr> equilibrio de carga cuando se habilita la optimización dinámica.

## <a name="see-also"></a>Vea también
* [Introducción al equilibrio de carga de máquina virtual](vm-load-balancing-overview.md)
* [Agrupación en clústeres de conmutación por error](failover-clustering-overview.md)
* [Introducción a Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
