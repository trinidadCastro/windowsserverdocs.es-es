---
ms.assetid: 5b5bab7a-727b-47ce-8efa-1d37a9639cba
title: "Equilibrio de carga de máquina virtual indicamos"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 6ee092a2e51e2181a2203bc263377c4a8ec60246
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="virtual-machine-load-balancing-deep-dive"></a>Equilibrio de carga de máquina virtual indicamos

> Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Windows Server 2016 presenta la [característica Equilibrio de carga de máquina Virtual](vm-load-balancing-overview.md) para optimizar el uso de los nodos de un clúster de conmutación por error. Este documento describe cómo configurar y controlar <abbr title="máquina virtual">VM</abbr> equilibrio de carga. 

## <a id="heuristics-for-balancing"></a>Heurística equilibrio
<abbr title="Máquina virtual">VM</abbr> equilibrio de carga de máquina Virtual evalúa la carga de un nodo en función de la heurística siguiente:
1. Actual **presión de memoria**: memoria es la restricción de recursos más habitual en un host de Hyper-V
2. <abbr title="Unidad de procesamiento central">CPU</abbr> **utilización** del nodo promediado por una ventana de 5 minutos: mitiga un nodo del clúster convertirse en exceso compromete

## <a id="controlling-aggressiveness-of-balancing"></a>Controlar la agresividad de equilibrio de
La agresividad del equilibrio en función de la heurística de CPU y memoria puede configurarse mediante la por la propiedad común de clúster 'AutoBalancerLevel'. Para controlar la agresividad ejecuta lo siguiente en PowerShell:

```PowerShell
(Get-Cluster).AutoBalancerLevel = <value>
```

| AutoBalancerLevel | Agresividad | Comportamiento |
|-------------------|----------------|----------|
| 1 (predeterminado) | Baja | Mover al host es más del 80% de carga |
| 2 | Mediano | Mover al host es más del 70% de carga |
| 3 | Alto | Mover al host está cargado en más de un 60% | 

![Gráfico de un PowerShell de configuración de la agresividad de equilibrio de](media/vm-load-balancing/detailed-VM-load-balancing-1.jpg)

## <a name="controlling-abbr-titlevirtual-machinevmabbr-load-balancing"></a>Controlar <abbr title="Máquina Virtual">VM</abbr> equilibrio de carga
<abbr title="Máquina virtual">VM</abbr> equilibrio de carga está habilitado de manera predeterminada y cuándo ocurre el equilibrio de carga puede configurarse mediante la propiedad común de clúster 'AutoBalancerMode'. Para controlar cuándo nodo imparcialidad equilibra el clúster:

### <a name="using-failover-cluster-manager"></a>Usar el Administrador de clúster de conmutación por error:
1. Haz clic en el nombre del clúster y selecciona la opción de "Propiedades"  
    ![Gráfico de selección de propiedad para clúster mediante el Administrador de clúster de conmutación por error](media/vm-load-balancing/detailed-VM-load-balancing-2.jpg)

2.  Selecciona el panel "Equilibrado"  
    ![Gráfico de la opción equilibrado mediante el Administrador de clúster de conmutación por error](media/vm-load-balancing/detailed-VM-load-balancing-3.jpg)

### <a name="using-powershell"></a>Uso de PowerShell:
Ejecuta lo siguiente:
```powershell
(Get-Cluster).AutoBalancerMode = <value>
```

|AutoBalancerMode |Comportamiento| 
|:----------------:|:----------:|
|0| Deshabilitado| 
|1| Equilibrio de carga en unirse a un nodo| 
|2 (predeterminado)| Cargar el saldo en unirse a un nodo y cada 30 minutos |

## <a name="abbr-titlevirtual-machinevmabbr-load-balancing-vs-system-center-virtual-machine-manager-dynamic-optimization"></a><abbr title="Máquina virtual">VM</abbr> equilibrio frente a System Center Virtual Machine Manager dinámicos optimización de carga
La función de igualdad de nodo proporciona la funcionalidad en el equipo que está dirigida a las implementaciones sin System Center Virtual Machine Manager (<abbr title="System Center Virtual Machine Manager">SCVMM</abbr>). <abbr title="System Center Virtual Machine Manager">SCVMM</abbr> optimización dinámica es el mecanismo recomendado para equilibrio de carga de la máquina virtual en el clúster por <abbr title="System Center Virtual Machine Manager">SCVMM</abbr> implementaciones. <abbr title="System Center Virtual Machine Manager">SCVMM</abbr> deshabilita automáticamente el servidor de Windows <abbr title="máquina virtual">VM</abbr> cuando se habilita la optimización dinámica de equilibrio de carga.

## <a name="see-also"></a>Consulta también
* [Introducción de equilibrio de carga de máquina virtual](vm-load-balancing-overview.md)
* [Clústeres de conmutación por error](failover-clustering-overview.md)
* [Información general de Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
