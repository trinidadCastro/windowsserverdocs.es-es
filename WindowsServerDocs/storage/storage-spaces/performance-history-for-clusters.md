---
title: Historial de rendimiento de clústeres
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
ms.localizationpriority: medium
ms.openlocfilehash: e013295364ae2951ffe8a963fb61a85d7863f5b6
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86962057"
---
# <a name="performance-history-for-clusters"></a>Historial de rendimiento de clústeres

> Se aplica a: Windows Server 2019

Este subtema del [historial de rendimiento de espacios de almacenamiento directo](performance-history.md) describe el historial de rendimiento recopilado para los clústeres.

No hay ninguna serie que se origine en el nivel de clúster. En su lugar, se agregan las series de servidores, como `clusternode.cpu.usage` , para todos los servidores del clúster. Las series de volumen, como `volume.iops.total` , se agregan para todos los volúmenes del clúster. Y la serie de unidades, como `physicaldisk.size.total` , se agregan para todas las unidades del clúster.

## <a name="usage-in-powershell"></a>Uso en PowerShell

Use el cmdlet [Get-Cluster](/powershell/module/failoverclusters/get-cluster) :

```PowerShell
Get-Cluster | Get-ClusterPerf
```

## <a name="additional-references"></a>Referencias adicionales

- [Historial de rendimiento de Espacios de almacenamiento directo](performance-history.md)
