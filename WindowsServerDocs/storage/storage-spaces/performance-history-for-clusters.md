---
title: Historial de rendimiento de clústeres
ms.author: cosdar
manager: eldenc
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
ms.localizationpriority: medium
ms.openlocfilehash: ea80be97e3940850f9892c50c534c3449fd3ecdb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954672"
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
